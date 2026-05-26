# Arquitetura Técnica do Gêmeo Digital

Este documento descreve em detalhes a arquitetura técnica do projeto, complementando o [README principal](../README.md).

## Visão Geral das Fases

O projeto é organizado em duas fases distintas de validação:

- **Fase I — Comissionamento Virtual (SIL):** o programa Ladder é executado em uma CPU emulada pelo S7-PLCSim Advanced, que se comunica com o cenário 3D do Factory I/O. Toda a depuração da lógica é feita sem acesso ao hardware físico.
- **Fase II — Implementação Física:** o mesmo programa Ladder (com adaptações mínimas) é descarregado em um CLP S7-1200 conectado à bancada real XC243.

## Comunicação na Fase I (SIL)

```
┌─────────────────┐   Protocolo S7    ┌───────────────────────┐
│   Factory I/O   │ ◄────────────────►│  S7-PLCSim Advanced   │
│  (Driver S7-    │   sobre TCP/IP    │  (CPU 1511-1 PN       │
│   1200/1500)    │   via Virtual     │   emulada)            │
│                 │   Ethernet Adapter│                       │
└─────────────────┘                   └───────────┬───────────┘
                                                  │
                                                  │ Download de programa
                                                  │ + Watch Table
                                                  ▼
                                      ┌───────────────────────┐
                                      │      TIA Portal V14    │
                                      │      (Engenharia)      │
                                      └───────────────────────┘
```

**Adaptador de rede:** `Siemens PLCSIM Virtual Ethernet Adapter`
**Protocolo:** S7 Communication sobre TCP/IP (porta 102)
**IP típico da instância PLCSim:** `192.168.168.213`

## Comunicação na Fase II (Hardware Real)

```
┌─────────────────┐   Fiação elétrica  ┌───────────────────────┐
│  Bancada XC243  │ ◄────────────────►│   CLP S7-1200          │
│  (sensores e    │   (24 VDC, %I/%Q) │   CPU 1214C DC/DC/DC   │
│   atuadores)    │                    │   + SB 1232 AQ         │
└─────────────────┘                    └───────────┬───────────┘
                                                   │
                                                   │ PROFINET (RJ45)
                                                   ▼
                                       ┌───────────────────────┐
                                       │      TIA Portal V14    │
                                       │ (Download + Monitor)   │
                                       └───────────────────────┘
```

> Importante: na Fase II, o Factory I/O **não participa** da execução. A bancada física substitui o modelo virtual, comunicando-se diretamente com o CLP pela fiação elétrica.

## Mapeamento de I/O

### Entradas Digitais

| Tag (Factory I/O) | Tag (Bancada) | Endereço Virtual | Endereço Físico | Função |
|---|---|---|---|---|
| Diffuse Sensor G | F3 | %I20.0 | %I0.0 | Sensor de altura — maior |
| Diffuse Sensor M | F2 | %I20.1 | %I0.1 | Sensor de altura — média |
| Diffuse Sensor P | F1 | %I20.2 | %I0.2 | Sensor de altura — baixa |
| Capacitive Sensor 0 | C1 | %I20.3 | %I0.3 | Sensor capacitivo |
| Inductive Sensor 0 | I1 | %I20.4 | %I0.4 | Sensor indutivo (metal) |
| Sensor Setor A | FC1 | %I20.5 | %I0.5 | Confirmação chegada Setor A |
| Sensor Setor B | FC2 | %I20.6 | %I0.6 | Confirmação chegada Setor B |
| Sensor Setor C | FC3 | %I20.7 | %I0.7 | Confirmação chegada Setor C |
| Sensor Setor D | FC4 | %I21.0 | %I1.0 | Confirmação chegada Despacho |

### Saídas Digitais e Analógicas

| Tag (Factory I/O) | Tag (Bancada) | Endereço Virtual | Endereço Físico | Função |
|---|---|---|---|---|
| Pistão 1 | V1 | %Q20.0 | %Q0.0 | Atuador Setor A (avanço) |
| Pistão 2 | V2 | %Q20.1 | %Q0.1 | Atuador Setor B (avanço) |
| Pistão 3 | V3 | %Q20.2 | %Q0.2 | Atuador Setor C (avanço) |
| — | V4 | — | %Q0.3 | Recuo do atuador Setor C |
| Esteira 1 | Control Driver | %QD64 | %QW64 | Referência de velocidade |
| Esteira 2 | — | %QD68 | — | (somente na cena virtual) |

## Estratégia de Classificação Dimensional

A bancada XC243 dispõe **três sensores ópticos de altura** dispostos **em sequência** ao longo da esteira (médio, pequeno e grande). Como os sensores são reflexivos e a peça está em movimento, quando ela atinge o sensor grande já saiu do feixe do sensor médio — a combinação "médio ativo + grande ativo" nunca é observada simultaneamente sem tratamento.

**Solução implementada:** um temporizador **TOF (Timer Off-Delay) de 0,7 s** prolonga o sinal do sensor médio o suficiente para coexistir com o sinal do sensor grande, permitindo que a lógica de classificação avalie a combinação correta:

- Médio ativo (prolongado pelo TOF) + Grande inativo → **peça média**
- Médio ativo (prolongado pelo TOF) + Grande ativo → **peça grande**
- Apenas Pequeno ativo → **peça pequena**
- Indutivo ativo → **peça metálica** (combinado com altura para refinar a classe)

## Setores de Destino

| Tipo de Peça | Setor de Destino | Justificativa |
|---|---|---|
| Metálica — Pequena | A | Primeiro ponto de desvio (prioridade ao metal) |
| Plástica — Pequena | B | Segundo ponto de desvio |
| Plástica — Grande | C | Terceiro ponto de desvio |
| Plástica — Média | Despacho | Não atende aos critérios anteriores; segue para o final |

## Arquitetura de Software

O programa Ladder é estruturado de forma modular, seguindo princípios de manutenibilidade e portabilidade:

- **OB1 (Main):** orquestra a chamada das funções a cada ciclo de varredura.
- **FCs (Funções):**
  - `FC_DEFPEC` — define o tipo de peça com base nos sensores
  - `FC_SETORES` — implementa a lógica de decisão de desvio por setor
  - `FC_ESTEIRA` — controla as esteiras e a referência de velocidade
- **DBs (Blocos de Dados):** armazenam estados persistentes (flags de destino, contadores, temporizadores).

Essa arquitetura permitiu a migração entre famílias de CLP (S7-1500 → S7-1200) com apenas dois ajustes pontuais.
