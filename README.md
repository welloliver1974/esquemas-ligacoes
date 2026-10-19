# Central de Arquivos — Esquemas e Ligações 🧩

Landing page estática que reúne, em um só lugar, esquemas, manuais e documentos de
manutenção das máquinas (radiologia/imagem) — Digiscope, Tripter, HIWIN, LAP4, etc.
Cada arquivo abre direto do Google Drive.

> HTML + CSS + JavaScript puros, num único `index.html`. Sem build, sem dependências.

## ✨ Recursos

- 📂 **27 arquivos** organizados em **6 categorias** (manuais, esquemas, pedais, display,
  sistema de água, versões/downgrade)
- 🎨 Visual de manual técnico com **modo claro/escuro** (persistente via `localStorage`)
- 🔍 **Busca** por texto com destaque do termo + **filtros por categoria** (chips)
- 🧾 **Índice por categoria** — filete de cor por seção e tags mono de tipo do arquivo
- 📱 **Responsivo** (mobile, tablet e desktop)
- ♿ Respeita `prefers-reduced-motion` e usa `aria-live` nos resultados
- ⚡ Zero build, zero dependências externas além das fontes do Google

## 🗂️ Categorias

| Categoria | Ícone | Conteúdo |
|---|---|---|
| Manuais | 📘 | Manuais de equipamentos |
| Esquemas e Circuitos | 🧩 | Circuitos de pulso, mesa, PCB, cabos |
| Pedais e Conectores | 🔌 | Pedais e controle remoto |
| Display e Configuração | 🖥️ | Display Nusp e ligações |
| Sistema de Água (Tripter) | 💧 | Esquemas do sistema de água |
| Versões e Downgrade | 🔄 | Procedimentos de downgrade |

## 🚀 Como ver / rodar localmente

Não há servidor nem build. Basta **abrir o `index.html` no navegador**.

Para servir estaticamente (opcional):

```bash
python -m http.server
# acesse http://localhost:8000
```

## 🛠️ Como adicionar um arquivo

O conteúdo é **data-driven**. Edite o array `ARQUIVOS` no `index.html`:

```js
{ cat: 'esquemas', nome: 'Circuito de Pulso (v3)', desc: 'Descrição curta.',
  link: 'https://drive.google.com/file/d/<ID>/view', tipo: 'pdf' },
```

Campos:

- `cat` — categoria: `manuais | esquemas | pedais | display | agua | versoes`
- `nome` — nome do arquivo
- `desc` — descrição curta (aparece no card)
- `link` — link de compartilhamento do arquivo no Google Drive
- `tipo` — `pdf | png | jpg | docx | doc` (define o ícone e o selo do card)

A renderização, a contagem, a busca e os filtros são automáticos. Há um comentário-guia
no topo do `<script>` do `index.html` com essas mesmas instruções.

Para alterar nome/cor/ícone de uma categoria: edite o array `CATEGORIAS`.

## 📁 Estrutura

```
.
├── index.html     # todo o app (HTML + CSS + JS)
├── logo.png       # logo (primária)
└── logo.jpg       # logo (fallback)
```

## 🌐 Deploy

Publicação via **GitHub Pages** servindo o branch `master`.
URL: <https://welloliver1974.github.io/esquemas-ligacoes/>

## 📝 Licença

Uso interno de manutenção.
