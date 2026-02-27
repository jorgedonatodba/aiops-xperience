# Agente AIOps - Backup de Workflow

Este diretório contém o backup do workflow **Agente AIOps** exportado da instância n8n.

## Visão Geral

O workflow **Agente AIOps** é uma solução de automação para monitoramento e diagnóstico inteligente de clusters Kubernetes. Ele identifica eventos de erro, utiliza inteligência artificial para diagnosticar a causa raiz e notifica as equipes responsáveis.

## Arquitetura do Workflow

O fluxo de trabalho segue as seguintes etapas:

1.  **Gatilho Manual**: Iniciado manualmente para verificação do cluster.
2.  **Coleta de Eventos**: Realiza uma requisição HTTP para um servidor MCP (`k8s-mcp`) para buscar eventos de todos os namespaces.
3.  **Filtragem Inteligente**: Um nó de código JavaScript filtra apenas eventos do tipo `Warning` ocorridos nos últimos 5 minutos.
4.  **Deduplicação e Agrupamento**: Agrupa eventos similares para evitar alertas redundantes e prepara um prompt detalhado.
5.  **Diagnóstico com IA**:
    *   Utiliza um **AI Agent** (LangChain) alimentado pelo modelo **Google Gemini**.
    *   O agente tem acesso a ferramentas via **MCP (Model Context Protocol)**: `kubectl_get`, `kubectl_describe` e `kubectl_logs`.
    *   A IA investiga os recursos afetados, analisa logs e identifica a causa raiz dos problemas.
6.  **Abertura de Issue**: Cria automaticamente uma Issue no GitHub com o relatório detalhado do diagnóstico.
7.  **Notificação**: Envia uma mensagem para um canal do Discord com o link da Issue criada no GitHub.

## Detalhes Técnicos

*   **Arquivo**: `agente-aiops.json`
*   **ID do Workflow**: `uc69w56GhU486GM9`
*   **Modelos de IA**: Gemini 1.5 Pro (via Google Gemini Chat Model)
*   **Integrações**: GitHub, Discord, HTTP Request (MCP Client)

## Como Restaurar

Para restaurar este workflow no n8n:
1. Abra o n8n.
2. Clique em **Workflows** > **Import from File**.
3. Selecione o arquivo `agente-aiops.json`.
4. Configure as credenciais necessárias para o GitHub, Discord e Google Gemini API.
