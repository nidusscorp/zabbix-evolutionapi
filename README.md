# EvolutionAPI Monitoring Template for Zabbix
O template utiliza Descoberta de Baixo Nível (LLD) para identificar e monitorar automaticamente todas as instâncias configuradas.

## 🚀 Visão Geral
O template foi projetado para oferecer visibilidade em tempo real sobre a saúde das conexões de WhatsApp. Ele utiliza um item mestre do tipo **HTTP Agent** para buscar dados de todas as instâncias em uma única requisição, processando-os via itens dependentes para máxima performance.

## ✨ Funcionalidades
* **Auto-discovery (LLD)**: Detecta novas instâncias automaticamente via endpoint `/instance/fetchInstances`.
* **Monitoramento de Status**: Acompanha o `connectionStatus` (open, connecting, close, etc.).
* **Eficiência de Dados**: Utiliza o pré-processamento `Discard unchanged` para economizar espaço no banco de dados quando não há alteração de status.
* **Tratamento de Erros**: Configurado para retornar "unknown" caso a instância não seja encontrada ou suporte falhe.

## 📋 Requisitos
* **Zabbix**: Versão 7.0 ou superior (homologada, mas pode funcionar em outras versões)
* **EvolutionAPI**: Instância ativa com Global API Key configurada.

## ⚙️ Configuração

### 1. Importação
1. Baixe o arquivo `template-evolutionapi.yaml` deste repositório.
2. No Zabbix, vá em **Data collection > Templates**.
3. Clique em **Import** e selecione o arquivo baixado.

### 2. Macros (Obrigatório)
Associe o template ao Host (EvolutionAPI) e configure as seguintes macros em **Macros > Host macros**:

| Macro | Descrição | Exemplo |
| :--- | :--- | :--- |
| `{$EVOLUTIONAPI_URL}` | URL base da sua API | `https://api.seudominio.com` |
| `{$EVOLUTIONAPI_TOKEN}` | Sua Global API Key | `sua_chave_mestra_aqui` |

## 📊 Estrutura Técnica

### Itens
- **get instances**: Coleta o JSON bruto da API.
- **instance {#NAME} : status**: Item dependente que extrai o status específico da instância via JSONPath.

### Triggers
- **instance status {#NAME} is disconnected**: Alerta de severidade **Alta** disparado quando o status da instância for diferente de `open`. Permite fechamento manual.

