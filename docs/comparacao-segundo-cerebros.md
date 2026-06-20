# Comparação de "Segundo Cérebro" (Second Brain)

> Análise comparativa entre projetos de second brain para Claude Code / Obsidian.
> Data: 2026-06-18

---

## 0. Projetos analisados

| # | Projeto | Origem |
|---|---|---|
| 1 | `Luispitik/claude-code-second-brain` | GitHub |
| 2 | `inematds/second-brain` | GitHub |
| 3 | `/home/nmaldaner/projetos/second-brain` | Local |

**Achado-chave:** o projeto **local é idêntico ao `inematds/second-brain`** — `diff -rq` deu zero diferenças, e os remotes apontam pra `inematds` (e `origin → earlyaidopters`). Ou seja, não são 3 projetos distintos: são **2 abordagens diferentes**, e o local é uma cópia/fork da abordagem do inematds (traduzida pra PT-BR, com suporte Linux).

### Linhagem completa (quem veio de quem)

O upstream real é o **`earlyaidopters/second-brain`** (⭐ 176, `fork: false`) — é dele que o `setup.sh` baixa via `raw.githubusercontent.com/earlyaidopters/...`. O `inematds` (e o local) é um fork dele.

```
earlyaidopters/second-brain   ← ORIGINAL (inglês, mac/Windows, 176⭐)
        │  fork
        ▼
inematds/second-brain  ≡  ~/projetos/second-brain   ← FORK (PT-BR + Linux + research/)
```

O fork **não mudou o motor** — `diff` mostrou que as 4 skills (`daily`, `tldr`, `file-intel`, `vault-setup`), os scripts Python (Gemini), o `vault-template/` e o `CLAUDE.md` são **byte-a-byte idênticos**. Só diferem: `README.md` (inglês → PT), adição de `README.pt-BR.md` e `research/`, e `setup.sh`/`setup.ps1` (ganharam suporte Linux). Então, das três entradas da tabela acima, **duas (inematds e local) são o mesmo fork**, e ambas descendem do `earlyaidopters`. Resta uma única filosofia *raw→LLM→wiki* nessa linhagem — `Luispitik` é a outra (conversa pura).

---

## 1. Os três projetos lado a lado

### Visão de uma frase

| Projeto | O que é, em 1 frase |
|---|---|
| **Luispitik/claude-code-second-brain** | Um **prompt só** que você cola no Claude Code; ele te entrevista e monta a estrutura de pastas de memória. Sem instalação, sem scripts. |
| **inematds/second-brain** | Um **sistema instalável** (Obsidian + Claude Code + Python/Gemini) que lê seus arquivos reais e vira memória. Com setup, skills e scripts. |
| **local `~/projetos/second-brain`** | = **cópia do inematds** (mesmos arquivos, traduzido p/ PT-BR, com suporte Linux). |

### A ideia que os dois compartilham
Ambos partem do mesmo truque: **o Claude Code lê os arquivos do diretório no início de cada sessão → esses arquivos *são* a memória.** A diferença é o *quanto de maquinário* cada um coloca em volta disso.

### Comparação detalhada

| Critério | **Luispitik** | **inematds / local** |
|---|---|---|
| **Como instala** | Cola `PROMPT.md` numa pasta vazia. Nada mais. | `curl … setup.sh` ou clona o repo e roda `./setup.sh` (mac/win/linux). |
| **Peso / dependências** | Zero. Só o Claude Code. | Python, `requirements.txt`, API do Gemini, Obsidian. |
| **Memória baseada em** | Arquivos de contexto que o Claude cria na entrevista (`context/`, `decisions/log.md`, `projects/`). | Vault do Obsidian (`inbox/ daily/ projects/ research/ archive/`) + `CLAUDE.md`. |
| **Processa seus arquivos reais?** | **Não.** Memória vem só da conversa/entrevista. | **Sim** — `file-intel` extrai PDF, PPTX, XLSX, DOCX, CSV, JSON via Gemini e gera resumos. |
| **Automação** | Templates manuais (`session-summary.md`). | Skills/slash-commands: `/vault-setup`, `/daily`, `/tldr`, `/file-intel`. |
| **Persona** | "Assistente executivo" — perfil, negócio, time, tom, log de decisões. | "Segundo cérebro" de conhecimento — notas, projetos, arquivos acumulados. |
| **Idioma** | Inglês + Espanhol. | Português (PT-BR) + suporte Linux (no local). |
| **Cara comercial** | Sim — branding NorteIA/SalgadoIA, CTA p/ salgadoia.com. | Não — open, "grátis/privado/local", referências de autor removidas. |
| **Tamanho do repo** | 6 arquivos (README, PROMPT, LICENSE…). | ~25 arquivos (scripts, skills, vault-template, 2 READMEs). |

### Como escolher
- **Quer leve, rápido, "assistente que me conhece" sem instalar nada** → **Luispitik**. É literalmente um prompt.
- **Quer ingerir documentos de verdade (PDFs, planilhas) e organizar num vault Obsidian** → **inematds / local**. Mais poderoso, mas pede Python + chave do Gemini.
- O **local** não é um terceiro caminho: é o inematds traduzido. Já tem inclusive um `research/analise-second-brain.md` dentro dele.

### Resumo honesto
São **filosofias opostas do mesmo conceito**: Luispitik aposta em *minimalismo* (prompt → memória conversacional), inematds aposta em *tooling* (instalador → vault + extração de arquivos). Nenhum é "melhor" — Luispitik ganha em simplicidade e zero-fricção; inematds ganha em capacidade real de processar e arquivar conteúdo. O local é o lado "tooling", em PT-BR.

---

## 2. Panorama: outros "second brain" bem cotados no GitHub

Busca por estrelas (`gh search`). Separado do mais parecido com os seus até o panorama geral.

### 2.1 Diretos — "Claude Code/CLI + arquivos vira cérebro" (mesmo nicho)

| ⭐ | Repo | O que é |
|---|---|---|
| **7.085** | `AgriciDaniel/claude-obsidian` | Second brain auto-organizável p/ Obsidian + Claude Code. Joga qualquer fonte, o Claude lê, linka e arquiva. **Líder do nicho exato do inematds.** |
| **3.410** | `agenticnotetaking/arscontexta` | Plugin do Claude Code que gera um sistema de conhecimento individual a partir da conversa. **Mesma filosofia "entrevista" do Luispitik.** |
| **2.976** | `SamurAIGPT/llm-wiki-agent` | Base de conhecimento que se constrói sozinha; Claude/Codex/Gemini lê as fontes. |
| **2.534** | `eugeniughelbur/obsidian-second-brain` | Skill cross-CLI (Claude Code, Codex, Gemini) que transforma o vault Obsidian em cérebro vivo. |
| **1.524** | `ballred/obsidian-claude-pkm` | Starter kit completo Obsidian + Claude Code. **Praticamente o que o `setup.sh` do inematds entrega.** |
| **777** | `coleam00/second-brain-skills` | Coleção de Claude Skills pra virar second brain. **Mesma ideia das 4 skills do inematds.** |
| **605** | `coleam00/second-brain-starter` | Skill que gera um PRD personalizado pra um second brain proativo. |
| **557** | `henrydaum/second-brain` | Framework agêntico = "SO" usando inteligência de arquivos locais + workflows. |
| **547** | `huytieu/COG-second-brain` | Cérebro auto-evolutivo com 17 skills, 6 worker agents e CRM de pessoas. |

### 2.2 Apps de PKM com IA (standalone, não dependem de Claude Code)

| ⭐ | Repo | O que é |
|---|---|---|
| **35.193** | `khoj-ai/khoj` | "Seu second brain de IA", self-hostable, responde sobre seus docs/web, agentes custom. **O maior de todos.** |
| **8.562** | `reorproject/reor` | App de PKM com IA, privado e local (RAG embutido). |
| **2.629** | `revezone/revezone` | Ferramenta local-first gráfica (Excalidraw) pra montar second brain. |
| **1.131** | `your-papa/obsidian-Smart2Brain` | Plugin Obsidian com assistente de IA focado em privacidade. |
| **905** | `churichard/notabase` | Second brain web pra conhecimento/ideias. |

### 2.3 PKM clássico (sem IA) e listas curadas

| ⭐ | Repo | O que é |
|---|---|---|
| **44.500** | `siyuan-note/siyuan` | PKM self-hosted, privacy-first, open source. |
| **17.229** | `foambubble/foam` | PKM dentro do VSCode. |
| **7.437** | `dendronhq/dendron` | PKM que "cresce com você". |
| **1.795** | `KasperZutterman/Second-Brain` | **Lista curada** de Zettelkastens / Second Brains / Digital Gardens públicos. |
| **426** | `aristoapp/awesome-second-brain` | Lista curada focada em second brain auto-evolutivo p/ agentes de IA. |

### Onde os seus se encaixam
- O **inematds / local** (Obsidian + Claude Code + scripts) compete direto com `AgriciDaniel/claude-obsidian` (7k ⭐) e `ballred/obsidian-claude-pkm` (1.5k ⭐) — mesma proposta, mais maduros.
- O **Luispitik** (prompt-only que entrevista) é a mesma filosofia de `arscontexta` (3.4k ⭐) e `coleam00/second-brain-starter` (605 ⭐).
- **Diferencial que os grandes não têm e o local tem:** PT-BR + suporte Linux. Os top do nicho são quase todos em inglês.

---

## 3. Caso à parte: `kepano/obsidian-skills` — ⭐ 36.042

**Quem fez:** Steph Ango (`kepano`), **CEO do Obsidian**. Quase "oficial". MIT.

**O que NÃO é:** não é um "segundo cérebro". Não tem sistema de organização, não te entrevista, não monta vault, não ingere arquivos, não tem workflow de memória.

**O que É:** um pacote de **5 skills que ensinam o agente a manipular corretamente os formatos abertos do Obsidian.** São as "mãos", não o "cérebro":

| Skill | Para quê |
|---|---|
| `obsidian-markdown` | Escrever Markdown sabor-Obsidian certo: wikilinks, embeds, callouts, properties/frontmatter. |
| `obsidian-bases` | Criar/editar arquivos `.base` (views, filtros, fórmulas — o "banco de dados" do Obsidian). |
| `json-canvas` | Criar/editar `.canvas` (mapas mentais, fluxogramas). |
| `obsidian-cli` | Dirigir o Obsidian aberto via CLI (ler/criar/buscar notas, dev de plugin/tema). |
| `defuddle` | Extrair markdown limpo de páginas web (tira propaganda/navegação, economiza tokens) — substituto do WebFetch. |

Distribuído como **plugin de marketplace** (`/plugin marketplace add kepano/obsidian-skills`) e segue a [Agent Skills spec](https://agentskills.io) → roda em Claude Code, Codex e OpenCode.

### Como ele se relaciona com os seus
Não é concorrente. É uma **camada abaixo** — fundação que os seus poderiam *usar*:

```
┌─────────────────────────────────────────────┐
│  inematds/local  ·  Luispitik  ·  arscontexta │  ← O CÉREBRO
│  (organizam, entrevistam, ingerem, arquivam) │     "o quê / como organizar"
├─────────────────────────────────────────────┤
│            kepano/obsidian-skills            │  ← AS MÃOS
│  (escrevem .md/.base/.canvas corretos,       │     "como mexer no Obsidian"
│   dirigem o CLI, limpam web)                  │
└─────────────────────────────────────────────┘
```

Comparando com as **4 skills do inematds** (`daily`, `tldr`, `file-intel`, `vault-setup`):
- As do inematds são de **fluxo/organização** (resumir conversa, processar pasta com Gemini, montar vault).
- As do kepano são de **sintaxe/formato** (gerar callout válido, montar `.base`, usar o CLI).
- **Se complementam:** `file-intel` ingere PDFs → `obsidian-markdown`/`obsidian-bases` garantiriam nota com wikilinks e properties corretos. `defuddle` substituiria bem qualquer fetch de web no pipeline.

### Resumo
Os outros competem pelo "ser o cérebro". O **kepano/obsidian-skills é a infraestrutura oficial do Obsidian pra IA** — não compete com o inematds, **fortaleceria** ele. Pra evoluir o local: instalar o plugin do kepano por baixo e deixar as skills próprias focarem só no fluxo.

---

## 4. LLM como cérebro (linha Karpathy) vs Obsidian

A pergunta de fundo: **deixar a LLM ser o cérebro (à la Karpathy) ou montar o cérebro à mão no Obsidian?** São duas filosofias de "onde mora o conhecimento e quem organiza".

### A lógica do Karpathy: raw → LLM → wiki

```
dado cru  →  LLM lê  →  LLM organiza  →  wiki (.md, wikilinks)
(PDF, conversa,         (não você)        ↑                │
 e-mail, nota solta)                      └────────────────┘
                                  o wiki vira o contexto que a
                                  LLM lê da próxima vez → acumula
```

**Correção importante:** não é "LLM = sem estrutura" contra "Obsidian = estrutura". O Karpathy **também** produz uma wiki estruturada no final. A diferença real é **quem faz a curadoria:**

- **Obsidian clássico (Zettelkasten):** *você* lê, *você* linka, *você* arquiva. A wiki é fruto da sua disciplina.
- **Karpathy:** *a LLM* lê o cru e *a LLM* monta/mantém a wiki. Você só joga o material. A wiki é fruto do modelo.

Ele **não é contra a wiki do Obsidian** — é contra *você ser o office-boy que arquiva tudo*. O Obsidian vira o **substrato** (texto plano, formato aberto, que ele aprova) onde a LLM escreve. A LLM é a **bibliotecária**.

### As três posições

| | Quem organiza | Tem wiki durável? |
|---|---|---|
| Luispitik (conversa pura) | LLM, mas sem arquivar formal | Quase não — vive no contexto |
| **Karpathy (raw → wiki)** | **LLM** | **Sim** — a LLM escreve os `.md` |
| Obsidian clássico | **Você** | Sim — você escreve os `.md` |

### Comparação de fundo (LLM vs Obsidian manual)

| | **LLM como cérebro** (linha Karpathy) | **Obsidian manual** (PKM clássico) |
|---|---|---|
| **Quem organiza** | O modelo. Estrutura emergente/automática. | Você, à mão. Estrutura explícita. |
| **Como você recupera** | Pergunta em linguagem natural → sintetiza. | Você navega/busca → acha a nota exata. |
| **Esforço de entrada** | Baixo: joga o cru, o modelo lê. | Alto: o "imposto" de curar e linkar. |
| **Confiabilidade** | Probabilística — conecta, mas **alucina**. | Determinística — o que está escrito está lá. |
| **Posse / durabilidade** | Os dados crus são seus; a "inteligência" depende do modelo. | `.md` seus, duram décadas, funcionam sem IA. |
| **Brilha em** | Sintetizar e conectar (serendipidade). | Precisão, permanência, fonte de verdade. |

**Onde isso aparece nos projetos:** a lógica raw→LLM→wiki **é literalmente** o `SamurAIGPT/llm-wiki-agent`, o `AgriciDaniel/claude-obsidian` e o **`file-intel` do inematds/local** (pasta de PDFs → a LLM gera notas prontas pro Obsidian). O local **já é** "o jeito Karpathy" em PT-BR — falta fechar a alça (a wiki gerada virar contexto da próxima rodada) e garantir `.md` válido.

**Resumo:** LLM-cérebro = *baixo esforço, alta síntese, baixa garantia*; Obsidian manual = *alto esforço, alta precisão, permanente*. A combinação (Obsidian = memória + LLM = mente) pega o melhor dos dois e é o desenho dos second brains modernos.

---

## 5. Vantagem de incorporar o `kepano/obsidian-skills`

O loop raw→wiki **só vale o quanto a wiki que ele gera é boa** — e "boa" no Obsidian quer dizer **linkada e consultável**, não `.md` solto. O kepano é o que faz a LLM escrever Obsidian *de verdade* em vez de markdown genérico.

### O problema sem o kepano
Quando o Claude organiza o cru sozinho, ele tende a:
- escrever links como texto comum em vez de `[[wikilink]]` → **as notas não se conectam.** E o grafo de links *é* o cérebro. Sem link, é uma pasta de arquivos, não um second brain.
- errar/ignorar `properties` (frontmatter YAML) → o Obsidian não indexa, não dá pra filtrar.
- nunca gerar um `.base` (views de banco de dados) nem um `.canvas` válido — não sabe a sintaxe.

### O que cada skill compra (mapeado no loop)

| Skill | O que garante | Por que importa no loop |
|---|---|---|
| `obsidian-markdown` | `[[wikilinks]]`, callouts, properties, embeds corretos | **Fecha a alça** — notas se linkam, a wiki vira contexto conectado que melhora a próxima rodada |
| `obsidian-bases` | gera `.base` (filtros, fórmulas, views) | Vira **banco de dados consultável** ("todos os projetos status=ativo") |
| `json-canvas` | gera `.canvas` válido | Mapas mentais/visões espaciais que a LLM não monta sozinha |
| `obsidian-cli` | opera o vault vivo (buscar, criar, deduplicar) | A LLM **mantém** a wiki (acha duplicata, atualiza nota) em vez de só despejar arquivo novo |
| `defuddle` | extrai markdown limpo da web | Melhor **entrada crua** → nota melhor, menos tokens |

### A vantagem em uma frase
- Sem kepano: `file-intel` → **pilha de resumos em markdown**.
- Com kepano: `file-intel` → **wiki Obsidian conectada e consultável** (links + bases + manutenção via CLI).

É a diferença entre "arquivo morto" e "cérebro que conecta" — e entrega **a parte que hoje falta** no local (o grafo linkado + as bases), que é justamente o que separa um second brain de uma pasta de notas.

### Quando vale / quando não
- **Vale muito** se você quer o grafo de verdade (links que conectam, bases consultáveis, a LLM mantendo o vault). É praticamente requisito.
- **Vale pouco** se você só quer resumos jogados em pastas e nunca navega links no Obsidian.

**Custo:** baixo. `obsidian-markdown`/`bases`/`canvas` são só conhecimento (zero dependência) — instala como plugin e pronto. Só `obsidian-cli` (precisa do Obsidian aberto) e `defuddle` (precisa do CLI) pedem instalação extra.

---

## 6. Camada de recuperação: `safishamsi/graphify` — ⭐ 69k (YC S26)

O graphify encaixa num lugar **diferente** de tudo acima — não é "o cérebro", é a **camada de recuperação** (retrieval).

### O que é
`/graphify .` varre um projeto (código, docs, PDFs, imagens, vídeos) e gera um **grafo de conhecimento consultável**:
- `graph.json` (o grafo), `graph.html` (clica/filtra no browser), `GRAPH_REPORT.md` (resumo).
- Você **consulta** em vez de grepar: `graphify query "what connects auth to the database?"`.
- Expõe um **MCP server** (`query_graph`, `get_node`, `get_neighbors`, `shortest_path`, `get_pr_impact`) → o agente pergunta ao grafo em vez de reler arquivos.
- Foco real: **entender codebase** (fluxo de auth, call-flow, impacto de PR). Generaliza pra docs, mas nasceu code-first. Tagline: "The Memory Layer". É produto (YC S26, PyPI `graphifyy`).

### Onde encaixa: é a peça de RETRIEVAL, não de organização

| | Obsidian / kepano | **graphify** |
|---|---|---|
| O grafo é... | **wikilinks que você lê/navega** | **índice que o agente consulta (MCP)** |
| Quem faz o grafo | você ou a LLM **escrevem** (`[[link]]`) | parser **auto-extrai** dos arquivos |
| Voltado pra | **humano** (notas que você abre) | **agente** (query estruturada) |
| Brilha em | conhecimento pessoal, notas | **codebase**, "não releia tudo, pergunte ao grafo" |

Ele responde **a metade que faltava no loop** — *"como a LLM recupera de uma pilha grande sem reler tudo"*. Karpathy/Obsidian resolvem isso fazendo uma **wiki de notas**; o graphify resolve fazendo um **grafo + MCP que o agente consulta**. É RAG-por-grafo, não wiki-por-notas.

### A resposta pro caso do local
- **Conhecimento pessoal / notas que você navega** (caso do inematds/local) → stack certo é **Obsidian + kepano**. graphify aqui é overkill/desalinhado: entrega grafo pra *agente consultar*, não wiki *pra você ler*.
- **Codebase que o agente precisa entender** → graphify é **o melhor da lista pra isso** (é a função dele). Nenhum second brain de notas faz call-flow/PR-impact.
- **Podem coexistir:** Obsidian/kepano = memória voltada pra **você**; graphify = índice de recuperação voltado pro **agente**, sobre os mesmos arquivos (ou sobre código).

### O mapa completo das camadas

```
ORGANIZAÇÃO   Luispitik · inematds/local · arscontexta   ← o quê / como organizar (o cérebro)
FORMATO       kepano/obsidian-skills                     ← escrever .md/.base/.canvas válidos (as mãos)
RECUPERAÇÃO   graphify                                   ← grafo + MCP que o agente consulta (a busca)
VISUALIZAÇÃO  brain-map                                  ← grafo bonito que o humano explora (a vista)
```

**Resumo de uma linha:** *kepano* = escrever o grafo certo · *Obsidian* = o grafo que **você lê** · *graphify* = o grafo que o **agente consulta** (code-first) · *brain-map* = o grafo que o **humano explora** (standalone).

---

## 7. Camada de visualização: `zubair-trabzada/brain-map` — ⭐ 16

Uma **5ª camada** que nenhuma das anteriores cobria: **visualização / navegação humana**. É o grafo do Obsidian destacado como produto standalone.

### O que é
Python zero-dependência (~500 linhas) que pega **qualquer pasta de markdown** e gera um **grafo interativo em HTML** (anima o "cérebro crescendo", zoom/pan/busca, cor por pasta, tamanho do nó por nº de conexões). Lê links de `[[wikilinks]]` e links markdown relativos. 100% local em `localhost:4710`, regenera em ~1s.

**Detalhe-chave:** trata o `CLAUDE.md` como nó **"router"** (central) — ou seja, **não é pra vault Obsidian clássico, é pra pasta "AI operating system" / Claude Code** (o cérebro estilo Ratos OS). É a ponte AI-OS → grafo.

### O diferencial
**É só o grafo do Obsidian — sem o Obsidian.** Standalone, leve, zero-dep, tunado pra pastas de CLAUDE.md. **Não organiza, não ingere, não escreve, não consulta** — só *lê* os `.md` + links e *desenha*.

### brain-map vs graphify (os dois fazem "grafo", públicos opostos)

| | **graphify** | **brain-map** |
|---|---|---|
| Pra quem | o **agente** consultar (MCP) | o **humano** olhar/explorar |
| Função | recuperação (retrieval) | visualização |
| Foco | code-first | notas / AI-OS |

Complementar ao **kepano:** kepano garante `[[wikilinks]]` válidos → brain-map então tem o que desenhar (sem link bom, o grafo vem vazio).

### Modelo de negócio (isca → curso)
O free (repo) é demo; o **Brain Studio** completo é exclusivo de membros pagantes da Skool (AI Workshop): 2D (v1), 3D galaxy fly-through (v2), v3 a caminho, preview de nota no grafo, path finder, isolate/pin, depth dial, heatmap, temas, **white-label pra logo de cliente**, skill `/brain-map`. Mesmo funil do graphify e do Ratos OS. O white-label revela o uso real: **mostrar pro cliente o "cérebro do negócio" dele** — ferramenta de *demo/venda* (o autor vende montar isso pra empresas por ~US$2–3k + retainer), não de pensar.

### A ironia com a escola "anti-Obsidian"
O argumento "a única coisa que o Obsidian dá é o gráfico bonitinho, e é inútil" encontra aqui a refutação comercial: *"quer o gráfico? toma — standalone, sem travar o Mac, na sua pasta de CLAUDE.md, sem Obsidian."* Os dois podem estar certos: o grafo é mais **exploração/demo do que trabalho** — *e* tem gente pagando pela versão 3D. A verdade fica no meio.

### Valor pra você (honesto)
- **Ferramenta de trabalho:** baixo (não organiza nem recupera).
- **Termômetro:** útil — vê o quão conectado seu conhecimento está (acha nota órfã, área fraca) e **te premia por linkar** (boa higiene de wikilink, amarra com o kepano).
- **Demo:** se for mostrar "o cérebro" pra alguém, é exatamente isso.

**Resumo:** é a camada de **Visualização** — o item de menor peso funcional, mas que **completa o mapa** (organização → formato → recuperação → visualização).

---

## 8. Os 3 tipos de cérebro (o frame que organiza tudo)

"Second brain" não é uma coisa só. Existem **3 tipos de conteúdo com necessidades opostas** — e batem com os 3 tipos de memória da ciência cognitiva. Tratar os três como um monte único é o erro clássico que faz o cérebro "inchar".

| | Nome | Tipo de memória | Natureza | O que guarda |
|---|---|---|---|---|
| **1** | **Projeto** | Episódica / operacional | cronológica, muda toda hora, depois arquiva | o que rolou, estado, decisões, "por que fizemos X" |
| **2** | **Self** | Identidade + reflexiva | poucos e estáveis (valores) + vivos e abertos (dúvidas) | quem você é, no que crê, debates não resolvidos |
| **3** | **Conhecimento** | Semântica / referência | vasta, cresce pra sempre, impessoal | fatos do mundo, o que você aprendeu |

### Por que separar: necessidades opostas
- **Projeto** quer ser **fresco e cronológico** → `daily/`, `projects/`, `decisions/log.md`. Append-only, arquiva o velho.
- **Self** quer ser **poucos e estáveis** (valores) + **conectados** (ideias e dúvidas que se linkam) → `me.md`/`values.md` + grafo de ideias. Muda devagar.
- **Conhecimento** quer ser **enorme e consultável** → a wiki de verdade, o loop raw→wiki.

Misturar os três incha o cérebro: coisa estável (valor) no meio de coisa volátil (log de ontem) e de coisa infinita (referência).

### Cada cérebro pede ferramenta — e ativa um lado diferente da LLM

| Cérebro | Como organizar | Ferramenta | A LLM brilha... |
|---|---|---|---|
| **1. Projeto** | log datado + pasta por projeto | vault inematds (`projects/`, `daily/`, `tldr`) | **registrando fiel** (decisão+razão+data) |
| **2. Self** | poucos arquivos de valor + grafo de ideias | Obsidian (links/serendipidade) + LLM socrática | **pensando junto** (te confronta, levanta contradição) |
| **3. Conhecimento** | raw → LLM → wiki, e consultar | `file-intel` + kepano + graphify/Bases | **organizando e recuperando** (raw→wiki, query) |

### Resumo
Três cérebros: **Projeto (episódico)** · **Self (identidade/reflexão)** · **Conhecimento (referência)**. Não tratar como um só — **três espaços separados, com regras e ferramentas próprias, conversando com a mesma LLM.** Este frame é o que organiza todas as seções anteriores: as camadas (organização/formato/recuperação/visualização) e as ferramentas se distribuem pelos três tipos.

---

## 9. Conclusão geral

1. **Local = inematds** (fork PT-BR/Linux). Não é projeto independente.
2. **Três posições, não duas:** conversa pura (Luispitik) · raw→LLM→wiki (Karpathy: inematds, claude-obsidian, llm-wiki-agent) · Obsidian manual. O local está na posição Karpathy.
3. **Mercado validado e concorrido** — de 35k⭐ (khoj) a dezenas de variações Claude Code + Obsidian.
4. **Diferencial do local:** PT-BR + Linux (raro no nicho).
5. **kepano/obsidian-skills** não é rival — é fundação. Entrega o que falta (grafo linkado + bases) e fecha a alça do loop Karpathy.
6. **Quatro camadas distintas, não concorrentes:** organização (Luispitik/inematds) · formato (kepano) · recuperação (graphify, p/ agente) · visualização (brain-map, p/ humano). graphify é code-first; brain-map é a vista/demo (menor peso funcional).
7. **Três tipos de cérebro** (Projeto/Self/Conhecimento) é o frame que organiza tudo: cada um quer espaço, regra e ferramenta próprios — não misturar.

### Próximos passos sugeridos
- [ ] Acoplar `kepano/obsidian-skills` ao `~/projetos/second-brain` (file-intel gerando notas via `obsidian-markdown`; trocar fetch por `defuddle`).
- [ ] Comparar estrutura do local com `AgriciDaniel/claude-obsidian` (7k⭐) pra ver o que falta.
- [ ] Avaliar reposicionar o local como "second brain PT-BR para Linux" (nicho vazio).
