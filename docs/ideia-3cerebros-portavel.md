# Ideia para pensar: projeto `3cerebros` — portátil, autônomo, com painel

> Status: **ideia / parking lot**. Não é pra construir agora — é um ponto pra
> pensarmos. Evolução do `cerebro-inema` (que hoje é Claude-Code-first e por
> comando) para algo **portátil entre runtimes, autônomo e monitorável**.
> Capturado: 2026-06-20.

## A semente (o que o Nei pediu)
Um projeto novo, talvez `3cerebros`, que:
1. seja **portado pra rodar em vários runtimes**: Claude Code, Codex, e agentes
   como **Hermes, AgentZero, OpenClaw, Intelecto**…
2. tenha **dicas de instalar e de usar** (por runtime);
3. **funcione sem comandos** — em vez de você disparar `/triagem`, `/nota` etc.,
   ele **se autogerencia**;
4. para isso, faça uma **entrevista** que define **o que pode e o que não pode
   ser automatizado**;
5. tenha um **painel pra monitorar** o que tem dentro dele;
6. tenha **importadores de dados** pras bases: e-mail, Telegram, WhatsApp,
   export do ChatGPT, redes sociais, e outros docs.

## ⚠️ ANTES de construir: os sistemas já não fazem isso? (gating)

Regra do Nei: **checar se os sistemas existentes já têm esses recursos** antes de
construir. Pesquisa feita (3 agentes web, 2026-06-20). Legenda de confiança:
**✅ verificado** (evidência/observação direta) · **⚠️ alegação web não confirmada**
(número/data veio de agente, tratar com ceticismo). Correção: `zubair-trabzada/brain-map`
**existe** (clonado nesta sessão, 16⭐) — um agente disse que não; ignore.

### Matriz A — projetos de second-brain (com evidência/URL)

| Projeto | Autônomo | Conectores | Painel | Cross-runtime | Entrevista→política |
|---|---|---|---|---|---|
| khoj-ai/khoj | parcial (cron→email) | parcial (PDF/Notion/GitHub) | parcial (admin Django) | **não** (é o runtime) | não |
| AgriciDaniel/claude-obsidian | parcial (`/autoresearch`, auto-commit 15min) | parcial (drop + Web Clipper) | **sim** (`dashboard.base`/Dataview) | sim (exp.: Codex/Cursor/Gemini) | não ("what is this vault for?") |
| SamurAIGPT/llm-wiki-agent | não | parcial (markitdown: PDF/DOCX…) | não (`graph.html` estático) | **sim** (Claude/Codex/Gemini/OpenCode) | não |
| huytieu/COG-second-brain | não (17 skills manuais) | parcial (Linear/GitHub/Jira/Slack/Notion) | parcial (Tasks) | **sim** (6 surfaces) | **parcial** (`/onboarding` → integrações) |
| henrydaum/second-brain | **sim** (cron Timekeeper + watcher) | parcial (Telegram nativo, OCR, whisper) | parcial (`/debug`, heartbeat) | sim (AGENTS.md) | parcial (`/setup`, limites hardcoded) |
| eugeniughelbur/obsidian-second-brain | **sim** (4 cron + PostCompact, "mantém enquanto você dorme") | parcial (Telegram, Calendar, X, YouTube) | parcial (`/obsidian-health`) | **sim** ("one codebase, four platforms") | não |

### Matriz B — runtimes/agentes (⚠️ vários números/datas não confirmados)

| Runtime | Memória nativa | Autônomo (cron/daemon) | Mensageria nativa | MCP | Acoplar skills .md |
|---|---|---|---|---|---|
| Claude Code | parcial (arquivos+tasks) | sim (tasks+hooks) | não (via MCP) | ✅ sim | ✅ excelente (padrão SKILL.md) |
| Codex CLI | parcial (AGENTS.md) | parcial (modos approval) | não | ✅ sim | parcial (AGENTS.md) |
| Hermes ⚠️ | sim (alegado) | sim (cron) | sim (TG/WA/Discord…) | sim | sim |
| agent-zero ⚠️ | sim (vetorial) | sim (multi-agente Docker) | parcial (plugin) | sim | sim (SKILL.md) |
| OpenClaw ⚠️ | parcial (plugins) | sim (daemon, computer-use) | sim (30+ plataformas) | sim | sim (registry) |
| **Intelecto** = `inematds/intelecto` ⚠️ | sim (SQLite FTS5 + SOUL/AGENTS/USER.md) | não | sim (Telegram) | ambíguo | ✅ excelente (já é .md-driven) — **é seu próprio projeto** |

### Infra pronta pra REUSAR (não construir)
- **Conectores via MCP:** Gmail (`GongRzhe/Gmail-MCP-Server`), Telegram (`chigwell/telegram-mcp`),
  WhatsApp (`lharries/whatsapp-mcp` — ⚠️ bridge não-oficial, risco de ban), redes sociais (**Postiz**, OSS).
- **Importar exports → Markdown:** ChatGPT (`gavi/chatgpt-markdown`, `Nexus AI Chat Importer`),
  Telegram (`tg2obsidian`), WhatsApp (plugin Obsidian). ChatGPT export = `conversations.json` (árvore).
- **Portabilidade:** **Agent Skills spec** (agentskills.io, padrão aberto Anthropic) — mesma skill em ~20 runtimes;
  vários projetos já fazem via `AGENTS.md` + `build.sh --platform`.
- **Painel base:** `brain-map` (grafo + timeline + click-to-inspect) e `graphify` (grafo queryável + MCP).

### Veredito (responde "os sistemas já não fazem isso?")
**Já fazem — quase tudo.** Dos 5 recursos:
- **Portabilidade** → ✅ **resolvido** (Agent Skills spec + AGENTS.md). Não construir; seguir o padrão.
- **Modo autônomo** → ✅ **já existe pronto** (henrydaum e eugeniughelbur rodam cron/background "enquanto você dorme"). Copiar o padrão, não reinventar.
- **Conectores** → ✅ **reusáveis** via MCP + importadores .md (gap nos projetos de brain, mas a infra existe). Só plugar.
- **Painel** → parcial (saúde em texto + brain-map/graphify visual). Falta só a camada **operacional** (o que foi automatizado / pendente de aprovação).
- **Entrevista → política de automação** → ❗ **GAP REAL.** Nenhum dos 6 tem política de *permissões de autonomia* (COG chega perto, mas é mapa de integrações, não de limites).

**Conclusão:** o que sobra de genuinamente novo é fino — **(1) a separação em 3 cérebros** (Projeto/Self/Conhecimento, que ninguém faz explícito) + **(2) autonomia governada por uma entrevista que define o que pode/não pode** + **(3) um painel operacional**. Todo o resto é **integrar/configurar** peças existentes. Então `3cerebros`, se acontecer, é **integration-first**: monta sobre MCP + Agent Skills spec + um background-agent (estilo eugeniughelbur) + brain-map, e constrói só a camada fina de política+3-cérebros+painel.

**Atalho possível:** como **Intelecto** (`inematds/intelecto`) já é um agente .md-driven com memória e Telegram **e é seu**, o `3cerebros` pode nascer como uma *evolução dele* (adicionar os 3 cérebros + política), em vez de projeto do zero.

## Como eu leio isso (5 frentes)

### 1. Portabilidade cross-runtime
- A base já ajuda: os 3 cérebros são **arquivos .md + pastas** — formato neutro
  que qualquer agente lê. O que muda por runtime é só a **camada de skills/comandos**.
- Caminho: seguir a **[Agent Skills spec](https://agentskills.io)** (a mesma que o
  kepano usa) → uma fonte, vários runtimes. A skill **`polyskill`** (já instalada
  aqui) empacota skill pra rodar em Claude Code **e** Codex a partir de um fonte
  portátil. Para Hermes/AgentZero/OpenClaw/Intelecto, mapear o equivalente de
  "skill/hook" de cada um (alguns leem `AGENTS.md`, outros têm formato próprio).
- Entregável: um `core/` neutro (os 3 cérebros + regras) + adaptadores finos por
  runtime (`adapters/claude`, `adapters/codex`, `adapters/hermes`…).

### 2. Onboarding (instalar + usar por runtime)
- Um instalador que detecta/pergunta o runtime e escreve os arquivos certos
  (no Claude vira `.claude/skills`, no Codex `~/.codex/skills`, no Hermes…).
- Guia de uso por runtime — provavelmente uma landing+guia (como a do cerebro-inema)
  com abas por agente.

### 3. Modo autônomo (sem comandos) — **o pulo do gato**
- Hoje: você roda `/triagem`, `/nota`, `/tldr`. No `3cerebros`: o agente faz isso
  **sozinho** — observa o que entra e roteia/destila/registra sem você pedir.
- O risco óbvio: autonomia sem limite assusta e erra. Por isso a **entrevista**:
  ela gera uma **política de automação** — o que ele PODE fazer sozinho (ex.:
  arquivar nota, atualizar daily) vs o que **pede confirmação** (ex.: mandar
  e-mail, mover decisão, apagar). 
- Onde isso mora: essa política é parte do **cérebro Self** (valores → permissões).
  Liga com [[estrutura-3-cerebros]] e a skill `socratica`/`setup-cerebro`.
- Níveis de autonomia (sugestão): **observar** → **sugerir** → **agir com
  confirmação** → **agir sozinho** (por tipo de ação). Tudo logado.

### 4. Painel de monitoramento
- Um dashboard local (estilo brain-map, mas com estado, não só grafo) que mostra:
  o que tem em cada cérebro, o que foi automatizado, o que entrou de cada fonte,
  o que está esperando confirmação, e a "saúde" (nota órfã, inbox acumulando).
- Self-contained, lê os `.md` — pode nascer de um `brain-map` + uma camada de stats.

### 5. Importadores / conectores
- Trazer dados crus pras bases (cérebro 3, `sources/` → `/nota` destila):
  **e-mail, Telegram, WhatsApp, export do ChatGPT, redes sociais, outros docs.**
- Reaproveita o processador Gemini (`file-intel`) pra docs; cada fonte vira um
  **conector** (export → normaliza pra .md → entra na inbox → triagem/nota).
- Começar pelos exports fáceis (ChatGPT JSON, e-mail .mbox, Telegram JSON) antes
  dos que pedem API/auth (WhatsApp, redes sociais).

## Tensões / perguntas pra pensar
1. **`3cerebros` é um projeto novo ou a v2 do `cerebro-inema`?** (portátil = reescrever
   a camada de comando como adaptadores; talvez o cerebro-inema vire o `adapters/claude`).
2. **Autonomia x confiança:** qual o default — conservador (só sugere) ou ativo?
   Quem aprova o quê? A política da entrevista resolve, mas precisa ser fácil de mudar.
3. **Painel:** local-only (lê arquivos) ou serviço (pra Hermes 24/7 numa VPS)?
4. **Conectores:** quais primeiro? (voto: ChatGPT export + e-mail + Telegram — exports
   sem API). WhatsApp e redes sociais são os mais chatos (auth/ToS).
5. **Privacidade:** importar e-mail/WhatsApp pro vault é dado sensível — criptografia?
   o que NUNCA sobe pro git? (liga com o 1Password/segredos que o vídeo do Dudu citou).
6. **Multi-runtime de verdade vale o custo?** ou focar em Claude+Codex (polyskill já
   cobre) e deixar os agentes (Hermes/AgentZero…) como adaptadores futuros?

## Relação com o que já temos
- `cerebro-inema` = a base (3 cérebros, Claude-Code, por comando). O `3cerebros`
  pega essa base e adiciona **portabilidade + autonomia + painel + conectores**.
- Camadas que já mapeamos servem de peça: kepano (formato), graphify (recuperação),
  brain-map (visualização → vira o painel). Ver [[comparacao-segundo-cerebros]].
