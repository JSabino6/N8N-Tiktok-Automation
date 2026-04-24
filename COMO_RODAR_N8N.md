# Como rodar este projeto no n8n (Docker + ngrok)

## Estado atual validado

- Container `n8n` esta ativo na porta `5678`.
- Túnel ngrok atual esta ativo e apontando para `localhost:5678`.
- URL pública atual: `https://a750-45-181-82-144.ngrok-free.app`

Observacao: URL do ngrok muda quando voce reinicia o ngrok.

## 1) Subir n8n com persistencia (recomendado)

No PowerShell, na pasta do projeto:

```powershell
cd C:\Users\joaov\OneDrive\Documentos\N8N
docker compose -f docker-compose.n8n.yml up -d
```

Depois abra:

- `http://localhost:5678`

## 2) Subir/reiniciar ngrok na porta certa

```powershell
cd C:\Users\joaov\OneDrive\Documentos\N8N
.\ngrok.exe http 5678
```

Copie a URL `https://...ngrok-free.app` mostrada pelo ngrok.

## 3) Atualizar WEBHOOK_URL no .env

Edite `C:\Users\joaov\OneDrive\Documentos\N8N\.env` e troque:

```env
WEBHOOK_URL=https://SUA-URL-ATUAL-DO-NGROK.ngrok-free.app
```

Depois reinicie o n8n:

```powershell
docker compose -f docker-compose.n8n.yml restart n8n
```

## 4) Importar seu workflow

No n8n:

1. Clique em `New` ou `Import from file`.
2. Importe o arquivo:
   - `C:\Users\joaov\OneDrive\Documentos\N8N\Desafio RPA TikTok - Bot Dinâmico Master (2).json`
3. Configure as credenciais:
   - Telegram
   - Header Auth (Groq)
   - TikTok token no `.env` (`TIKTOK_ACCESS_TOKEN`)

## 5) Teste rapido

1. Abra seu bot no Telegram.
2. Envie o link/ID do arquivo no Google Drive.
3. Confira:
   - transcricao;
   - legenda sugerida;
   - botoes de aprovar/recusar.
4. Ao aprovar, valide se o upload no TikTok retorna sucesso.

## Problema que voce teve (diagnostico)

Seu link ngrok antigo apontava para `localhost:8000`, por isso nao abria o n8n.

O n8n usa `5678`, entao o tunel precisa ser `ngrok http 5678`.
