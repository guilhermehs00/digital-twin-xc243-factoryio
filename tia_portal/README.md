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


## Pré-requisitos para Execução

| Componente | Versão Mínima |
|---|---|
| TIA Portal | V14 |
| S7-PLCSim Advanced | V6.0 |
| Sistema Operacional | Windows 10/11  |

Veja o [README principal](../README.md) para o passo a passo completo de execução.
