# CLAUDE.md — Manual do Vinho

Livro aberto em **Quarto**, quinto título da série *Manuais de Ciências* (irmão
dos manuais de Matemática, Física, Química e Filosofia). Vai da videira no
vinhedo à taça na mesa, do básico ao avançado. Publicado no GitHub Pages.

## Comandos

```bash
quarto preview          # site local com hot-reload — uso diário
quarto render           # gera site + PDF em _book/ (PDF exige TeX)
quarto publish gh-pages # render + deploy manual (raramente necessário)
```

- Deploy normal é **automático**: todo `push` na `main` dispara o GitHub Action
  (`.github/workflows/publish.yml`), que renderiza e publica.
- **PDF exige TeX local** (`quarto install tinytex`). Sem TeX, o render do PDF
  falha com `No TeX installation was detected`. Para trabalhar só no site numa
  máquina sem TeX, comente o bloco `pdf:` em `_quarto.yml`.

## Bootstrap do toolchain (rodar no início da sessão)

As figuras exigem TeX **mesmo só para o site**. Para não depender de o usuário
lembrar, ao iniciar uma sessão verifique e, se necessário, instale o toolchain:

```bash
quarto install tinytex
# tlmgr costuma não ficar no PATH após o passo acima; localize-o e use o caminho:
TLMGR="$(command -v tlmgr || find "$HOME" / -name tlmgr 2>/dev/null | head -1)"
"$TLMGR" install standalone pgf pgfplots dvisvgm xcolor amsmath amsfonts
```

Se `quarto preview` reclamar de `pdflatex`/`dvisvgm` ausente, é sinal de que
este passo foi pulado.

## FIGURAS (TikZ) — leia antes de mexer

Figuras são escritas em **TikZ dentro do próprio `.qmd`** e compiladas para SVG
(site) ou PDF (livro) pela extensão local `_extensions/danmackinlay/tikz/`.

- **NUNCA** rode `quarto add`/`quarto update` para essa extensão: o upstream vem
  **sem os patches locais** (Windows, SVG inline, saída PDF) e quebra em
  silêncio. Os arquivos corretos já estão no repositório.
- **Requer TeX mesmo só para o site.** Cada figura é compilada por
  `pdflatex → dvisvgm`. Numa máquina sem TeX, o `quarto preview` quebra em
  qualquer capítulo com figura. Instale uma vez:

  ```bash
  quarto install tinytex
  # depois, no bin do TinyTeX da sua plataforma:
  tlmgr install standalone pgf pgfplots dvisvgm xcolor amsmath amsfonts
  ```

- **CI** (`publish.yml`) já instala esses pacotes no runner e o
  `chrome-headless-shell` (o grafo `mermaid` do `index.qmd` precisa dele para
  virar imagem no PDF).
- Sintaxe (ver o gabarito `volumes/v1-mundo-do-vinho/04-da-uva-a-taca.qmd`):

  ````markdown
  ::: {#fig-nome}
  ```{.tikz}
  %%| filename: nome-do-arquivo
  %%| alt: Texto alternativo descritivo (acessibilidade).
  \begin{tikzpicture}
    ...
  \end{tikzpicture}
  ```

  Legenda da figura, terminando em ponto.
  :::
  ````

  O **id `#fig-nome` vai na `div`**, não no bloco de código. Referencie sempre
  com `@fig-nome` (nunca "Figura 4.1" na mão). Estilos prontos no template:
  `curva`, `destaque`, `auxiliar`, `eixo`, `ponto`, `vetor`; cores `manualblue`,
  `manualred`, `manualgreen`, `manualyellow`, `manualgray`. Preâmbulo e paleta
  são centralizados no `tikz.lua` — não repita `\usepackage` por figura.

## Estrutura

```
_quarto.yml   config do livro (estrutura, formatos, lang: pt no root, filtro tikz ANTES de quarto)
index.qmd     apresentação + grafo de pré-requisitos (mermaid)
glossario.qmd glossário (apêndice)
references.bib bibliografia (BibTeX)
styles.css    identidade visual do site
PLANO.md      guia de estilo + decisões técnicas (FONTE DA VERDADE de estilo)
ROADMAP.md    fila executável dos 13 volumes / 67 capítulos, com status [ ]/[x]
volumes/      capítulos, em volumes/vN-tema/NN-nome.qmd
_extensions/  extensão TikZ (patcheada — não atualizar pelo `quarto add`)
.github/      deploy automático
```

## Convenções de escrita (sempre seguir)

- **Gabarito de estilo:** `volumes/v1-mundo-do-vinho/04-da-uva-a-taca.qmd`. Ao
  escrever qualquer capítulo, espelhe a estrutura dele.
- Estrutura padrão: **Motivação → Conceitos → Figura(s) → Exemplos →
  Exercícios → Soluções selecionadas**.
- Idioma **português**; tom didático antes de formal — sempre motivar antes de
  definir.
- Ambientes (numeram e referenciam sozinhos), com `## Título` interno opcional:
  - `::: {#def-nome}` definição · `prp-` proposição · `exm-` exemplo · `exr-` exercício
  - caixas: `.callout-note` (observação), `.callout-warning` (erro comum),
    `.callout-tip collapse="true"` (solução escondida).
- Referência cruzada: `@def-nome`, `@fig-nome`, `@exm-nome`. **Nunca** escreva o
  número na mão.
- Há um pouco de matemática quando útil (teor alcoólico, açúcar, acidez):
  `$...$` em linha, `$$...$$` em destaque.

## Regras do projeto

- **Não** edite numerações, sumário ou rótulos manualmente — o Quarto gera tudo.
- Ao adicionar capítulo: criar o `.qmd` E registrá-lo em `chapters:` do
  `_quarto.yml`. Para abrir um volume novo, descomente o bloco `part:`.
- Mantenha `glossario.qmd` e a tabela do `ROADMAP.md` atualizados.
- **Fatia vertical:** feche o Volume I inteiro (e publique) antes de abrir o II.
- Antes de propor reestruturação grande, consulte `PLANO.md` e `ROADMAP.md`.
- Tema sensível: vinho envolve álcool. Mantenha tom técnico/cultural, mencione
  moderação quando couber, nunca incentive consumo excessivo.
- Commits pequenos e descritivos, um por capítulo/seção concluída.
