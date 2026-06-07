# Manual do Vinho

Quinto título da série *Manuais de Ciências* — um livro aberto que vai da
videira no vinhedo à taça na mesa. Construído com [Quarto](https://quarto.org) e
publicado no GitHub Pages.

**📖 Leia online:** https://vitormattosdev.github.io/manual-vinhos/

## Pré-requisitos

- [Quarto](https://quarto.org/docs/get-started/) (CLI).
- **TeX** (necessário mesmo só para o site, por causa das figuras TikZ):

  ```bash
  quarto install tinytex
  # depois, no bin do TinyTeX da sua plataforma (ex.: bin/windows ou bin/x86_64-linux):
  tlmgr install standalone pgf pgfplots dvisvgm xcolor amsmath amsfonts
  ```

  Sem TeX, o `quarto preview` quebra em qualquer capítulo com figura.

## Construir localmente

```bash
quarto preview     # abre o site no navegador e recarrega ao salvar
quarto render      # gera o site (e o PDF) em _book/
```

## Publicar no GitHub Pages

Deploy automático: cada `push` na branch `main` dispara o GitHub Action em
`.github/workflows/publish.yml`, que instala o toolchain (TinyTeX + pacotes
LaTeX das figuras + `chrome-headless-shell` para o grafo mermaid), renderiza e
atualiza o site.

Configuração inicial (uma vez):

1. Crie um repositório no GitHub chamado `manual-vinhos` e envie estes arquivos
   para a branch `main`.
2. Troque `SEU-USUARIO` no `_quarto.yml` pelo seu usuário/organização.
3. No GitHub: **Settings → Pages → Source: Deploy from a branch**, branch
   **`gh-pages`** (criada pelo Action no primeiro deploy), pasta **`/ (root)`**.

O site ficará em `https://SEU-USUARIO.github.io/manual-vinhos/`.

## Figuras (TikZ)

Escritas em TikZ dentro dos `.qmd` e compiladas para SVG (site) / PDF (livro)
pela extensão local em `_extensions/danmackinlay/tikz/`. **Não** atualize essa
extensão via `quarto add` — ela tem patches locais. Detalhes no `CLAUDE.md`,
`PLANO.md` e `FIGURAS.md`.

## Estrutura

```
_quarto.yml      configuração do livro
index.qmd        apresentação + grafo de pré-requisitos
glossario.qmd    glossário (apêndice)
PLANO.md         guia de estilo + decisões técnicas
ROADMAP.md       fila dos 13 volumes / 67 capítulos
CLAUDE.md        contrato do projeto para o Claude Code
styles.css       identidade visual
volumes/         capítulos
_extensions/     extensão TikZ (patcheada)
.github/         deploy automático
```

## Licença

Conteúdo sob CC BY-SA 4.0; código sob MIT (sugestão — ajuste se quiser).
