# Plano de Acao para Implementacao em Producao

## Projeto

Automacao de criacao e publicacao de conteudo para TikTok com n8n, com etapa de aprovacao humana via Telegram.

## Objetivo de negocio

Reduzir tempo operacional na criacao e publicacao de videos, padronizar qualidade de legenda e aumentar a capacidade de publicacao sem crescimento proporcional da equipe.

## Escopo da solucao

1. Receber video via Telegram (link/ID do Google Drive).
2. Baixar o arquivo no n8n.
3. Transcrever audio com Whisper.
4. Gerar legenda com Llama.
5. Enviar preview para aprovacao humana no Telegram.
6. Publicar no TikTok quando aprovado.
7. Notificar erros operacionais.

## Tecnologias utilizadas

- n8n (orquestracao low-code)
- Docker (execucao e deploy)
- ngrok (webhook em ambiente de teste)
- Telegram Bot API (interface operacional e aprovacao)
- Google Drive (origem de video)
- Groq API:
  - Whisper Large v3 Turbo (transcricao)
  - Llama 3.3 70B Versatile 128k (legenda)
- TikTok API (publicacao)
- Hetzner Cloud (hospedagem recomendada para producao)

## Arquitetura resumida

- Entrada: Telegram Trigger no n8n
- Processamento: download, transcricao, geracao de legenda
- Governanca: aprovacao humana obrigatoria
- Saida: upload/publicacao no TikTok
- Observabilidade: alerta de erro via Telegram

## Plano de implementacao em producao

### Fase 1 - Preparacao de infraestrutura (D1-D2)

Atividades:

- Provisionar VM na Hetzner Cloud (CAX21 recomendado).
- Subir n8n com Docker Compose e volume persistente.
- Configurar dominio/subdominio e HTTPS.
- Configurar variaveis de ambiente e secrets.
- Definir conta tecnica para operacao.

Entregaveis:

- Ambiente de producao ativo.
- Endpoint publico para webhooks.
- Backup basico de volume e configuracao.

### Fase 2 - Integracoes e seguranca (D3-D4)

Atividades:

- Configurar Telegram Bot e chat de operacao.
- Configurar credenciais Groq e TikTok.
- Configurar OAuth/token do TikTok com rotina de renovacao.
- Importar workflow definitivo no n8n.
- Revisar regras de acesso e segregacao de credenciais.

Entregaveis:

- Fluxo integrado com todas as APIs.
- Credenciais isoladas e documentadas.

### Fase 3 - Homologacao controlada (D5-D7)

Atividades:

- Testar com lote de 10 a 20 videos.
- Validar tempos de processamento por etapa.
- Validar legibilidade e qualidade de legendas.
- Validar publicacao no TikTok e retorno de status.
- Validar cenarios de erro e alertas.

Entregaveis:

- Relatorio de homologacao.
- Lista de ajustes finais.

### Fase 4 - Piloto operacional (Semana 2)

Atividades:

- Operar com pequeno grupo de usuarios.
- Medir volume, tempo economizado e taxa de falha.
- Ajustar mensagens de aprovacao e tratamento de erro.

Entregaveis:

- KPI inicial do piloto.
- Go/No-Go para producao plena.

### Fase 5 - Producao plena e estabilizacao (Semana 3)

Atividades:

- Ativar uso oficial para o processo de conteudo.
- Definir rotina diaria de acompanhamento.
- Definir responsavel por incidentes e renovacao de token.
- Fechar documentacao de operacao.

Entregaveis:

- Operacao em producao.
- Manual rapido de suporte.

## Cronograma de integracoes (macro)

- Semana 1:
  - Infra + setup n8n + secrets
  - Integracao Telegram, Groq e TikTok
  - Homologacao tecnica
- Semana 2:
  - Piloto com usuarios reais
  - Ajustes de qualidade e estabilidade
- Semana 3:
  - Entrada oficial em producao
  - Ajustes finos e fechamento de governanca

## Planejamento de custos

## Premissas

- 1 minuto medio de audio por video.
- 1 chamada de transcricao por video.
- 1 chamada de LLM por video.
- Telegram sem custo.
- Hospedagem em Hetzner CAX21.

## Custos de IA informados

- Llama 3.3 70B Versatile 128k:
  - input: USD 0.59 / 1M tokens
  - output: USD 0.79 / 1M tokens
- Whisper Large v3 Turbo:
  - USD 0.04 (considerando por hora de audio)

## Custo estimado por video

- LLM (1200 in + 120 out): ~USD 0.00080
- Whisper (1 min medio): ~USD 0.00067
- Total IA por video: ~USD 0.00147

## Custo mensal estimado

- 100 videos/mes:
  - IA: ~USD 0.15
  - Infra Hetzner CAX21: ~USD 9.49
  - Total: ~USD 9.64/mes

- 500 videos/mes:
  - IA: ~USD 0.73
  - Infra Hetzner CAX21: ~USD 9.49
  - Total: ~USD 10.22/mes

- 1000 videos/mes:
  - IA: ~USD 1.47
  - Infra Hetzner CAX21: ~USD 9.49
  - Total: ~USD 10.96/mes

Observacao: os custos variam conforme duracao media dos videos, volume mensal e politica de precos das APIs.

## Possiveis problemas e desafios

## Tecnicos

- Erros de autenticacao/expiracao de token TikTok.
- Falha de download do Google Drive por permissao.
- Timeouts para videos maiores.
- Falhas intermitentes de API externa.
- Mudancas na API do TikTok ou na politica de escopos.

## Operacionais

- Postagem duplicada sem controle de idempotencia.
- Dependencia de um unico aprovador.
- Ausencia de trilha de auditoria estruturada.
- Crescimento de volume sem fila/processamento em lote.

## Qualidade de conteudo

- Legenda fora do tom da marca.
- Excesso de hashtags ou formato inconsistente.
- Necessidade de revisao editorial para campanhas sensiveis.

## Mitigacoes recomendadas

- Implementar renovacao de token e alerta preventivo.
- Persistir historico (status, publish_id, erro, aprovador).
- Implementar controle de duplicidade por driveId/hash.
- Adicionar retries com backoff para falhas transientes.
- Definir checklist editorial para aprovacao.
- Evoluir para dashboard de operacao e indicadores.

## Indicadores de sucesso (KPIs)

- Tempo medio por postagem (antes vs depois).
- Taxa de aprovacao na primeira sugestao de legenda.
- Taxa de falha por etapa do fluxo.
- SLA de resolucao de incidentes.
- Custo medio por video publicado.

## Roadmap de melhorias (pos-producao)

1. Integracao nativa com Google Drive API.
2. Banco de dados para trilha de auditoria.
3. Geracao de multiplas opcoes de legenda.
4. Recomendacao de horario de postagem.
5. Suporte a lote de videos com fila.
6. Dashboard de performance operacional.

## Conclusao

A automacao proposta atende ao objetivo de ganho de eficiencia operacional, mantendo controle humano antes da publicacao. O plano acima permite implementacao em ondas curtas, com baixo custo inicial, risco controlado e caminho claro de evolucao para producao robusta.
