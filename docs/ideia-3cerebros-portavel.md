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
construir. Não reinventar. Abaixo, meu palpite atual (a **verificar** repo por repo
e nas docs de cada runtime — é pesquisa a fazer, não conclusão fechada):

| Frente | Quem já faz (palpite) | Veredito provável |
|---|---|---|
| **Portabilidade cross-runtime** | Agent Skills spec (kepano, polyskill já cobre Claude+Codex); `graphify` roda em ~20 runtimes; `eugeniughelbur/obsidian-second-brain` é "cross-CLI" | **Largamente resolvido** — reusar a spec, não criar formato novo |
| **Conectores (email/Telegram/WhatsApp/ChatGPT/social)** | `khoj-ai/khoj` tem integrações; existem **MCP servers** prontos (Gmail, Telegram…); importadores de ChatGPT export já existem | **Reusar** — provavelmente via MCP + exports, não conectores caseiros |
| **Painel / dashboard** | `brain-map`/Brain Studio (grafo), `graphify` (graph.html), khoj (UI) | **Base existe** (visual); falta a camada *operacional* (o que foi automatizado / pendente / saúde) |
| **Modo autônomo (self-organizing)** | `AgriciDaniel/claude-obsidian` ("drop e ele arquiva"), `huytieu/COG` (worker agents), `henrydaum/second-brain` (agentic) | **Existe em parte** — mas geralmente sem política de limites clara |
| **Entrevista → política de automação (o que pode/não pode)** | — não vi ninguém fazer isso explícito | **Provável GAP / diferencial** |

**Leitura honesta:** portabilidade, conectores e visualização já estão **muito
servidos** lá fora — o `3cerebros` deve **apoiar-se neles** (MCP pros conectores,
Agent Skills spec pra portar, brain-map de base pro painel). O que parece
**genuinamente faltar** — e seria o diferencial — é a combinação:
**separação em 3 cérebros + modo autônomo governado por uma política definida numa
entrevista + um painel operacional (não só grafo)**, tudo sobre os mesmos arquivos.

**Tarefa antes de decidir construir:** uma rodada de pesquisa que confirme, recurso
por recurso, o que cada sistema/runtime (khoj, claude-obsidian, COG, Hermes,
AgentZero, OpenClaw, Intelecto, MCP servers) já entrega — e isolar o que sobra
de novo. Se sobrar pouco, talvez seja **integrar/configurar**, não construir.

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
