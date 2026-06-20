# 2cerebrox — Estudo sobre o uso de "Segundo Cérebro"

Repositório de **pesquisa e notas** sobre como construir e usar um *second brain* com LLMs (Claude Code) + Obsidian. Não é um produto — é um caderno de estudo: comparações de projetos existentes, frameworks mentais e uma proposta de arquitetura própria.

## O que estamos estudando

A pergunta central: **qual a melhor forma de montar um "segundo cérebro" usando uma LLM?** Para responder, comparamos projetos open source, entendemos a lógica por trás de cada abordagem (incl. a visão do Karpathy de *raw → LLM → wiki*), e destilamos um frame próprio.

### Principais conclusões até aqui

1. **Três filosofias de second brain:**
   - *Conversa pura* (prompt que te entrevista, ex. `Luispitik/claude-code-second-brain`)
   - *raw → LLM → wiki* (a LLM lê o cru e organiza a wiki — a "linha Karpathy", ex. `inematds/second-brain`, `AgriciDaniel/claude-obsidian`)
   - *Obsidian manual* (você cura os links à mão — Zettelkasten clássico)

2. **Três camadas, não concorrentes:**
   - **Organização** (o cérebro): Luispitik, inematds
   - **Formato** (as mãos): `kepano/obsidian-skills` — escrever `.md`/`.base`/`.canvas` válidos
   - **Recuperação** (a busca): `safishamsi/graphify` — grafo + MCP que o agente consulta (code-first)

3. **Três TIPOS de cérebro** (o frame que organiza tudo):
   - **Projeto** (episódico) — o que rolou, decisões, estado
   - **Self** (identidade) — valores, pensamentos, dúvidas abertas
   - **Conhecimento** (referência) — fatos do mundo, o que aprendi

   → Cada um quer espaço, regra e ferramenta próprios. Misturar = cérebro inchado.

## Documentos

| Doc | O que é |
|---|---|
| [`docs/comparacao-segundo-cerebros.md`](docs/comparacao-segundo-cerebros.md) | Análise comparativa completa: 3 projetos → panorama do GitHub → Karpathy vs Obsidian → kepano (formato) → graphify (recuperação) → os 3 tipos de cérebro → conclusão. |
| [`docs/estrutura-3-cerebros.md`](docs/estrutura-3-cerebros.md) | Proposta de **estrutura de pastas** dos 3 cérebros (Projeto/Self/Conhecimento), skills por zona, fluxo de uso e 6 decisões em aberto pra pensar. |
| [`docs/ideia-3cerebros-portavel.md`](docs/ideia-3cerebros-portavel.md) | **Parking lot:** ideia de um `3cerebros` portátil (Claude/Codex/Hermes/AgentZero/OpenClaw…), autônomo (entrevista → política de automação), com painel e conectores (email/Telegram/WhatsApp/ChatGPT export). Inclui gating "os sistemas já não fazem isso?". |

## Status

🔬 Em estudo. Próximo passo provável: escolher as decisões em aberto da proposta de arquitetura e montar o esqueleto de pastas real.
