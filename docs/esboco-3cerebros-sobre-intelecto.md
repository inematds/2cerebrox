# Esboço: `3cerebros` evoluindo a partir do `intelecto`

> Arquitetura proposta. Não é build — é o desenho. Baseado na **arquitetura real**
> do `inematds/intelecto` (lida em 2026-06-20), não em palpite.
> Liga com [[ideia-3cerebros-portavel]] (gating) e [[estrutura-3-cerebros]].

## Por que o intelecto é a base certa
O `intelecto` já tem, prontos, 4 dos pilares que o `3cerebros` precisaria construir:

- **Stack de identidade em `.md`** — `workspace/SOUL.md` (personalidade/valores),
  `AGENTS.md` (regras), `USER.md` (perfil), `MEMORY.md` (seed). Carregados por
  `agent/context.py` a cada mensagem. → **já é o cérebro Self.**
- **Memória SQLite FTS5 + BM25** com categorias `fact / conversation / solution /
  preference`, dedup >80%, tools `save_memory / search_memory / forget_memory`. →
  **já é o motor do cérebro Conhecimento.**
- **Interfaces abstratas** (`BaseProvider`, `BaseChannel`, `BaseTool` + registry):
  adicionar canal/ferramenta = arquivo novo, sem tocar no existente. → **é o
  encaixe pros conectores e pras skills dos 3 cérebros.**
- **Wizard de entrevista** (`wizard.py`, 6 passos) + `security/safety.py`
  (blocklist, confirmação, audit log). → **é o lugar da política de automação.**

E o **roadmap do próprio intelecto** já lista: `tools/cron.py` (autonomia),
`integrations/mcp.py` (conectores), `integrations/obsidian.py` (vault). Ou seja:
`3cerebros` é **quase um preenchimento do roadmap do intelecto** + a camada nova de
3 cérebros e política.

## A grande ideia: 1 cérebro, 2 frontends
```
                ┌─────────── O CÉREBRO (formato único) ───────────┐
                │  workspace/ (.md, 3 cérebros)  +  memory.db (FTS5) │
                └───────────────┬───────────────────┬───────────────┘
                                │                   │
                 ┌──────────────┴───┐      ┌────────┴──────────────┐
                 │  cerebro-inema    │      │      intelecto         │
                 │  (Claude Code,    │      │  (agente Python 24/7,  │
                 │   por comando,    │      │   Telegram, autônomo)  │
                 │   Agent Skills)   │      │                        │
                 └───────────────────┘      └────────────────────────┘
```
O `cerebro-inema` (que já construímos) é o **frontend Claude-Code, por comando**.
O `intelecto` é o **frontend standalone, autônomo, always-on**. O `3cerebros` é a
**unificação**: os dois leem/escrevem o **mesmo formato de cérebro** (pasta `.md` +
`memory.db`), e ganham por cima a **política de automação** (o gap real do gating).

## Mapa: recurso do 3cerebros → módulo do intelecto

| Recurso 3cerebros | Onde encaixa no intelecto | Reusar / Adicionar |
|---|---|---|
| **Cérebro Self** | `workspace/SOUL.md` + `USER.md` + `AGENTS.md` | **reusar** (+ `values.md`, `questions/`) |
| **Cérebro Conhecimento** | `memory/store.py` (FTS5) + `integrations/obsidian.py` (roadmap) | reusar memória; **add** vault |
| **Cérebro Projeto** | categoria `conversation` + `workspace/projeto/` (daily, decisions) | **add** (pequeno) |
| **Triagem / nota (sem comando)** | `tools/registry.py` (novas tools) + `agent/loop.py` | **add** tools `triagem`, `nota` |
| **Modo autônomo** | `tools/cron.py` (roadmap Fase 2) | **add** jobs (briefing, triagem, saúde) |
| **Conectores** (email/TG/WA/ChatGPT) | `channels/` + `integrations/mcp.py` (roadmap Fase 3) | **reusar** MCP servers + importadores |
| **Política de automação** | `wizard.py` (entrevista) + `security/safety.py` | **add** `POLICY` + policy-engine ← novidade |
| **Painel operacional** | (intelecto não tem web por filosofia) | **add** HTML local (brain-map) + status no Telegram |
| **Portabilidade** | formato `.md`+SQLite neutro; Agent Skills no lado Claude | **reusar** (2 frontends, 1 formato) |

## O diferencial, em detalhe: a Política de Automação
É o único gap real do gating — então é o coração do `3cerebros`. Mecanismo:

1. **Entrevista (estende o `wizard.py`):** além de nome/modelo/personalidade, pergunta
   *o que o cérebro pode fazer sozinho*. Gera `workspace/POLICY.md` (legível/editável).
2. **Níveis por tipo de ação:** `observar` → `sugerir` → `agir com confirmação` →
   `agir sozinho`. Ex.:
   ```
   arquivar nota / atualizar daily      → agir sozinho
   registrar decisão / mover de cérebro → agir com confirmação
   enviar email / postar / rodar shell  → confirmação sempre
   apagar / pagar / comprar             → nunca (bloqueado)
   ```
3. **Enforcement (estende `security/safety.py`):** antes de cada tool-call no
   `agent/loop.py`, consulta a POLICY. Já existe o gancho (blocklist + audit log) —
   vira um **policy-engine** que decide: executa / pede confirmação no Telegram / nega.
4. **Tudo logado** no `audit.log` que já existe. A política é dado do **cérebro Self**
   (deriva dos valores) — fecha o ciclo com a skill `socratica`/valores.

## Como os 3 cérebros assentam sobre o intelecto
```
~/.intelecto/                      (já existe)
  memory.db        → Conhecimento (fatos) + Projeto (conversation/solution)
  audit.log        → trilha de tudo que a autonomia fez

workspace/                          (já existe, .md)
  SOUL.md AGENTS.md USER.md         → cérebro SELF (identidade)
  values.md  questions/             → SELF (add)
  POLICY.md                         → política de automação (add) ← novidade
  projeto/ daily/ decisions.md      → cérebro PROJETO (add)
  conhecimento/ (vault Obsidian)    → cérebro CONHECIMENTO (add, via integrations/obsidian.py)
  0-inbox/                          → captura → triagem autônoma (add)
```

## Plano em fases (piggyback no roadmap do intelecto)
- **Fase 0 — formato:** definir a pasta `workspace/` dos 3 cérebros + categorias do
  `memory.db`. Fazer o `cerebro-inema` e o `intelecto` lerem o mesmo layout.
- **Fase 1 — política (o diferencial):** `POLICY.md` + entrevista no wizard +
  policy-engine no `safety.py`/`loop.py`. Entregável testável sozinho.
- **Fase 2 — autonomia:** `tools/cron.py` (roadmap) → jobs de triagem, briefing diário,
  destilar inbox, health — todos **gated pela POLICY**.
- **Fase 3 — conectores:** `integrations/mcp.py` (roadmap) + importadores
  (ChatGPT export, Telegram, email) → caem em `0-inbox/` → triagem.
- **Fase 4 — painel:** HTML local estilo `brain-map` sobre `workspace/` + comandos de
  status no Telegram ("o que está pendente de aprovação?", "saúde do cérebro").

## Perguntas em aberto (pra pensar)
1. **`3cerebros` = repo novo ou branch/v2 do `intelecto`?** Voto: **evoluir o intelecto**
   (preenche o roadmap dele) e deixar `cerebro-inema` como o frontend Claude-Code.
2. **macOS-only:** o intelecto usa `launchd` + UUID de hardware (Fernet). Pra Linux
   (sua máquina) precisa de `systemd` + outra derivação de chave. Quanto custa portar?
3. **Painel:** local-only (gera HTML on-demand, fiel à filosofia "sem web") vs serviço.
   Voto: on-demand.
4. **Categorias da memória vs 3 cérebros:** mapear `fact/solution`→Conhecimento,
   `conversation`→Projeto, `preference`→Self? Ou adicionar uma coluna `cerebro`?
5. **Confiança:** começar a POLICY **conservadora** (quase tudo "sugerir") e o usuário
   solta gradualmente? (recomendado).
