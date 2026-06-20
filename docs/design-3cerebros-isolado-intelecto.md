# Design: `3cerebros` isolado (embutível) + integração no `intelecto`

> Foco pedido: implementar no **intelecto**, mas com o `3cerebros` como **repo
> isolado** que qualquer um incorpora no seu sistema via repo. Ancorado no código
> real do `intelecto-testes` (FastAPI + WhatsApp/Evolution + OpenRouter, `soul.md`,
> módulos-stub `memory/knowledge/scheduler`). Ver [[analise-3cerebros-sobre-openpcbot]],
> [[esboco-3cerebros-sobre-intelecto]], [[ideia-3cerebros-portavel]].

## A separação que você pediu
```
┌────────────────────────────────────────────────────────────┐
│  REPO ISOLADO: 3cerebros   (o CÉREBRO — embutível por qualquer um) │
│  • formato (pasta .md + SQLite)  • memória  • política       │
│  • ingestão (conectores)         • skills triagem/nota/socratica │
│  exposto como: SDK (py/ts) · MCP server · Agent Skills · spec │
└───────────────┬────────────────────────────────────────────┘
                │ incorporado via repo (submodule / pip / npm / MCP)
   ┌────────────┼─────────────┬───────────────────────┐
   ▼            ▼             ▼                       ▼
intelecto   openpcbot   cerebro-inema           sistema de terceiros
(jarvis,    (bot TS)    (Claude Code)           (qualquer agente)
 o "corpo")
```
O **3cerebros** é o cérebro, isolado e reutilizável. O **intelecto** é um **corpo de
jarvis** que o incorpora. Outros sistemas incorporam o mesmo repo do seu jeito.

## O repo `3cerebros` (isolado) — o que tem dentro
```
3cerebros/
├── spec/
│   ├── FORMAT.md            # a "constituição": layout da pasta + schema SQLite
│   └── POLICY.md            # formato da política de automação
├── core/                    # núcleo runtime-agnóstico (lógica, sem I/O de canal)
│   ├── brain.py             # classe Brain (a API pública) — ver contrato abaixo
│   ├── memory.py            # SQLite FTS5/BM25, categoria por cérebro (sector)
│   ├── policy.py            # policy-engine: decide agir/perguntar/negar
│   ├── triage.py            # roteia item → cérebro certo
│   ├── distill.py           # raw → nota-wiki (cérebro Conhecimento)
│   └── ingest/              # importadores: chatgpt_export, telegram, email, …
├── sdk/
│   ├── python/              # `pip install cerebros` — cliente fino sobre core/
│   └── ts/                  # `npm i @inema/cerebros` — idem p/ hosts TS (openpcbot)
├── mcp/
│   └── server.py            # expõe o Brain como MCP server (qualquer runtime pluga)
├── skills/                  # Agent Skills (Claude/Codex/…): triagem, nota, socratica
└── examples/                # host mínimo de 30 linhas mostrando a integração
```
Distribuição: **git submodule** (`git submodule add …/3cerebros`), **pip/npm**, ou
só subir o **MCP server**. Tudo opcional — o host escolhe a via.

## O contrato público (a única coisa que o host precisa saber)
Uma superfície mínima. Mesma semântica em py, ts e MCP:

```python
brain = Brain(path="~/.cerebros")          # abre/cria o cérebro (pasta + db)

ctx = brain.build_context(query, who)      # → system prompt: Self + memórias relevantes
brain.observe(who, user_msg, ai_reply)     # autônomo: triagem + save no cérebro certo
brain.ingest(source)                       # importa export (chatgpt/telegram/email) → inbox
verdict = brain.policy.check(action)       # → 'allow' | 'confirm' | 'deny'  (o diferencial)
brain.run_jobs()                           # tarefas de manutenção (cron chama isto)
```
Isso é tudo. O resto (formato, FTS5, 3 cérebros, política) fica **dentro** do repo.

## Integração no `intelecto-testes` (cirúrgica, no `core.py`)
Hoje (`src/agent/core.py`):
```python
soul = load_soul(settings.soul_path)
response = await _provider.chat(messages=history, system=soul)
```
Com 3cerebros (o `soul.md` vira o seed do cérebro **Self**):
```python
from cerebros import Brain
brain = Brain(path=settings.cerebros_path)             # incorpora o repo isolado

# 1. contexto = Self (inclui soul.md) + memórias relevantes dos 3 cérebros
system = brain.build_context(query=text, who=remote_jid)
response = await _provider.chat(messages=history, system=system)

# 2. observa e roteia sozinho (preenche os stubs memory/ e knowledge/)
brain.observe(who=remote_jid, user_msg=text, ai_reply=response)
```
E os módulos-stub do intelecto deixam de ser vazios — viram **cascas finas**:
- `src/memory/` → delega pra `brain` (save/search)
- `src/knowledge/` → delega pra `brain.distill`/`ingest`
- `src/scheduler/` → no tick do cron, chama `brain.run_jobs()`
- **antes de qualquer ação externa** (enviar WhatsApp proativo, rodar tool) →
  `brain.policy.check(...)` decide. ← a peça nova que ninguém do gating tem.

Resultado: o intelecto vira o **jarvis** (WhatsApp + LLM + agendador), e **toda a
inteligência de memória/organização/autonomia governada** vem do repo 3cerebros.

## Como **qualquer um** incorpora (o requisito central)
| Host | Como incorpora |
|---|---|
| Python (intelecto, jarvis caseiro) | `pip install cerebros` ou submodule → `from cerebros import Brain` |
| TypeScript (openpcbot) | `npm i @inema/cerebros` (SDK ts) **ou** falar com o MCP server |
| Claude Code / Codex / Gemini CLI | instalar as **Agent Skills** do `skills/` + (opcional) MCP |
| Qualquer outro runtime | subir `mcp/server.py` e usar as tools via **MCP** |
| Só quer o formato | apontar o tool pro `~/.cerebros/` (pasta .md + db) — é spec aberta |

O **MCP server** é o curinga: é o que torna o 3cerebros incorporável por *qualquer*
sistema sem SDK — exatamente o caminho que o gating mostrou ser o reuso padrão.

## Plano em fases (casa com a "Fase 2 = SQLite" do intelecto)
- **F0 — repo isolado:** criar `3cerebros` com `spec/FORMAT.md` + `core/brain.py` +
  `core/memory.py` (o intelecto já ia fazer SQLite na Fase 2 — fazer **dentro** do
  3cerebros e importar). Entrega: `Brain` com build_context/observe/save/search.
- **F1 — política (o diferencial):** `core/policy.py` + `spec/POLICY.md` + entrevista
  que gera a `POLICY.md` do usuário. O intelecto passa a chamar `policy.check`.
- **F2 — autonomia:** `run_jobs()` (triagem/briefing/saúde) + `scheduler/` do intelecto
  chamando no cron, tudo sob política.
- **F3 — conectores:** `core/ingest/*` (chatgpt export, telegram, email via MCP) →
  `0-inbox/` → triagem. (WhatsApp o intelecto já tem como canal.)
- **F4 — skills + MCP + SDK ts:** empacotar as Agent Skills, subir o MCP server, e o
  SDK ts pro openpcbot — fechando "um cérebro, vários frontends".

## Perguntas em aberto
1. **Núcleo só Python ou Python+TS?** Voto: **core em Python** (intelecto é py) +
   **MCP** como ponte universal; SDK ts só se o openpcbot entrar de fato.
2. **`soul.md` do intelecto** vira o `2-self/SOUL.md` do cérebro (mesmo arquivo) — ok?
   Assim a personalidade do jarvis **é** o cérebro Self. (recomendado)
3. **Onde mora o cérebro:** `~/.cerebros/` (compartilhável entre frontends) vs dentro
   do projeto. Voto: `~/.cerebros/` pra um cérebro, vários corpos.
4. **Política default conservadora** (quase tudo "confirmar") e o usuário solta? (sim).
5. **Nome do pacote/repo:** `3cerebros` / `cerebros` / `@inema/cerebros`?
