# PLANO.md — Manual do Vinho (guia de estilo e decisões)

Fonte da verdade de **estilo e decisões técnicas**. A fila de capítulos está no
`ROADMAP.md`. Atualize este arquivo sempre que uma convenção mudar.

## Por que Quarto (e não outra ferramenta)

O manual do vinho herda a mesma stack dos manuais irmãos — **livro Quarto →
GitHub Pages** — e a decisão se sustenta bem aqui:

- **Livro longo, multi-volume, com sumário, busca e referências cruzadas
  automáticas** (`@fig-`, `@def-`, `@exm-`): exatamente o forte do Quarto book.
- **Dois formatos de uma fonte só:** site HTML (consulta) e PDF (leitura/
  impressão), sem manter dois documentos.
- **Conteúdo versionável em texto puro** (`.qmd`), ideal para escrever junto com
  o Claude Code, commit a commit.
- **Figuras como código** (TikZ), versionáveis e idênticas em HTML e PDF.

A única ressalva real frente aos manuais de ciências exatas: o vinho pede também
**fotografias** (cor dos vinhos, cachos, paisagens de regiões) que o TikZ não
faz. A divisão de trabalho adotada:

- **TikZ** para o que é **esquemático/didático**: fluxogramas (vinificação),
  ciclo da videira, anatomia da taça, corte de terroir, roda de aromas, linhas
  do tempo, mapas estilizados de regiões.
- **Imagem embutida** (`.jpg`/`.png`/`.svg` em `imagens/`) para o que precisa de
  **realismo**: fotos de uvas, rótulos, paisagens. Inserir com
  `![Legenda](imagens/arquivo.jpg){#fig-nome}` e referenciar por `@fig-nome`.
  Preferir imagens de domínio público / licença livre e creditar a fonte.

## Estrutura do capítulo (padrão)

Motivação → Conceitos → Figura(s) → Exemplos → Exercícios → Soluções
selecionadas. Gabarito vivo: `volumes/v1-mundo-do-vinho/04-da-uva-a-taca.qmd`.

## Ambientes e referências

- `::: {#def-nome}` definição · `prp-` proposição · `exm-` exemplo · `exr-` exercício.
- Caixas: `.callout-note` (observação), `.callout-warning` (erro comum),
  `.callout-tip collapse="true"` (solução escondida).
- Referência cruzada sempre por rótulo: `@def-nome`, `@fig-nome`. Nunca o número
  na mão.
- Termos técnicos novos vão para o `glossario.qmd`.

## Figuras (resumo operacional)

Detalhe completo no `CLAUDE.md` e no `FIGURAS.md`. Pontos-chave:

1. Extensão local patcheada em `_extensions/danmackinlay/tikz/` — **não**
   atualizar via `quarto add`.
2. `_quarto.yml`: filtro `danmackinlay/tikz` **antes** de `quarto`; `tikz:
   svg-engine: dvisvgm`.
3. Toolchain local: `quarto install tinytex` + `tlmgr install standalone pgf
   pgfplots dvisvgm xcolor amsmath amsfonts`. (CI já faz isso.)
4. Sintaxe: `div #fig-nome` + bloco `{.tikz}` com `%%| filename:` e `%%| alt:`,
   legenda antes do `:::`.

## Identidade visual

- Site: `styles.css` (acento **bordeaux** + dourado).
- Figuras: paleta centralizada no template do `tikz.lua`
  (`manualblue/red/green/yellow/gray`). Para retintar as figuras à identidade do
  vinho (ex.: bordeaux como primária), edite os `\definecolor` nesse template —
  é o único ponto a mexer, e vale para todas as figuras.
- Manter centralizado preserva o padrão visual da série inteira.

## Roadmap em uma linha

13 volumes, 67 capítulos, em 6 fases: **Fundamentos → Matéria-prima →
Vinificação → Estilos e Regiões → Degustação e Serviço → Cultura e Mercado.**
Regra da **fatia vertical**: fechar o Volume I (e publicá-lo) antes de abrir o
Volume II. Detalhe completo no `ROADMAP.md`.
