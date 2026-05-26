# Instância do S7-PLCSim Advanced

Este diretório contém a **instância exportada** do CLP virtual utilizado na Fase I (Comissionamento Virtual) deste projeto, emulada pelo **SIMATIC S7-PLCSim Advanced**.

## Arquivo Principal

- **`S7-1511-1_PN/`** — Pasta contendo todos os arquivos da instância do CLP virtual, incluindo dados retentivos, configuração de hardware simulado e \textit{web apps} do TIA Portal.

## Versão do S7-PLCSim Advanced

- **Versão de desenvolvimento:** V6.0
- **CPU emulada:** SIMATIC S7-1511-1 PN (família S7-1500)
- **IP configurado:** `192.168.168.213`

## Estrutura da Instância

```
S7-1511-1_PN/
├── retain.pms                  # Dados retentivos da CPU
├── sim_hwdb.ini                # Base de dados de hardware simulado
└── SIMATIC_MC/
    ├── RData/                  # Dados retentivos por subsistema
    │   ├── retain_motion
    │   ├── retain_aslog
    │   ├── retain_system
    │   └── retain_user
    └── s7p.web.apps/           # Web apps do TIA Portal
```

## Como Importar a Instância

### Pré-requisitos

| Componente | Versão Mínima |
|---|---|
| S7-PLCSim Advanced | V6.0 |
| Sistema Operacional | Windows 10/11 |
| Virtualização | Habilitada na BIOS/UEFI |

### Passo a Passo

1. **Copie a pasta `S7-1511-1_PN/`** deste diretório para o local padrão de instâncias do PLCSim Advanced no seu computador.
   - Caminho típico no Windows:
     ```
     C:\Users\<seu_usuário>\Documents\Siemens\Simatic\Simulation\Instances\
     ```

2. **Abra o Control Panel do S7-PLCSim Advanced**.

3. **Verifique se a instância aparece na lista** "Instances" no painel principal. Se não aparecer, reinicie o S7-PLCSim Advanced e tente novamente.

4. **Selecione a interface de rede** como **PLCSIM Virtual Eth. Adapter** (não use TCP/IP nativo do sistema).

5. **Clique em Start** para iniciar a instância. O LED verde indica que o CLP virtual está em execução e pronto para receber download do programa pelo TIA Portal.

### Configuração de Rede

A instância foi configurada com o IP `192.168.168.213` para comunicação com o Factory I/O. Se o seu adaptador virtual estiver em outra sub-rede, ajuste o IP da instância pelo Control Panel do PLCSim Advanced antes de iniciar.

> **Importante:** o Factory I/O também precisa estar configurado para apontar para o mesmo IP. Veja as instruções no [README do Factory I/O](../factoryio/README.md).

## Download do Programa

Após iniciar a instância:

1. Abra o projeto do TIA Portal localizado em [`../tia_portal/DigitalTwin_Selecao_de_Pecas.ap14`](../tia_portal/).
2. Selecione a CPU **PLCSim_Advanced [CPU 1511-1 PN]** na árvore do projeto.
3. Clique em `Download to device → Hardware and software (only changes)`.
4. Selecione a instância em execução no PLCSim Advanced como destino.
5. Confirme o download e coloque a CPU em **RUN**.

## Por que esta instância está no repositório?

Distribuir a instância pré-configurada economiza tempo de quem for reproduzir o projeto: não é necessário recriar a configuração de hardware, definir IP manualmente nem ajustar parâmetros de simulação. Basta importar, iniciar e fazer o download do programa.

> Veja o [README principal](../README.md) para o passo a passo completo de execução das duas fases do projeto.
