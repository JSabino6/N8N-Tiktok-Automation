# Case RPA - Automacao de Postagem no TikTok com n8n
Automacao em `n8n` para transformar um processo manual de publicacao em um fluxo semiautomatizado com aprovacao humana.

Video explicativo:https://youtu.be/CRzM7YGxHpI


## Objetivo

Reduzir tempo operacional e padronizar a criacao/publicacao de conteudo para TikTok.

## Fluxo da solucao

1. Recebe video via Telegram (link ou ID do Google Drive).
2. Baixa o arquivo no n8n.
3. Transcreve o audio com IA (`Whisper Large v3 Turbo`).
4. Gera legenda com IA (`Llama 3.3 70B Versatile 128k`).
5. Envia preview para aprovacao humana no Telegram.
6. Publica no TikTok quando aprovado.
7. Dispara alerta em caso de falha.

## Stack

- n8n
- Docker
- Telegram Bot API
- Google Drive
- Groq API
- TikTok API
- ngrok (para webhook em ambiente local)

## Estrutura do repositorio

- `Desafio RPA TikTok - Bot Dinamico Master (2).json`: workflow exportado do n8n
- `docker-compose.n8n.yml`: ambiente local com Docker
- `ENTREGA_CASE/`: documentacao de implementacao, custos, riscos e melhorias

## Requisitos

- Docker
- Bot do Telegram configurado
- Conta no TikTok Developers
- Chave da API Groq
- ngrok

## Como executar

1. Suba o n8n:

```bash
docker compose -f docker-compose.n8n.yml up -d
```

2. Exponha a porta 5678 (Ou de sua preferencia):

```bash
ngrok http 5678
```

3. Atualize `WEBHOOK_URL` com a URL publica atual do ngrok.
4. Importe o workflow `.json` no n8n.
5. Configure as credenciais de Telegram, Groq e TikTok.

## Variaveis de ambiente

Exemplo:

```env
WEBHOOK_URL=https://SEU-SUBDOMINIO.ngrok-free.app
TIKTOK_ACCESS_TOKEN=
TIKTOK_CLIENT_KEY=
TIKTOK_CLIENT_SECRET=
TIKTOK_REDIRECT_URI=
TIKTOK_SCOPE=video.publish,user.info.basic
N8N_BLOCK_SVG_AND_HTML_IN_EXPRESSIONS=false
N8N_JS_CREDENTIALS_ACCESS_ENABLED=true
```

## Entregaveis do case

- Workflow funcional no n8n
- Plano de implementacao em producao
- Cronograma de integracoes
- Planejamento de custos
- Riscos e mitigacoes
- Roadmap de melhorias
