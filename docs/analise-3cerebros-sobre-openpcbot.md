# Análise: implementar o `3cerebros` no `openpcbot`

> Análise ancorada na arquitetura **real** do `openpcbot` (lida em 2026-06-20:
> `CLAUDE.md`, `ARQUITETURA_SECOND_BRAIN.md`, árvore do projeto).
> Compara com o esboço sobre o intelecto ([[esboco-3cerebros-sobre-intelecto]]).
> Gating: [[ideia-3cerebros-portavel]].

## TL;DR
**O openpcbot é o melhor hospedeiro dos dois.** Ele já entrega, prontos, **4 dos 5
recursos** que o gating apontou como necessários — e **roda no Linux** (a sua
máquina), ao contrário do intelecto (macOS/launchd). Implementar o `3cerebros` aqui
é, na prática: **(1) trocar o vault plano pelos 3 cérebros** + **(2) adicionar a
política de automação** (o único gap real). O resto já existe.

## O que o openpcbot JÁ é (verificado no código)
Bot Telegram em **TypeScript** que usa o **Claude Code como motor** (escreve no vault
via `fs`, lê/sintetiza delegando ao Claude). Já tem:

| Recurso do gating | No openpcbot (evidência) | Status |
|---|---|---|
| **Modo autônomo / cron** | `dist/schedule-cli.js create "PROMPT" "CRON"` — tarefas agendadas que rodam o Claude | ✅ **existe** |
| **Painel / dashboard** | `dist/dashboard.js` + `dashboard-html.js` (+ assets de preview) | ✅ **existe** |
| **Memória estruturada** | SQLite `store/openpcbot.db`, tabela `memories(content, sector, salience, …)` + sessions + token_usage | ✅ **existe** (já com setores!) |
| **Conectores** | Telegram nativo, **WhatsApp bridge**, voz (inemaVOX/Whisper), Gmail, Google Calendar, Skool, YouTube/TikTok | ✅ **muitos** |
| **Multi-agente** | `agents/{comms,content,ops,research}/` (agent.yaml + CLAUDE.md) + `_template` | ✅ **existe** |
| **Vault second-brain** | `vault-template/` (inbox/daily/projects/research/archive + memory.md) + skills `daily`/`tldr`/`file-intel`/`vault-setup` | ✅ (mas é o **vault plano**, não 3 cérebros) |
| **Sem comando (triggers naturais)** | regex em `bot.ts` ("guarda isso", "salva no brain") salva direto no inbox | ✅ parcial |
| **Roda no Linux** | serviço persistente Mac/Linux; `cd /home/nmaldaner/projetos/openpcbot` | ✅ |
| **Política de automação** | — não existe (tem cron + execução, mas sem camada de permissões) | ❌ **GAP** |
| **Separação em 3 cérebros** | — o vault é plano (inbox/daily/projects/research/archive) | ❌ **a fazer** |

## O que falta — e é só isto
1. **3 cérebros:** hoje o vault é plano. Migrar `inbox/daily/projects/research/archive`
   → `0-inbox/ 1-projeto/ 2-self/ 3-conhecimento/`. A tabela `memories` **já tem a
   coluna `sector`** — dá pra etiquetar cada memória por cérebro sem mudar schema.
2. **Política de automação:** o diferencial do gating. openpcbot já age sozinho (cron +
   triggers), mas **sem permissões** — exatamente o risco. Adicionar `POLICY.md` +
   um gate que toda ação autônoma consulta. É a peça nova de verdade.

## Mapa: recurso do 3cerebros → onde encaixa no openpcbot

| Recurso | Onde encaixa | Reusar / Add |
|---|---|---|
| Cérebro **Self** | `CLAUDE.md` (personalidade/regras já existe) + `2-self/values.md` | reusar + add |
| Cérebro **Conhecimento** | `memories`(sector) + vault `3-conhecimento/` + `file-intel` | reusar + reorganizar |
| Cérebro **Projeto** | vault `1-projeto/` (daily/decisions/projects) + `memories` | reorganizar |
| **Triagem/nota sem comando** | regex de trigger em `bot.ts` (já existe) + cron classificando inbox | **add** (rota pro cérebro certo) |
| **Modo autônomo** | `schedule-cli` (já existe) → jobs de triagem/briefing/saúde | **reusar** |
| **Conectores** | Telegram/WhatsApp/voz/Gmail/Calendar (já existem) → caem no `0-inbox/` | **reusar** |
| **Política de automação** | `CLAUDE.md` + novo `POLICY.md` + gate antes de cada ação/cron | **add** ← novidade |
| **Painel operacional** | `dashboard.js` (já existe) → adicionar visão dos 3 cérebros + fila de aprovação | **estender** |
| **Multi-agente** | `agents/{comms,content,ops,research}` → workers operando os cérebros sob política | **reaproveitar** |

## O ângulo multi-agente (exclusivo do openpcbot)
O openpcbot já tem **agentes especializados** (comms, content, ops, research). No
`3cerebros` eles viram os **trabalhadores autônomos** que mantêm os cérebros:
- `research` → destila fontes pro **Conhecimento** (`/nota`)
- `ops` → roteia inbox, arquiva, roda saúde (**Projeto**)
- `comms` → conectores de mensageria (ingestão pro `0-inbox/`)
- `content` → usa o cérebro pra produzir (ponte com o portal/INEMA)
Cada um **governado pela mesma POLICY**. Isso é algo que nem o intelecto nem os
projetos do gating têm.

## openpcbot vs intelecto — qual hospedeiro?

| Critério | **openpcbot** | **intelecto** |
|---|---|---|
| Linguagem | TypeScript (mais código) | Python (~3k linhas, enxuto) |
| Já tem autonomia (cron) | ✅ sim | ❌ (roadmap) |
| Já tem dashboard | ✅ sim | ❌ (filosofia "sem web") |
| Já tem conectores | ✅ muitos | só Telegram |
| Já tem multi-agente | ✅ sim | ❌ |
| Memória | SQLite com sector/salience | SQLite FTS5/BM25 |
| Roda no Linux | ✅ sim | ❌ (macOS launchd) |
| Simplicidade / clareza | menor (mais peças) | **maior** (limpo, interfaces abstratas) |
| Esforço pro 3cerebros | **menor** (só vault+policy) | maior (construir autonomia+painel+conectores) |

**Recomendação:** para um `3cerebros` **funcional rápido**, o **openpcbot** é o caminho
— já tem quase tudo e roda no seu Linux; falta só os 3 cérebros + a política. O
**intelecto** é a base mais limpa se o objetivo for um produto **enxuto e portável**
pra distribuir (menos peças, código legível). Não são excludentes: o **formato de
cérebro** (pasta `.md` + SQLite) pode ser **o mesmo** nos dois, com o `cerebro-inema`
(Claude Code) como terceiro frontend. Um cérebro, três frontends.

## Plano em fases (openpcbot)
- **F0 — vault → 3 cérebros:** migrar `vault-template/` e o `vault-setup` pra estrutura
  dos 3 cérebros; etiquetar `memories.sector` por cérebro. Instalar as skills
  `triagem`/`nota`/`socratica` (do `cerebro-inema`) em `~/.claude/skills/`.
- **F1 — política (o diferencial):** `POLICY.md` + gate consultado por toda ação
  autônoma (cron + triggers) + fila de aprovação no Telegram.
- **F2 — autonomia governada:** registrar jobs no `schedule-cli` (triagem da inbox,
  briefing diário, saúde do cérebro), todos sob POLICY.
- **F3 — painel:** estender `dashboard.js` com a visão dos 3 cérebros + o que está
  pendente de aprovação + saúde (notas órfãs, inbox acumulando).
- **F4 — multi-agente:** ligar comms/content/ops/research como workers dos cérebros.

## Perguntas em aberto
1. **Host único ou os dois?** Voto: prototipar no **openpcbot** (rápido, Linux), manter
   o **formato de cérebro** compatível com intelecto e cerebro-inema.
2. **`memories.sector`** já existe — usar pra marcar cérebro (projeto/self/conhecimento)
   ou criar coluna nova? (provável: reusar `sector`).
3. **Política x triggers naturais:** hoje "guarda isso" salva direto. Sob POLICY isso
   continua livre (baixo risco), mas enviar email/postar passa a pedir confirmação.
4. **Quanto da autonomia herda do cron atual** sem reescrever? (provável: muito).
