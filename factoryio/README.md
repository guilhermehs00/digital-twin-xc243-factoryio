# Cenário do Factory I/O

Este diretório contém o cenário 3D do Gêmeo Digital construído no Factory I/O.

## Arquivo Principal

- **`Esteira Exsto XC243 - Digital Twin.factoryio`** — Cena completa do Factory I/O reproduzindo a bancada XC243.

## Versão do Factory I/O

- **Versão de desenvolvimento:** 2.5.4
- **Formato do arquivo:** `.factoryio`
- ⚠️ Versões anteriores podem não conseguir abrir o cenário; recomenda-se atualizar para a versão 2.5.4 ou superior.

## Como Abrir

1. Inicie o Factory I/O 2.5.4 ou superior.
2. Acesse `File → Open Scene`.
3. Navegue até este diretório e selecione `Esteira Exsto XC243 - Digital Twin.factoryio`.

## Componentes Principais da Cena

| Componente | Quantidade | Função |
|---|---|---|
| Esteira transportadora | 2 segmentos (2 m + 6 m) | Transporte das peças |
| Sensor difuso (alturas G, M, P) | 3 | Classificação dimensional |
| Sensor capacitivo | 1 | Gatilho de início do ciclo |
| Sensor indutivo | 1 | Detecção de material metálico |
| Sensor difuso (rampas) | 4 | Confirmação de chegada nos setores A, B, C e Despacho |
| Separador por braço pivotante | 3 | Desvio para os setores A, B e C |
| Emissor de peças | 1 | Geração dos itens (peças plásticas e metálicas) |
| Removedor | 4 | Destrói as peças após chegada no setor |

## Configuração do Driver

Para conectar com o S7-PLCSim Advanced em ambiente SIL:

1. Acesse `File → Drivers`.
2. Selecione **Siemens S7-1200/1500** na lista de drivers.
3. No campo **Model**, escolha **S7-1500**.
4. No campo **Host**, insira o IP da instância PLCSim (ex.: `192.168.168.213`).
5. No campo **Network adapter**, selecione **Siemens PLCSIM Virtual Ethernet Adapter**.
6. Clique em **Connect**.

Após a conexão estabelecida, mapeie os tags conforme a tabela de I/O documentada em [`docs/arquitetura.md`](../docs/arquitetura.md).

## Pré-requisitos

| Componente | Versão Mínima |
|---|---|
| Factory I/O | 2.5.4 |
| S7-PLCSim Advanced | V2.0 |
| Sistema Operacional | Windows 10/11 |

Veja o [README principal](../README.md) para o passo a passo completo de execução.
