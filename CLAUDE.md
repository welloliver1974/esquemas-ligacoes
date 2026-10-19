# CLAUDE.md — Central de Arquivos (AppDiagDrive)

Contexto para o Claude Code em sessões futuras neste repositório.

## Visão geral

Landing page **estática, single-file** que funciona como uma central de arquivos de
manutenção: organiza 27 documentos técnicos (esquemas, manuais, circuitos, procedimentos)
de equipamentos médicos de radiologia/imagem (Digiscope, Tripter, HIWIN, LAP4, etc.) em
6 categorias, cada item com link direto para um arquivo no Google Drive.

Sem build, sem framework, sem dependências. É HTML + CSS + JS puros num único `index.html`.

## Arquitetura

Todo o app mora em `index.html` (~924 linhas). Dentro do `<script>` a lógica é 100%
data-driven:

- `CATEGORIAS` — array com `id`, `nome`, `icon` e `cor` (hex) de cada categoria.
  ids: `manuais`, `esquemas`, `pedais`, `display`, `agua`, `versoes`. (`icon` está em
  desuso no UI atual — a cor vira o filete lateral da seção/linhas.)
- `ARQUIVOS` — array dos itens. Cada item: `{ cat, nome, desc, link, tipo }`.
  `tipo` ∈ `pdf | png | jpg | docx | doc` → define o ícone SVG e a tag mono (mapa em `ICONES`).
- `renderSections()` — gera as seções e linhas de índice via template strings a partir de
  `CATEGORIAS` + `ARQUIVOS`. Cada linha expõe `data-search` (nome+desc+categoria, lowercase)
  para a busca.
- `filtrar()` — busca por texto + filtro por chip de categoria. Destaca o termo com `<mark>`
  em `h3`/`p`, atualiza o contador `#total-info`, a mensagem de "sem resultados" e o
  `document.title`. O nome original de cada elemento é guardado em `dataset.orig` para
  poder re-highlightar sem duplicar `<mark>`. É chamado uma vez no load pra mostrar o total.
- `IntersectionObserver` — aplica `section.in-view` para o fade de entrada discreto (150ms).

## Adicionar / editar arquivos

O item de dado é **a fonte da verdade**. Já existe um comentário-guia no topo do `<script>`
(em `index.html`, procure por "COMO ADICIONAR UM NOVO ARQUIVO"). Resumo:

1. Abra `index.html`, vá no array `ARQUIVOS`.
2. Copie uma linha na categoria desejada e troque os campos `cat`, `nome`, `desc`, `link`, `tipo`.
3. Pronto — a renderização, contagem, busca e filtro são automáticos.

Para mudar nomes/cores/ícones de categoria: edite `CATEGORIAS`.

## Tema (escuro / claro)

- Definido por `data-theme="escuro|claro"` no `<html>`, controlado pelo botão `#tema-toggle`.
- Persistido em `localStorage` chave `tema`.
- Script anti-flash no `<head>` aplica o tema salvo **antes** da primeira pintura.
- Tokens de cor são CSS custom properties nos blocos `[data-theme="escuro"]` e `[data-theme="claro"]`.
- A `meta[name=theme-color]` é atualizada via `MutationObserver` para acompanhar o tema (Android).
- `prefers-reduced-motion` desliga animações.

## Rodar / preview local

Não há servidor nem build. Para visualizar:

- Abrir `index.html` diretamente no navegador, **ou**
- Servir estaticamente: `python -m http.server` (ou similar) na raiz e acessar `localhost:8000`.

## Deploy (GitHub Pages)

Alvo de publicação: `https://welloliver1974.github.io/esquemas-ligacoes/`
(referenciado nas `og:url`/`og:image` do `<head>`).

- Repo: `github.com/welloliver1974/esquemas-ligacoes` (público), branch padrão `master`.
- Deploy via **GitHub Pages** servindo o branch `master` (root).

## Estrutura de arquivos

```
AppDiagDrive/
├── index.html     # todo o app (HTML + CSS + JS)
├── logo.png       # logo da topbar (primária)
├── logo.jpg       # fallback do logo (onerror)
└── .gitignore     # ignora node_modules, .claude/, *.bak, *.log
```

## Convenões

- Tudo em português (pt-BR): textos, labels, comentários.
- Sem dependências externas além das fontes Google (Inter / Space Grotesk) carregadas via CDN.
- Estilo: visual de manual técnico — superfícies sólidas (sem glassmorphism), sem
  animações infinitas, ícones SVG inline (sem emoji na UI), tags/filetes em
  IBM Plex Mono, texto em IBM Plex Sans. Paleta cinza-chumbo + azul de destaque.
