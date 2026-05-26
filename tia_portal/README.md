# Projeto TIA Portal — DigitalTwin_Selecao_de_Pecas

Este diretório contém o projeto do **TIA Portal V14** desenvolvido para o controle do Gêmeo Digital da bancada XC243.

## Arquivo Principal

- **`DigitalTwin_Selecao_de_Pecas.ap14`** — Projeto completo do TIA Portal V14 com:
  - CPU virtual: **S7-1511-1 PN** (família S7-1500, para uso com PLCSim Advanced)
  - CPU física: **S7-1200 CPU 1214C DC/DC/DC** + placa de sinal **SB 1232 AQ**
  - Lógica de controle em linguagem **Ladder (LAD)**
  - Estrutura modular com **Funções (FCs)** e **Blocos de Dados (DBs)** segmentados

## Versão do TIA Portal

- **Versão de desenvolvimento:** TIA Portal V14
- **Formato do arquivo:** `.ap14` (compatível apenas com V14 ou superior, com possível upgrade automático)
- ⚠️ Versões anteriores do TIA Portal **não conseguem abrir** este arquivo.

## Como Abrir

1. Abra o TIA Portal V14 (ou versão posterior).
2. Acesse `Project → Open`.
3. Navegue até este diretório e selecione `DigitalTwin_Selecao_de_Pecas.ap14`.
4. Se estiver usando uma versão mais recente que V14, o TIA Portal solicitará uma atualização automática do projeto.

## Estrutura Interna do Projeto

```
DigitalTwin_Selecao_de_Pecas
├── PLC_1 [CPU 1511-1 PN]              ← Configuração para uso com PLCSim Advanced
│   ├── Program blocks
│   │   ├── Main [OB1]
│   │   ├── FC_DEFPEC  (definição do tipo de peça)
│   │   ├── FC_SETORES (lógica de decisão por setor)
│   │   ├── FC_ESTEIRA (controle das esteiras)
│   │   └── DBs        (memórias persistentes)
│   ├── PLC tags        (mapeamento de I/O)
│   └── Watch and force tables
│
└── PLC_2 [CPU 1214C DC/DC/DC]         ← Configuração para hardware físico
    ├── Program blocks (mesmos blocos do PLC_1, com ajustes pontuais)
    ├── PLC tags
    └── Watch and force tables
```

## Adaptações entre as CPUs

A migração da CPU **S7-1511-1 PN** (virtual) para a CPU **S7-1200 CPU 1214C** (física) exigiu apenas **duas adaptações pontuais**, sem alteração no algoritmo de decisão:

| # | Item | S7-1500 (virtual) | S7-1200 (físico) |
|---|---|---|---|
| 1 | Endereço da saída analógica `%QW` | conforme mapeamento da CPU | adequado à SB 1232 |
| 2 | Tipo de dado da referência de velocidade | `Real` (valor 2.0) | `UInt` (valor 27648) |

## Pré-requisitos para Execução

| Componente | Versão Mínima |
|---|---|
| TIA Portal | V14 |
| S7-PLCSim Advanced | V2.0 |
| Sistema Operacional | Windows 10/11 com virtualização habilitada |

Veja o [README principal](../README.md) para o passo a passo completo de execução.
