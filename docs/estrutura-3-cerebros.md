# Estrutura de pastas dos 3 cérebros — proposta para pensar

> Rascunho de arquitetura. Não é decisão final — é um mapa pra você pensar em cima.
> Baseado no frame da §7 do `comparacao-segundo-cerebros.md`: **Projeto · Self · Conhecimento**.
> Data: 2026-06-18

---

## Princípio central

Três cérebros = **três espaços separados, com regras opostas**. A raiz é um vault Obsidian único (pra os links cruzarem entre cérebros), mas dividido em três zonas que **nunca se misturam**:

| Cérebro | Regra de ouro | Ritmo |
|---|---|---|
| **1. Projeto** | append-only e datado; o velho **arquiva** | muda todo dia |
| **2. Self** | poucos arquivos estáveis + ideias linkadas | muda devagar |
| **3. Conhecimento** | cresce pra sempre; tudo consultável | só acumula |

Uma `inbox/` única na entrada: captura rápida cai ali e **depois é roteada** pro cérebro certo (triagem). Nada nasce já classificado.

---

## A árvore proposta

```
brain/                          ← vault Obsidian (raiz)
│
├── 0-inbox/                    ← CAPTURA: tudo entra aqui primeiro, sem classificar
│                                  (triagem move pro cérebro certo depois)
│
├── 1-projeto/                  ← CÉREBRO EPISÓDICO (o que rolou)
│   ├── daily/                  ← notas diárias  YYYY-MM-DD.md
│   ├── projects/               ← uma pasta por projeto ATIVO
│   │   └── <projeto>/
│   │       ├── README.md       ←   brief, estado atual, próximas ações
│   │       └── decisions.md    ←   decisões DESTE projeto (datadas)
│   ├── decisions/
│   │   └── log.md              ← decisões TRANSVERSAIS (append-only global)
│   └── archive/                ← projeto concluído sai de projects/ e vem pra cá
│
├── 2-self/                     ← CÉREBRO DE IDENTIDADE (quem eu sou / penso)
│   ├── me.md                   ← perfil: papel, foco, contexto (estável)
│   ├── values.md               ← valores e princípios (raramente muda)
│   ├── beliefs.md              ← posições/visões de mundo
│   ├── questions/              ← perguntas ABERTAS, não resolvidas
│   │   └── <pergunta>.md       ←   uma por arquivo, linkadas entre si
│   ├── thinking/               ← ensaios / discussões comigo mesmo (Zettelkasten)
│   │   └── <ideia>.md          ←   uma ideia-átomo por nota, com [[wikilinks]]
│   └── journal/                ← reflexão pessoal (separado do daily de projeto)
│
├── 3-conhecimento/             ← CÉREBRO DE REFERÊNCIA (fatos do mundo)
│   ├── sources/                ← material CRU ingerido (PDFs, docs originais)
│   ├── notes/                  ← A WIKI: notas-átomo destiladas, com [[wikilinks]]
│   ├── topics/                 ← MOCs (Maps of Content) — índices por tema
│   │   └── <tema>.md           ←   nota-índice que linka as notes daquele tema
│   ├── bases/                  ← arquivos .base (views consultáveis)
│   └── canvas/                 ← arquivos .canvas (mapas visuais)
│
├── .claude/
│   ├── skills/                 ← skills roteadas por cérebro (ver tabela abaixo)
│   └── settings.json
│
└── CLAUDE.md                   ← O ROTEADOR: aponta cada cérebro e quando usar cada um
```

---

## O que vai (e o que NÃO vai) em cada cérebro

### 1. Projeto — episódico
- **Vai:** o que aconteceu, estado de projeto, decisão + razão + data, próximas ações.
- **Não vai:** conhecimento genérico (isso é cérebro 3), opinião pessoal (cérebro 2).
- **Sinal de que tá certo:** se daqui a 1 ano isso vira história/arquivo, é cérebro 1.

### 2. Self — identidade/reflexão
- **Vai:** quem você é, valores, no que acredita, dúvidas abertas, debates internos, ideias em formação.
- **Não vai:** fato externo verificável (cérebro 3), log de tarefa (cérebro 1).
- **Sinal:** se é *seu* (subjetivo, evolui devagar, ninguém mais teria), é cérebro 2.
- **Detalhe:** `values.md` quase nunca muda; `questions/` e `thinking/` estão sempre vivos. Os dois convivem aqui porque ambos são "você pensando".

### 3. Conhecimento — referência
- **Vai:** fatos do mundo, o que você aprendeu, resumos de fontes, material de consulta.
- **Não vai:** nada datado/pessoal. É impessoal e atemporal.
- **Sinal:** se outra pessoa poderia ter a mesma nota, é cérebro 3.

---

## Skills / ferramentas por cérebro

| Cérebro | Skills/ferramentas | A LLM faz o quê |
|---|---|---|
| **0. Inbox** | `triagem` (nova) — lê o item e sugere pra qual cérebro vai | classifica/roteia |
| **1. Projeto** | `daily`, `tldr` (salva resumo no projeto certo), `vault-setup` | **registra fiel** (decisão+razão+data) |
| **2. Self** | `socratica` (nova) — a LLM te **questiona** em vez de concordar; `reflect` | **pensa junto**, levanta contradição |
| **3. Conhecimento** | `file-intel` (raw→wiki), **kepano** (`obsidian-markdown`/`bases`/`canvas`), **graphify** (se virar código/consulta pesada) | **organiza e recupera** |

> As skills `daily`, `tldr`, `file-intel`, `vault-setup` já existem no seu `~/projetos/second-brain`. As novas (`triagem`, `socratica`) seriam o que falta criar.

---

## Fluxo de uso (como roda no dia a dia)

```
1. CAPTURA   → joga qualquer coisa em 0-inbox/ (sem pensar onde)
2. TRIAGEM   → skill `triagem` lê e roteia:
                 "isso é projeto, self ou conhecimento?"
3. PROCESSA  → no destino, a skill certa age:
                 projeto   → vira nota datada / decisão
                 self      → vira pergunta aberta / ideia linkada
                 conhecimento → file-intel destila em nota-wiki válida (kepano)
4. CONSULTA  → pergunta em linguagem natural; a LLM busca:
                 nas notes/ (wikilinks) ou via graphify (grafo+MCP)
5. ARQUIVA   → projeto concluído vai pra 1-projeto/archive/
```

---

## Links cruzados (o que faz virar um cérebro só)

Os três são separados em **pasta**, mas os `[[wikilinks]]` **cruzam livremente** — é isso que costura tudo:
- uma decisão de projeto (`1`) linka o valor que a motivou (`2/values.md`)
- uma ideia sua (`2/thinking/`) linka o fato que a embasa (`3/notes/`)
- um projeto (`1`) linka o conhecimento que usou (`3`)

Separação na pasta = ordem. Links cruzados = o cérebro pensando junto.

---

## Decisões em aberto (pra você pensar depois)

1. **Vault único ou três vaults?** Um vault (links cruzam fácil) vs três separados (isolamento total, mas link cruzado fica chato). → *Recomendo um só.*
2. **Nomes de pasta:** prefixo numérico (`1-projeto/`) pra forçar ordem, ou nomes PARA-style (`projects/areas/resources/archive`)? Numérico é mais explícito pro frame dos 3 cérebros.
3. **Journal (self) vs Daily (projeto):** mantém os dois separados, ou junta a reflexão pessoal na nota diária? Risco de misturar episódico com identidade.
4. **`questions/` mora no Self ou vira parte do Conhecimento** quando a pergunta é resolvida? (talvez: nasce em `2/questions/`, ao resolver vira nota em `3/notes/`).
5. **graphify entra ou não?** Só vale se você for consultar **código** ou um volume grande de conhecimento. Pra notas pessoais, fica de fora (ver §6 do doc principal).
6. **PT-BR nos nomes de pasta?** As skills esperam estrutura previsível; nomes em inglês são mais portáveis, conteúdo em PT. Decisão de gosto.

---

## Resumo de uma frase

Um vault, **três zonas que não se misturam** (Projeto/Self/Conhecimento), uma **inbox** na entrada, **skills roteadas por zona**, e **wikilinks cruzando** tudo — cada cérebro com seu ritmo, sua regra e sua ferramenta, conversando com a mesma LLM.
