# CSS Modular

Quando um projeto começa a crescer, um único arquivo `style.css` vira um problema. Você rola a página procurando onde está a cor do botão. Altera uma classe e quebra outra parte do layout. Tem medo de apagar qualquer coisa porque não sabe de onde vem.

CSS modular é a solução para isso: organizar o CSS de forma que cada parte do projeto tenha um lugar previsível, um propósito claro e um impacto controlado.

Neste módulo vamos aprender três técnicas que funcionam juntas, em HTML e CSS puros, sem nenhuma ferramenta extra:

1. **Custom Properties** — variáveis que funcionam como o "manual de identidade" do projeto
2. **Arquitetura de pastas** — onde cada arquivo vive e por quê
3. **BEM** — como nomear classes sem causar conflitos

A ordem importa: vamos aprender variáveis primeiro porque todos os exemplos de organização de arquivos as utilizam.

---

## 1. Custom Properties — variáveis CSS

### O problema

```css
/* A cor primária aparece em 20 lugares diferentes */
.button    { background: #3b82f6; }
.link      { color: #3b82f6; }
.badge     { border: 2px solid #3b82f6; }
.progress  { background: #3b82f6; }

/* O cliente pediu para trocar o azul por verde.
   Agora você vai procurar #3b82f6 em cada arquivo? */
```

Valores repetidos em muitos lugares causam dois problemas: qualquer mudança exige busca e substituição manual em vários arquivos, e é fácil esquecer algum e deixar o projeto inconsistente.

### A solução

Definir cada valor **uma única vez**, com um nome descritivo, e referenciá-lo em todo o projeto.

```css
:root {
  --color-primary: #3b82f6;  /* define aqui uma vez */
}

.button   { background: var(--color-primary); }  /* usa aqui */
.link     { color: var(--color-primary); }       /* e aqui  */
.badge    { border: 2px solid var(--color-primary); } /* e aqui */
```

Agora para trocar a cor primária: um único caractere, em um único lugar.

---

### Anatomia de uma custom property

```css
/* Definição — sempre no :root */
:root {
  --nome-da-variavel: valor;
}

/* Uso — em qualquer seletor */
.elemento {
  propriedade: var(--nome-da-variavel);

  /* Com valor de fallback, caso a variável não exista */
  propriedade: var(--nome-da-variavel, valor-substituto);
}
```

Três regras para lembrar:

- O nome começa **sempre com dois hifens**: `--color-primary`, `--spacing-md`
- São definidas no `:root` para ficarem disponíveis em **toda a página**
- São lidas com `var()`

---

### Passo a passo — criando o sistema de variáveis

**Passo 1** — Crie ou abra o arquivo `css/base.css`. As variáveis ficam sempre aqui, no topo do arquivo, antes de qualquer outra regra.

**Passo 2** — Escreva o bloco `:root` organizando as variáveis em grupos com comentários. Use este modelo como ponto de partida:

```css
/* base.css */

:root {
  /* ─── Cores ─────────────────────────────────────── */
  --color-primary:      #3b82f6;   /* azul principal */
  --color-primary-dark: #1d4ed8;   /* azul escuro — usado em :hover */
  --color-danger:       #ef4444;   /* vermelho para erros */
  --color-success:      #22c55e;   /* verde para confirmações */

  --color-text:         #1e1e1e;   /* texto principal */
  --color-text-muted:   #6b7280;   /* texto secundário, placeholders */
  --color-bg:           #ffffff;   /* fundo da página */
  --color-surface:      #f9fafb;   /* fundo de cards e painéis */
  --color-border:       #e5e7eb;   /* bordas em geral */

  /* ─── Espaçamentos ───────────────────────────────── */
  --spacing-xs:   0.25rem;   /*  4px */
  --spacing-sm:   0.5rem;    /*  8px */
  --spacing-md:   1rem;      /* 16px */
  --spacing-lg:   2rem;      /* 32px */
  --spacing-xl:   4rem;      /* 64px */
  /* ↑ rem explicado logo abaixo */

  /* ─── Tipografia ─────────────────────────────────── */
  --font-body:       'Inter', sans-serif;
  --font-size-sm:    0.875rem;   /* 14px */
  --font-size-base:  1rem;       /* 16px */
  --font-size-lg:    1.25rem;    /* 20px */
  --font-size-xl:    1.5rem;     /* 24px */
  --font-size-2xl:   2rem;       /* 32px */

  /* ─── Bordas ─────────────────────────────────────── */
  --radius-sm:   4px;
  --radius-md:   8px;
  --radius-lg:   16px;
  --radius-full: 9999px;   /* formato de pílula */
}
```

### O que é `rem` — e por que usar em vez de `px`

Você deve ter notado que os espaçamentos e tamanhos de fonte usam `rem` em vez de `px`. Entender essa unidade é essencial antes de continuar.

**`px` é um tamanho absoluto.** `16px` sempre vai ser 16 pixels, independente de qualquer configuração.

**`rem` é relativo ao tamanho de fonte da raiz da página** — que por padrão nos navegadores é `16px`. Por isso a conversão é simples:

```
1rem   = 16px  (padrão do navegador)
0.5rem =  8px
0.25rem=  4px
2rem   = 32px
```

Parece mais complicado, mas tem uma vantagem enorme: **respeita as preferências de acessibilidade do usuário**. Quando alguém aumenta o tamanho de fonte padrão do navegador nas configurações do sistema, tudo que usa `rem` escala junto automaticamente. Tudo que usa `px` fica do mesmo tamanho e pode se tornar ilegível.

```css
/* ✗ Ruim — ignora a preferência do usuário */
body { font-size: 16px; }
h1   { font-size: 32px; }

/* ✓ Bom — respeita a preferência do usuário */
body { font-size: 1rem; }
h1   { font-size: 2rem; }
```

**Quando usar `px` mesmo assim?** Para valores que realmente não devem escalar com o texto: `border: 1px solid`, `border-radius: 4px`, `box-shadow`. Bordas finas e cantos arredondados em `px` são aceitáveis — eles não impactam legibilidade.

```css
/* Regra prática */
font-size, padding, margin, gap, width, height → rem
border-width, border-radius, box-shadow        → px
```

**Passo 3** — Escreva os estilos globais do `body` logo abaixo, **usando as variáveis** que acabou de definir. Nunca use valores diretos aqui:

```css
/* Reset */
*, *::before, *::after {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

/* Estilos globais */
body {
  font-family: var(--font-body);
  font-size: var(--font-size-base);
  color: var(--color-text);
  background-color: var(--color-bg);
  line-height: 1.6;
}

h1, h2, h3, h4 {
  line-height: 1.2;
}

a {
  color: var(--color-primary);
}

img {
  max-width: 100%;
  display: block;
}
```

**Passo 4** — Nos outros arquivos (layout, components), **nunca escreva cores ou tamanhos fixos**. Sempre use `var()`. Se precisar de uma cor que ainda não existe nas variáveis, volte ao `base.css` e adicione lá.

**Passo 5** — Para testar se tudo está funcionando: mude `--color-primary` para outra cor e veja se tudo que usa essa variável muda junto. Se algo ficou azul, é porque ainda tem um valor fixo `#3b82f6` em algum lugar.

---

### Tema escuro com variáveis

A maior vantagem das custom properties é o tema escuro: você só redefine as variáveis, e os componentes se adaptam automaticamente — sem tocar em nenhum arquivo de componente.

#### Media queries além do tamanho de tela

Você já conhece `@media` para tamanhos de tela: `@media (max-width: 768px)`. Mas media queries detectam muito mais do que largura — elas conseguem ler **preferências do sistema operacional do usuário**.

Uma dessas preferências é o tema: se o usuário configurou o sistema para tema escuro (Windows, macOS, Android, iOS), o navegador expõe isso via media query:

```css
@media (prefers-color-scheme: dark) {
  /* CSS aplicado automaticamente quando o SO usa tema escuro */
}
```

Funciona exatamente como as media queries de tamanho que você já usa — a diferença é que a condição é uma preferência do sistema, não uma medida de pixel.

Outras media queries de preferência que existem:

```css
@media (prefers-reduced-motion: reduce) {
  /* Usuário pediu para reduzir animações — desative transitions e animations aqui */
}

@media (prefers-contrast: high) {
  /* Usuário pediu alto contraste */
}
```

#### Aplicando tema escuro automático e manual

A estratégia mais completa combina os dois: o sistema detecta a preferência do SO automaticamente, mas o usuário pode sobrescrever via botão.

```css
/* base.css */

/* Tema claro — padrão para todos */
:root {
  --color-bg:      #ffffff;
  --color-surface: #f9fafb;
  --color-text:    #1e1e1e;
  --color-border:  #e5e7eb;
}

/* Tema escuro automático — ativado quando o SO do usuário usa tema escuro */
@media (prefers-color-scheme: dark) {
  :root {
    --color-bg:      #0f172a;
    --color-surface: #1e293b;
    --color-text:    #f1f5f9;
    --color-border:  #334155;
  }
}

/* Tema escuro manual — ativado pelo botão da página via JavaScript */
/* Esta classe sobrescreve a media query quando necessário */
.dark {
  --color-bg:      #0f172a;
  --color-surface: #1e293b;
  --color-text:    #f1f5f9;
  --color-border:  #334155;
}
```

```js
// main.js — o único JS necessário para o toggle manual
document.getElementById('toggle-tema').addEventListener('click', () => {
  document.body.classList.toggle('dark');
});
```

Nenhum arquivo de componente precisa mudar. O card, o botão, a nav — todos se adaptam porque usam as variáveis.

---

### Exercícios — Custom Properties

**Exercício 1.1 — Identificar o que deveria ser variável** *(fácil)*

Leia o CSS abaixo e liste todos os valores que deveriam virar variáveis. Para cada um, escreva o nome de variável que você usaria.

```css
.card {
  background: #ffffff;
  border: 1px solid #e5e7eb;
  border-radius: 8px;
  padding: 24px;
  font-family: 'Inter', sans-serif;
}

.card:hover {
  border-color: #3b82f6;
}

.card__titulo {
  font-size: 20px;
  color: #1e1e1e;
  margin-bottom: 8px;
}

.card__texto {
  font-size: 14px;
  color: #6b7280;
}
```

**Resposta esperada (formato):**
```
#ffffff       → --color-bg
#e5e7eb       → --color-border
8px           → --radius-md
...
```

---

**Exercício 1.2 — Substituição simples** *(fácil)*

Reescreva o CSS abaixo. Primeiro defina as variáveis no `:root`, depois substitua cada valor fixo por `var(...)`. O resultado visual deve ser idêntico — só a forma de escrever muda.

```css
.hero {
  background: #1a1a2e;
  padding: 64px 16px;
}

.hero h1 {
  color: #ffffff;
  font-size: 48px;
  font-family: 'Georgia', serif;
}

.hero p {
  color: #94a3b8;
  font-size: 18px;
  margin-top: 16px;
}
```

**O que entregar:** o bloco `:root` com as variáveis + o CSS reescrito com `var()`.

---

**Exercício 1.3 — Sistema de espaçamento** *(médio)*

Crie um sistema de espaçamento com 5 tamanhos onde cada um é o dobro do anterior, começando em `0.25rem`:

```
xs = 0.25rem  →  4px
sm = 0.50rem  →  8px
md = 1.00rem  → 16px
lg = 2.00rem  → 32px
xl = 4.00rem  → 64px
```

Depois aplique esses espaçamentos nos `padding` e `margin` de pelo menos 3 componentes: um card, um botão e um campo de formulário. Nenhum dos três pode ter valores numéricos diretos — só `var()`.

---

**Exercício 1.4 — Toggle de tema claro/escuro** *(médio-difícil)*

Crie uma página com pelo menos: header, um card e um botão. O botão alterna entre tema claro e escuro adicionando ou removendo a classe `.dark` no `<body>`.

Requisitos:
- A classe `.dark` só redefine variáveis — não toca em nenhum componente diretamente
- Todos os componentes devem se adaptar automaticamente
- O botão deve mostrar o texto correto conforme o estado atual: "Ativar tema escuro" ou "Ativar tema claro"

```js
// O JS pode ser exatamente este:
const toggle = document.getElementById('toggle-tema');

toggle.addEventListener('click', () => {
  document.body.classList.toggle('dark');
  toggle.textContent = document.body.classList.contains('dark')
    ? 'Ativar tema claro'
    : 'Ativar tema escuro';
});
```

---

**Exercício 1.5 — Paleta gerada com HSL** *(difícil)*

Crie uma paleta de 5 tons de uma cor usando HSL. Mudar apenas `--hue` deve gerar uma paleta completamente diferente.

```css
:root {
  --hue: 220;

  --color-100: hsl(var(--hue), 70%, 90%);  /* mais claro */
  --color-300: hsl(var(--hue), 70%, 70%);
  --color-500: hsl(var(--hue), 70%, 50%);  /* tom base */
  --color-700: hsl(var(--hue), 70%, 30%);
  --color-900: hsl(var(--hue), 70%, 15%);  /* mais escuro */
}
```

Crie uma página que mostre os 5 tons lado a lado. Adicione um `<input type="range" min="0" max="360">` que muda `--hue` em tempo real via JavaScript, atualizando toda a paleta instantaneamente.

```js
document.querySelector('input[type="range"]').addEventListener('input', (e) => {
  document.documentElement.style.setProperty('--hue', e.target.value);
});
```

---

## 2. Arquitetura de pastas

### O problema

```
meu-projeto/
├── index.html
└── style.css   ← 800 linhas. Onde está o CSS do botão? Boa sorte.
```

Mesmo usando variáveis, um único arquivo ainda fica ilegível quando o projeto cresce. Quando tudo está misturado, três problemas aparecem:

- Você não sabe onde procurar uma regra específica
- Alterar uma coisa quebra outra sem querer
- Dois membros da equipe editando o mesmo arquivo causam conflito

### A solução

Separar o CSS por **responsabilidade**. Cada arquivo cuida de uma coisa só, e você sempre sabe onde ir.

```
meu-projeto/
├── index.html
├── css/
│   ├── base.css
│   ├── layout.css
│   ├── components/
│   │   ├── button.css
│   │   ├── card.css
│   │   └── nav.css
│   ├── utils.css
│   └── main.css        ← importa tudo, é o único linkado no HTML
└── js/
    └── main.js
```

O HTML linka **apenas um arquivo**: `main.css`. Esse arquivo importa todos os outros na ordem certa. Assim, se você adicionar um componente novo, só precisa adicionar um `@import` no `main.css` — o HTML não muda.

---

### O arquivo `main.css` — ponto central de importação

```css
/* main.css */
/* Este arquivo não contém CSS próprio — só importa os outros na ordem certa */

/* 1. Fundação: reset, variáveis, tipografia global */
@import url('base.css');

/* 2. Estrutura: container, header, main, footer, grids */
@import url('layout.css');

/* 3. Componentes: um @import por arquivo */
@import url('components/nav.css');
@import url('components/button.css');
@import url('components/card.css');

/* 4. Utilitários: sempre por último */
@import url('utils.css');
```

```html
<!-- index.html — apenas um link -->
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Meu Projeto</title>
  <link rel="stylesheet" href="css/main.css">
</head>
```

A ordem dos `@import` importa pela mesma razão que a ordem dos `<link>` importava antes: o CSS é lido de cima para baixo, e `base.css` precisa ser lido antes de tudo porque define as variáveis que os outros arquivos usam.

---

### O que vai em cada arquivo — detalhado

#### `css/base.css` — A fundação

Este é o primeiro arquivo carregado. Ele define as **regras que valem para o projeto inteiro**. Nenhum componente deve contradizer o que está aqui.

O que colocar:

```css
/* base.css */

/* 1. Reset */
*, *::before, *::after {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

/* 2. Variáveis — o sistema de design do projeto */
:root {
  --color-primary:      #3b82f6;
  --color-primary-dark: #1d4ed8;
  --color-danger:       #ef4444;
  --color-success:      #22c55e;

  --color-text:         #1e1e1e;
  --color-text-muted:   #6b7280;
  --color-bg:           #ffffff;
  --color-surface:      #f9fafb;
  --color-border:       #e5e7eb;

  --spacing-xs:   0.25rem;
  --spacing-sm:   0.5rem;
  --spacing-md:   1rem;
  --spacing-lg:   2rem;
  --spacing-xl:   4rem;

  --font-body:       'Inter', sans-serif;
  --font-size-sm:    0.875rem;
  --font-size-base:  1rem;
  --font-size-lg:    1.25rem;
  --font-size-xl:    1.5rem;
  --font-size-2xl:   2rem;

  --radius-sm:   4px;
  --radius-md:   8px;
  --radius-lg:   16px;
  --radius-full: 9999px;
}

/* Tema escuro */
.dark {
  --color-bg:      #0f172a;
  --color-surface: #1e293b;
  --color-text:    #f1f5f9;
  --color-border:  #334155;
}

/* 3. Estilos globais */
body {
  font-family: var(--font-body);
  font-size: var(--font-size-base);
  color: var(--color-text);
  background-color: var(--color-bg);
  line-height: 1.6;
}

h1, h2, h3, h4 { line-height: 1.2; }
a { color: var(--color-primary); }
img { max-width: 100%; display: block; }
```

**Regra de ouro:** nunca escrever `.card`, `.nav` ou qualquer classe de componente aqui. Se escreveu, está no lugar errado.

---

#### `css/layout.css` — A estrutura da página

Cuida do **esqueleto** da página: onde ficam o cabeçalho, o conteúdo principal, o rodapé e como as seções se organizam. Não define a aparência de componentes individuais — só a estrutura.

O que colocar:

```css
/* layout.css */

/* Container centralizado — reutilizado em toda a página */
.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 var(--spacing-md);
}

/* Cabeçalho */
.site-header {
  background: var(--color-primary);
  padding: var(--spacing-md) 0;
}

/* Conteúdo principal */
.site-main {
  min-height: 60vh;
  padding: var(--spacing-lg) 0;
}

/* Rodapé */
.site-footer {
  background: var(--color-surface);
  border-top: 1px solid var(--color-border);
  padding: var(--spacing-lg) 0;
}

/* Grids reutilizáveis */
.grid-2 {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: var(--spacing-md);
}

.grid-3 {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: var(--spacing-md);
}
```

**Como diferenciar layout de componente:** o layout posiciona e dimensiona regiões da página. O componente define a aparência de um elemento específico. `.site-header` é layout (posiciona o cabeçalho no topo). `.nav` é componente (define como a navegação parece por dentro).

---

#### `css/components/` — Um arquivo por componente

Cada arquivo aqui é responsável por **um único componente reutilizável**. Reutilizável significa: esse elemento aparece em mais de um lugar.

**`components/button.css`**

```css
/* components/button.css */

/* ── Block ────────────────────────── */
.button {
  display: inline-block;
  background: var(--color-primary);
  color: var(--color-bg);
  padding: var(--spacing-sm) var(--spacing-md);
  border: 2px solid transparent;
  border-radius: var(--radius-md);
  font-size: var(--font-size-base);
  font-family: var(--font-body);
  cursor: pointer;
  text-decoration: none;
  transition: opacity 0.2s;
}

.button:hover {
  opacity: 0.85;
}

/* ── Modifiers ────────────────────── */
.button--outline {
  background: transparent;
  border-color: var(--color-primary);
  color: var(--color-primary);
}

.button--danger {
  background: var(--color-danger);
}

.button--sm {
  padding: var(--spacing-xs) var(--spacing-sm);
  font-size: var(--font-size-sm);
}

.button--full {
  display: block;
  width: 100%;
  text-align: center;
}
```

**`components/card.css`**

```css
/* components/card.css */

/* ── Block ────────────────────────── */
.card {
  background: var(--color-surface);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-md);
  overflow: hidden;
}

/* ── Elements ─────────────────────── */
.card__imagem {
  width: 100%;
  aspect-ratio: 16 / 9;
  object-fit: cover;
}

.card__corpo {
  padding: var(--spacing-md);
  display: flex;
  flex-direction: column;
  gap: var(--spacing-xs);
}

.card__titulo {
  font-size: var(--font-size-lg);
  color: var(--color-text);
}

.card__descricao {
  font-size: var(--font-size-sm);
  color: var(--color-text-muted);
  line-height: 1.5;
}

.card__link {
  color: var(--color-primary);
  font-size: var(--font-size-sm);
  text-decoration: none;
  margin-top: var(--spacing-xs);
}

.card__link:hover {
  text-decoration: underline;
}

/* ── Modifiers ────────────────────── */
.card--destaque {
  border-color: var(--color-primary);
  box-shadow: 0 0 0 2px var(--color-primary);
}

.card--horizontal {
  display: flex;
  flex-direction: row;
}

.card--horizontal .card__imagem {
  width: 180px;
  aspect-ratio: auto;
  flex-shrink: 0;
}

.card--desativado {
  opacity: 0.5;
  pointer-events: none;
}
```

**`components/nav.css`**

```css
/* components/nav.css */

/* ── Block ────────────────────────── */
.nav {
  display: flex;
  align-items: center;
  gap: var(--spacing-md);
  list-style: none;
}

/* ── Elements ─────────────────────── */
.nav__link {
  color: var(--color-bg);
  text-decoration: none;
  padding: var(--spacing-xs) var(--spacing-sm);
  border-radius: var(--radius-sm);
  transition: background 0.2s;
}

.nav__link:hover {
  background: rgba(255, 255, 255, 0.15);
}

/* ── Modifiers ────────────────────── */
.nav__link--ativo {
  background: rgba(255, 255, 255, 0.2);
  font-weight: 500;
}
```

**Quando criar um novo arquivo de componente?** Sempre que um elemento aparecer em mais de um lugar do projeto. Se aparece só uma vez e nunca vai se repetir, pode ficar no `layout.css`. Se vai se repetir, merece seu próprio arquivo em `components/`.

---

#### `css/utils.css` — Ajustes pontuais

Classes pequenas de propósito único. Usadas diretamente no HTML para ajustes rápidos sem criar um componente inteiro.

```css
/* utils.css */

/* Espaçamentos */
.mt-auto { margin-top: auto; }
.mb-0    { margin-bottom: 0; }

/* Alinhamento */
.text-center { text-align: center; }
.text-right  { text-align: right; }

/* Visibilidade */
.hidden  { display: none; }

/* Flexbox */
.flex        { display: flex; }
.flex-center { display: flex; align-items: center; justify-content: center; }
.gap-sm      { gap: var(--spacing-sm); }
.gap-md      { gap: var(--spacing-md); }
```

**Regra de ouro:** cada classe utilitária faz **uma única coisa**. Se você está escrevendo mais de duas propriedades em uma classe utilitária, ela deveria ser um componente.

---

### Passo a passo — montando a estrutura do zero

Siga estes passos sempre que começar um projeto novo:

**Passo 1** — Crie as pastas:

```
meu-projeto/
├── css/
│   └── components/
└── js/
```

**Passo 2** — Crie cada arquivo CSS com um comentário de cabeçalho dizendo o que ele faz:

```css
/* base.css — Reset, variáveis e estilos globais */
```

```css
/* layout.css — Estrutura da página: container, header, main, footer, grids */
```

```css
/* components/button.css — Componente: botão e suas variações */
```

```css
/* components/card.css — Componente: card de conteúdo e suas variações */
```

```css
/* components/nav.css — Componente: navegação principal */
```

```css
/* utils.css — Classes utilitárias de uso geral */
```

**Passo 3** — Crie o `main.css` com os `@import` na ordem certa:

```css
/* main.css — Arquivo central: importa todos os outros */

@import url('base.css');
@import url('layout.css');
@import url('components/nav.css');
@import url('components/button.css');
@import url('components/card.css');
@import url('utils.css');
```

**Passo 4** — No `index.html`, linke **apenas o `main.css`**:

```html
<link rel="stylesheet" href="css/main.css">
```

**Passo 5** — Comece a escrever CSS pelo `base.css`: primeiro as variáveis no `:root`, depois o reset, depois os estilos globais do `body`.

**Passo 6** — Só então escreva o `layout.css`, e depois os componentes um a um.

**Passo 7** — Quando adicionar um componente novo, crie o arquivo em `components/` e adicione o `@import` correspondente no `main.css`.

---

### Exercícios — Arquitetura de pastas

**Exercício 2.1 — Classificar antes de organizar** *(fácil)*

Leia cada linha de CSS abaixo e escreva ao lado em qual arquivo ela deve ir. Não precisa mover nada ainda — só classifique.

```css
* { box-sizing: border-box; margin: 0; padding: 0; }          /* _______ */
body { font-family: sans-serif; color: #333; }                 /* _______ */
:root { --color-primary: #3b82f6; }                            /* _______ */
.container { max-width: 1200px; margin: 0 auto; }              /* _______ */
.site-header { background: var(--color-primary); }             /* _______ */
.card { background: white; border-radius: 8px; }               /* _______ */
.card__titulo { font-size: 1.25rem; }                          /* _______ */
.text-center { text-align: center; }                           /* _______ */
.site-footer { padding: 2rem 0; }                              /* _______ */
.button { background: var(--color-primary); color: white; }    /* _______ */
```

---

**Exercício 2.2 — Montar e linkar** *(fácil)*

Crie os arquivos do exercício anterior nos lugares certos, mova cada bloco de CSS para o arquivo correto e escreva o `main.css` com os `@import` na ordem certa. Crie um `index.html` que linka só o `main.css` e exibe um card no centro da tela.

---

**Exercício 2.3 — Estrutura para um portfólio** *(médio)*

Monte a estrutura de pastas completa para um portfólio com seção hero, lista de projetos e formulário de contato. Componentes mínimos: `nav.css`, `card.css`, `button.css`, `form.css`. Cada arquivo deve ter o cabeçalho de comentário e pelo menos 3 regras reais de CSS. O `main.css` deve importar tudo. O projeto deve abrir no navegador sem erros.

---

**Exercício 2.4 — Auditoria de projeto próprio** *(difícil)*

Pegue qualquer projeto seu anterior com um único `style.css`. Leia o arquivo inteiro e classifique cada regra em base, layout, componente ou utilitário. Depois refatore: mova tudo para a estrutura de pastas, crie o `main.css` com os `@import` e atualize o HTML para linkar só o `main.css`. Ao terminar, verifique se consegue descrever em uma frase o que cada arquivo faz.

---

## 3. BEM — nomenclatura de classes

### O problema

```css
/* Em components/hero.css */
.titulo { font-size: 3rem; color: white; }

/* Em components/card.css */
.titulo { font-size: 1.25rem; color: #333; } /* sobrescreve o anterior */
```

Mesmo com a estrutura de pastas e variáveis, nomes genéricos como `.titulo`, `.texto`, `.imagem` colidem entre componentes. O CSS de um arquivo acaba sobrescrevendo o outro.

### A solução

BEM (**B**lock **E**lement **M**odifier) é uma convenção de nomenclatura que torna cada classe **única e autodescritiva** — você sabe de qual componente ela vem só de ler o nome.

```
.bloco {}                ← o componente inteiro
.bloco__elemento {}      ← parte interna (dois underscores)
.bloco--modificador {}   ← variação do componente (dois hifens)
```

---

### Os três conceitos com exemplos

#### Block — o componente

Qualquer elemento que faz sentido existir sozinho na página.

```
card    nav    button    modal    hero    form    badge    avatar
```

```css
.card { ... }
.button { ... }
.nav { ... }
```

#### Element — parte interna do block

Só existe dentro do block. Usa dois underscores: `.block__element`.

```html
<article class="card">
  <img class="card__imagem">
  <div class="card__corpo">
    <h2 class="card__titulo">
    <p class="card__descricao">
  </div>
</article>
```

**Regra importante:** nunca aninhe mais de um nível. Não existe `.card__corpo__titulo`. Se o elemento filho precisa de estilo, ele é um elemento direto do block:

```css
/* ✓ correto */
.card__titulo { font-size: var(--font-size-lg); }

/* ✗ errado — BEM não usa mais de um nível de elemento */
.card__corpo__titulo { font-size: var(--font-size-lg); }
```

#### Modifier — variação do block ou elemento

Muda aparência ou comportamento. Usa dois hifens: `.block--modifier`.

```html
<!-- O modifier é sempre adicionado junto com a classe base -->
<button class="button">Padrão</button>
<button class="button button--outline">Outline</button>
<button class="button button--danger">Perigo</button>
<button class="button button--sm">Pequeno</button>

<article class="card card--destaque">...</article>
<article class="card card--horizontal">...</article>
```

---

### Exemplo completo — card com BEM

```html
<!-- index.html -->
<article class="card">
  <img class="card__imagem" src="projeto.jpg" alt="Projeto Alpha">
  <div class="card__corpo">
    <span class="card__tag">Web</span>
    <h2 class="card__titulo">Projeto Alpha</h2>
    <p class="card__descricao">Aplicação de gestão de tarefas com foco em simplicidade.</p>
    <a class="card__link" href="#">Ver projeto</a>
  </div>
</article>

<article class="card card--destaque">...</article>
<article class="card card--desativado">...</article>
```

```css
/* components/card.css */

/* ── Block ───────────────────────────────── */
.card {
  background: var(--color-surface);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-md);
  overflow: hidden;
}

/* ── Elements ────────────────────────────── */
.card__imagem {
  width: 100%;
  aspect-ratio: 16 / 9;
  object-fit: cover;
}

.card__corpo {
  padding: var(--spacing-md);
  display: flex;
  flex-direction: column;
  gap: var(--spacing-xs);
}

.card__tag {
  font-size: var(--font-size-sm);
  color: var(--color-primary);
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

.card__titulo {
  font-size: var(--font-size-lg);
  color: var(--color-text);
}

.card__descricao {
  font-size: var(--font-size-sm);
  color: var(--color-text-muted);
  line-height: 1.5;
}

.card__link {
  color: var(--color-primary);
  font-size: var(--font-size-sm);
  text-decoration: none;
  margin-top: var(--spacing-xs);
}

.card__link:hover {
  text-decoration: underline;
}

/* ── Modifiers ───────────────────────────── */
.card--destaque {
  border-color: var(--color-primary);
  box-shadow: 0 0 0 2px var(--color-primary);
}

.card--horizontal {
  display: flex;
  flex-direction: row;
}

.card--horizontal .card__imagem {
  width: 180px;
  aspect-ratio: auto;
  flex-shrink: 0;
}

.card--desativado {
  opacity: 0.5;
  pointer-events: none;
}
```

---

### Passo a passo — escrevendo BEM

Siga estes passos toda vez que for estilizar um componente novo:

**Passo 1** — Olhe o HTML e identifique o **block**: qual é o elemento pai que representa o componente inteiro?

```html
<article class="card">  ← este é o block
```

**Passo 2** — Identifique os **elements**: quais são as partes internas que precisam de estilo?

```
<img>  →  card__imagem
<div>  →  card__corpo
<h2>   →  card__titulo
<p>    →  card__descricao
<a>    →  card__link
```

**Passo 3** — Adicione as classes no HTML:

```html
<article class="card">
  <img class="card__imagem">
  <div class="card__corpo">
    <h2 class="card__titulo">...</h2>
    <p class="card__descricao">...</p>
    <a class="card__link">...</a>
  </div>
</article>
```

**Passo 4** — Crie o arquivo CSS em `components/card.css` com as três seções comentadas:

```css
/* ── Block ────── */
.card { }

/* ── Elements ─── */
.card__imagem { }
.card__corpo { }
.card__titulo { }
.card__descricao { }
.card__link { }

/* ── Modifiers ── */
.card--destaque { }
```

**Passo 5** — Escreva as propriedades usando `var()` para tudo. Nunca valores fixos.

**Passo 6** — Adicione o `@import url('components/card.css');` no `main.css`.

**Passo 7** — Se precisar de uma variação visual (card horizontal, card desativado), adicione um **modifier** — nunca use seletor como `.card:nth-child(2)` ou escreva um novo block do zero.

---

### Exercícios — BEM

**Exercício 3.1 — Ler e nomear** *(fácil)*

Para cada HTML abaixo, escreva os nomes de classe BEM corretos para cada elemento. Não escreva CSS — só os nomes.

```html
<!-- Componente 1: barra de navegação com link ativo -->
<nav>
  <ul>
    <li><a href="#">Início</a></li>
    <li><a href="#" class="ativo">Projetos</a></li>
    <li><a href="#">Contato</a></li>
  </ul>
</nav>

<!-- Componente 2: cartão de usuário com status -->
<div>
  <img src="foto.jpg">
  <div>
    <strong>Ana Silva</strong>
    <span>Desenvolvedora</span>
  </div>
  <span class="status online">● Online</span>
</div>

<!-- Componente 3: campo de formulário com erro -->
<div>
  <label for="email">E-mail</label>
  <input id="email" type="email" class="invalido">
  <span>Formato inválido</span>
</div>
```

**Resposta esperada (formato):**
```
Componente 1:
  nav       → block: nav
  ul        → (sem classe necessária)
  li        → nav__item
  a         → nav__link
  a.ativo   → nav__link nav__link--ativo
```

---

**Exercício 3.2 — Adicionar as classes no HTML** *(fácil)*

Pegue o resultado do exercício anterior e adicione as classes BEM corretas no HTML dos três componentes. Não escreva CSS ainda — só o HTML com as classes no lugar.

---

**Exercício 3.3 — Criar o componente completo** *(médio)*

Crie um componente de perfil de usuário com HTML + CSS. Use BEM e custom properties. O componente deve ter: foto circular, nome, cargo e badge de status. Deve ter dois modifiers: `--online` (badge verde) e `--offline` (badge cinza).

```
┌──────────────────────────────┐
│  [foto]  Ana Silva     🟢   │
│          Desenvolvedora      │
└──────────────────────────────┘
```

Estrutura esperada do CSS:
```css
/* ── Block ────── */
.perfil { }

/* ── Elements ─── */
.perfil__foto { }
.perfil__info { }
.perfil__nome { }
.perfil__cargo { }
.perfil__status { }

/* ── Modifiers ── */
.perfil--online .perfil__status { }
.perfil--offline .perfil__status { }
```

---

**Exercício 3.4 — Refatorar código sem BEM** *(médio-difícil)*

Reescreva o HTML e CSS abaixo usando BEM + custom properties. O resultado visual deve ser idêntico. Coloque o CSS no arquivo `components/produto.css` e adicione o import no `main.css`.

```html
<div class="produto">
  <img src="tenis.jpg">
  <h3>Tênis Runner X</h3>
  <p class="preco">R$ 299,90</p>
  <p class="preco desconto">R$ 199,90</p>
  <button>Comprar</button>
  <button class="esgotado" disabled>Esgotado</button>
</div>
```

```css
.produto { border: 1px solid #ddd; padding: 16px; width: 260px; }
.produto img { width: 100%; border-radius: 4px; }
.produto h3 { font-size: 18px; margin: 8px 0 4px; }
.preco { color: #333; font-size: 16px; }
.preco.desconto { color: #16a34a; font-weight: bold; font-size: 20px; }
.produto button { background: #3b82f6; color: white; border: none; padding: 8px 16px; border-radius: 4px; cursor: pointer; margin-top: 8px; width: 100%; }
.produto button.esgotado { background: #9ca3af; cursor: not-allowed; }
```

Critério: abrir `components/produto.css` e entender o que cada parte faz sem ver o HTML.

---

**Exercício 3.5 — Projeto integrado completo** *(difícil)*

Construa uma página de portfólio usando tudo o que aprendeu neste módulo.

**Estrutura obrigatória:**
```
portfolio/
├── index.html
├── css/
│   ├── base.css
│   ├── layout.css
│   ├── components/
│   │   ├── nav.css
│   │   ├── card.css
│   │   └── button.css
│   ├── utils.css
│   └── main.css
└── js/
    └── main.js
```

**Conteúdo mínimo:**
- Header com nav e botão de toggle de tema
- Seção hero com título e botão
- Grade com 4 cards de projeto (um com modifier `--destaque`)
- Footer

**Requisitos técnicos:**
- `index.html` linka apenas `css/main.css`
- `main.css` importa tudo na ordem certa com `@import`
- `base.css` tem o sistema completo de variáveis e o `.dark` para tema escuro
- Nenhum componente usa valores fixos de cor, tamanho ou fonte — só `var()`
- BEM em todas as classes de componente, com as seções Block / Elements / Modifiers comentadas

**Critérios de avaliação:**
1. Mudar `--color-primary` no `:root` altera a cor principal em toda a página
2. Adicionar a classe `.dark` no `<body>` muda o tema inteiro sem tocar em nenhum componente
3. Abrir qualquer arquivo em `components/` e entender o que ele faz sem ver o HTML

---

## Resumo

| Técnica | Resolve | Onde fica | Regra de ouro |
|---|---|---|---|
| Custom Properties | "Preciso mudar esse valor em 30 lugares" | `:root` no `base.css` | Defina uma vez, use em todo lugar |
| Arquitetura de pastas | "Onde está esse CSS?" | Estrutura de arquivos | Um arquivo, uma responsabilidade |
| BEM | "Essa classe vai colidir com outra?" | Nomes das classes | Block, dois underscores, dois hifens |

As três técnicas se completam:

- **Variáveis** organizam os valores — você muda em um lugar só
- **Pastas** organizam os arquivos — você sabe onde procurar
- **BEM** organiza os nomes — você evita colisões

O `main.css` une tudo: é o ponto central que importa cada arquivo na ordem certa, e o único que o HTML precisa conhecer.
