# STATUS — manhã de 2026-06-20

Resumo do que rodou autônomo enquanto você dormia. Os **3 pontos** (build → teach → publish), na ordem que combinamos.

---

## Visão rápida

| Ponto | O quê | Status | Onde |
|---|---|---|---|
| 0 | Repo histórico atualizado | ✅ | github.com/inematds/2cerebrox |
| 1 | **BUILD** — produto cerebro-inema | ✅ | github.com/inematds/cerebro-inema |
| 2 | **TEACH** — landing+guia (GitHub Pages) | ✅ no ar | inematds.github.io/cerebro-inema |
| 3 | **PUBLISH** — vídeo explicativo | ⏳ ver §3 | ~/projetos/output/cerebro-inema-video |

---

## 0 · Repo histórico (2cerebrox)
A pesquisa toda ficou aqui como histórico. Adicionei a §7 (camada de Visualização / brain-map) e este STATUS.md. O doc principal `docs/comparacao-segundo-cerebros.md` está com 9 seções; `docs/estrutura-3-cerebros.md` tem a proposta de arquitetura.

## 1 · BUILD — cerebro-inema (✅)
Produto novo, **público**: https://github.com/inematds/cerebro-inema

Implementa a arquitetura dos 3 cérebros (decisões em aberto fechadas com defaults — ver lista abaixo):
- **Estrutura:** `0-inbox/ 1-projeto/ 2-self/ 3-conhecimento/` (vault-template)
- **CLAUDE.md roteador** — mapa dos 3 cérebros + regras + roteamento
- **Skills:** `triagem`, `socratica`, `nota` (novas) + `daily`, `tldr`, `setup-cerebro` (adaptadas) — todas em texto livre, sem AskUserQuestion (sua regra global)
- **setup.sh** PT-BR/Linux+mac — **testado** (smoke test passou, gera o vault completo)
- **docs/** arquitetura (3 cérebros × 4 camadas) + integração kepano
- Scripts Gemini (ingestão raw→wiki) reaproveitados do fork

**Defaults que bati (reverta se quiser):**
1. Vault único (não três separados)
2. Prefixo numérico nas pastas (`1-projeto/`…)
3. Journal (self) separado do daily (projeto)
4. Pergunta resolvida migra de `2-self/questions` → `3-conhecimento/notes`
5. graphify fora do core (documentado como opcional)
6. Nomes de pasta + conteúdo em PT-BR

## 2 · TEACH — landing+guia (✅ no ar)
Página única INEMA dark âmbar, dark/light toggle, INEMA.CLUB no nav e footer.
- **No ar:** https://inematds.github.io/cerebro-inema/ (HTTP 200 confirmado)
- Fonte: `index.html` na raiz do cerebro-inema (self-contained)
- Seções: hero (diagrama dos 3 cérebros) · o que é · como funciona · pré-requisitos · guia passo a passo (comandos reais) · exemplos · as 4 camadas · footer

## 3 · PUBLISH — vídeo explicativo (⏳)
Projeto HyperFrames em `~/projetos/output/cerebro-inema-video/`.
- **Roteiro** `SCRIPT.md` — 8 cenas + CTA INEMA.CLUB, ~2:00, PT-BR
- **Narração** gerada (voz pf_dora, 9 WAVs em assets/audio/)
- **Composição** `build-index.mjs` adaptada (3 cérebros), lint **0 erros**
- **Render:** ✅ os dois formatos em alta, conferidos por frame:
  - `renders/cerebro-inema-16x9.mp4` — 1920×1080 · **120,6s** · 87 MB
  - `renders/cerebro-inema-9x16.mp4` — 1080×1920 · **120,6s** · 83 MB
- **Bug corrigido no caminho:** o template tinha overshoot de ~43s (cauda vazia,
  vídeo ia a 162s). Limitei os tweens de ambiente ao TOTAL → fecha em ~120s na CTA.
- **Conteúdo conferido** (frames): cena 1 (hero "Seu cérebro em três"), cena 4
  (os 3 cérebros), CTA INEMA.CLUB, e frame vertical (safe zones ok).

**As 8 cenas:** 1) hook amnésia · 2) o cérebro são os arquivos / CLAUDE.md é o mapa ·
3) 3 memórias, não misturar · 4) os 3 cérebros (Projeto/Self/Conhecimento) ·
5) instalar é um comando · 6) as skills · 7) é real, PT-BR/Linux · 8) sobrevive ao
hype · + CTA INEMA.CLUB.

---

## Próximos passos sugeridos (pra quando acordar)
- Revisar a landing e o vídeo draft; ajustar texto/voz se quiser.
- Rodar o `cerebro-inema` de verdade: `git clone … && ./setup.sh && /setup-cerebro` (dogfooding).
- Decidir se publica o vídeo (YouTube/Shorts) e se a landing entra no portal inema.club.
- Rever os 6 defaults da arquitetura (acima) e mudar o que não fizer sentido.
