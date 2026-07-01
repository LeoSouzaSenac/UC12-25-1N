# Construindo um site com React + Tailwind

Neste tutorial vamos construir a landing page de uma cafeteria de café especial fictícia, a **Grão**. 
O objetivo não é aprender Tailwind nem React do zero — é pegar o que vocês já sabem (JSX, props, componentes) e o básico de Tailwind, e aplicar isso a um projeto real, com atenção em decisões de design, não só de código.

Não vamos usar `useState`, `useEffect` nem nenhum hook. Todo o site é estático: os dados mudam via **props**, não via estado. Isso é proposital — o objetivo aqui é fixar componentização e Tailwind antes de introduzir estado.

---

## O que vamos construir

Uma página única com:

1. **Header** — logo e menu de navegação
2. **Hero** — a primeira coisa que a pessoa vê, com um selo de identidade visual do café
3. **Seção "Sobre"** — texto + imagem
4. **Seção "Cardápio"** — grade de cards de produto
5. **Seção "Depoimentos"** — o que os clientes dizem
6. **Footer** — contato e links

---

## Antes de codar: as decisões de design

Um site "bonito" não é sorte — é um conjunto de decisões deliberadas sobre cor, tipografia e layout, tomadas *antes* de abrir o editor. Vamos explicar cada uma, porque em projetos de vocês essas mesmas perguntas vão aparecer.

### Paleta de cores

Café remete a tons terrosos e quentes. Em vez do azul genérico que qualquer site usa, a paleta é:

| Nome | Hex | Uso |
|---|---|---|
| `creme` | `#F5EDE1` | fundo principal da página |
| `creme-claro` | `#FBF7F0` | fundo de cards, contraste sutil com o `creme` |
| `cafe-900` | `#2B1B12` | texto principal, quase preto mas quente |
| `cafe-600` | `#6B5645` | texto secundário |
| `terracota` | `#A63D2F` | cor de destaque — botões, links, detalhes |
| `musgo` | `#7A8A5E` | segunda cor de destaque — usada com moderação |

Repare que não é "café marrom óbvio + branco". O terracota (tom de barro queimado) e o musgo (verde folha de cafeeiro) dão personalidade sem fugir do tema.

### Tipografia

Duas fontes, cada uma com um papel:

- **Fraunces** (serifada, com bastante personalidade) para títulos — remete a rótulos artesanais, letreiros de cafeteria
- **Work Sans** (sem serifa, neutra e legível) para texto corrido

Fontes serifadas em títulos + sem serifa no corpo é uma combinação clássica porque cria contraste: o título tem caráter, o texto é fácil de ler.

### O elemento de assinatura

Toda seção hero genérica tem: fundo escuro, texto branco centralizado, botão degradê. Para fugir disso, o hero da Grão vai ter um **círculo decorativo atrás do título**, lembrando a marca que uma xícara de café deixa na mesa — um detalhe que só faz sentido *para esse assunto específico*, feito em SVG puro (sem precisar de nenhuma imagem).

---

## Passo 1 — Setup do projeto

```bash
npm create vite@latest grao -- --template react
cd grao
npm install
npm install tailwindcss @tailwindcss/vite
```

No `vite.config.js`:

```js
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import tailwindcss from '@tailwindcss/vite'

export default defineConfig({
  plugins: [react(), tailwindcss()],
})
```

---

## Passo 2 — Configurando o tema (cores e fontes customizadas)

Isso é a ponte entre o que vocês aprenderam de **variáveis CSS** e o Tailwind: no Tailwind v4, você define suas próprias cores e fontes dentro de um bloco `@theme`, direto no CSS. Por baixo dos panos, isso vira variáveis CSS — e gera classes novas automaticamente (`bg-terracota`, `text-cafe-900`, `font-display`).

`src/index.css`:

```css
/* index.css */
/* Importa o Tailwind inteiro */
@import "tailwindcss";

/* Define o "sistema de design" do projeto — nossas próprias cores e fontes,
   que passam a existir como classes do Tailwind (ex: bg-terracota, font-display) */
@theme {
  /* Cores */
  --color-creme: #f5ede1;
  --color-creme-claro: #fbf7f0;
  --color-cafe-900: #2b1b12;
  --color-cafe-600: #6b5645;
  --color-terracota: #a63d2f;
  --color-terracota-escuro: #832f24;
  --color-musgo: #7a8a5e;

  /* Fontes */
  --font-display: "Fraunces", serif;
  --font-body: "Work Sans", sans-serif;
}

/* Estilos globais mínimos */
body {
  font-family: var(--font-body);
  background-color: var(--color-creme);
  color: var(--color-cafe-900);
}
```

E no `index.html`, importamos as fontes do Google Fonts:

```html
<!-- index.html -->
<head>
  <meta charset="UTF-8" />
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link
    href="https://fonts.googleapis.com/css2?family=Fraunces:opsz,wght@9..144,400;9..144,600;9..144,700&family=Work+Sans:wght@400;500;600&display=swap"
    rel="stylesheet"
  />
  <title>Grão — Café Especial</title>
</head>
```

---

## Passo 3 — Estrutura de pastas

Igual ao que vocês fizeram no módulo de CSS Modular: cada responsabilidade no seu lugar. A diferença é que agora "componente" vira um arquivo `.jsx`, não um bloco de CSS.

```
grao/
├── src/
│   ├── components/
│   │   ├── Header.jsx
│   │   ├── Botao.jsx
│   │   ├── Hero.jsx
│   │   ├── TituloSecao.jsx
│   │   ├── CardProduto.jsx
│   │   ├── Depoimento.jsx
│   │   └── Footer.jsx
│   ├── data/
│   │   └── conteudo.js
│   ├── assets/
│   │   └── (imagens aqui)
│   ├── App.jsx
│   ├── index.css
│   └── main.jsx
├── index.html
└── vite.config.js
```

Separar os **dados** (`data/conteudo.js`) dos **componentes** é o mesmo princípio de responsabilidade única que vocês já usam no CSS: o componente só sabe *como exibir*, o arquivo de dados só sabe *o que exibir*. Isso facilita trocar textos e imagens sem mexer em nenhum JSX.

---

## Passo 4 — Os dados do site

`src/data/conteudo.js`:

```js
// conteudo.js
// Guarda todo o conteúdo textual do site separado dos componentes.
// Assim, para editar um texto, não é preciso mexer em nenhum arquivo .jsx.

export const linksNav = [
  { texto: "Início", href: "#inicio" },
  { texto: "Sobre", href: "#sobre" },
  { texto: "Cardápio", href: "#cardapio" },
  { texto: "Contato", href: "#contato" },
];

export const produtos = [
  {
    id: 1,
    nome: "Café Coado",
    descricao: "Grãos 100% arábica, torra média, notas de caramelo e frutas vermelhas.",
    preco: "R$ 12,00",
  },
  {
    id: 2,
    nome: "Cappuccino",
    descricao: "Espresso encorpado com leite vaporizado e uma fina camada de espuma.",
    preco: "R$ 15,00",
  },
  {
    id: 3,
    nome: "Prensa Francesa",
    descricao: "Extração lenta que realça o corpo do café, servida em porção individual.",
    preco: "R$ 14,00",
  },
];

export const depoimentos = [
  {
    id: 1,
    texto: "O melhor café coado que já tomei na cidade. Virou parada obrigatória de manhã.",
    autor: "Marina Costa",
    cargo: "Cliente desde 2023",
  },
  {
    id: 2,
    texto: "Ambiente aconchegante e atendimento atencioso. Cada xícara é preparada com cuidado.",
    autor: "Rafael Nunes",
    cargo: "Cliente desde 2024",
  },
];
```

---

## Passo 5 — Componente `Botao`

Reaproveitando o componente de botão de vocês, agora com uma prop `variante` para controlar a aparência sem duplicar código.

`src/components/Botao.jsx`:

```jsx
// Botao.jsx
// Botão reutilizável. A prop "variante" controla a aparência
// (equivalente a um modifier do BEM, só que resolvido em JS).

function Botao({ texto, variante = "primario", href }) {
  // Guarda as classes de cada variante num objeto — evita um monte de if/else
  const estilosPorVariante = {
    primario: "bg-terracota text-creme-claro hover:bg-terracota-escuro",
    secundario: "bg-transparent text-cafe-900 border-2 border-cafe-900 hover:bg-cafe-900 hover:text-creme-claro",
  };

  return (
    <a
      href={href}
      className={`inline-block px-6 py-3 rounded-full font-body font-medium
                  transition duration-300 ${estilosPorVariante[variante]}`}
    >
      {texto}
    </a>
  );
}

export default Botao;
```

**Por que um objeto em vez de `if/else` ou ternário?** Porque se amanhã vocês precisarem de uma terceira variante, é só adicionar uma linha no objeto — não precisa mexer na lógica do componente. Vale mostrar essa técnica aos alunos como alternativa mais limpa a encadear vários ternários.

---

## Passo 6 — Componente `Header`

`src/components/Header.jsx`:

```jsx
// Header.jsx
// Cabeçalho fixo no topo com logo e navegação.
// Recebe a lista de links como prop — o componente não sabe quais são os links,
// só sabe como exibi-los.

function Header({ links }) {
  return (
    <header className="fixed top-0 left-0 w-full bg-creme/90 backdrop-blur-sm z-50 border-b border-cafe-900/10">
      <div className="max-w-6xl mx-auto px-6 py-4 flex items-center justify-between">
        {/* Logo em texto, usando a fonte de display */}
        <span className="font-display text-2xl font-semibold text-cafe-900">
          Grão
        </span>

        {/* Navegação — escondida no mobile, visível a partir de md */}
        <nav className="hidden md:flex gap-8">
          {links.map((link) => (
            <a
              key={link.texto}
              href={link.href}
              className="font-body text-sm text-cafe-600 hover:text-terracota transition"
            >
              {link.texto}
            </a>
          ))}
        </nav>
      </div>
    </header>
  );
}

export default Header;
```

Ponto de atenção pros alunos: `bg-creme/90` é a sintaxe do Tailwind para opacidade — `/90` significa 90% de opacidade daquela cor. `backdrop-blur-sm` desfoca o que está atrás do header, um efeito comum em headers fixos modernos.

---

## Passo 7 — Componente `Hero`

Aqui entra o elemento de assinatura: o círculo decorativo em SVG atrás do título, remetendo à marca de xícara de café.

`src/components/Hero.jsx`:

```jsx
// Hero.jsx
// Primeira seção da página. Recebe título, subtítulo e imagem via props,
// para que o mesmo componente pudesse, em outro projeto, ser reaproveitado
// com outro conteúdo.

function Hero({ titulo, subtitulo, imagem }) {
  return (
    <section id="inicio" className="pt-32 pb-20 px-6">
      <div className="max-w-6xl mx-auto flex flex-col md:flex-row items-center gap-12">
        {/* Coluna de texto */}
        <div className="flex-1 relative">
          {/* Círculo decorativo atrás do título — o "selo" da marca,
              lembrando a marca que uma xícara deixa na mesa */}
          <svg
            className="absolute -top-10 -left-10 w-40 h-40 text-terracota/20 -z-10"
            viewBox="0 0 100 100"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <circle cx="50" cy="50" r="48" stroke="currentColor" strokeWidth="4" />
          </svg>

          <h1 className="font-display text-5xl md:text-6xl font-semibold leading-tight text-cafe-900">
            {titulo}
          </h1>

          <p className="mt-6 text-lg text-cafe-600 max-w-md">
            {subtitulo}
          </p>

          <div className="mt-8 flex gap-4">
            <Botao texto="Ver cardápio" variante="primario" href="#cardapio" />
            <Botao texto="Sobre nós" variante="secundario" href="#sobre" />
          </div>
        </div>

        {/* Coluna de imagem */}
        <div className="flex-1">
          <img
            src={imagem}
            alt="Xícara de café especial servida na cafeteria Grão"
            className="rounded-3xl w-full h-[420px] object-cover shadow-xl"
          />
        </div>
      </div>
    </section>
  );
}

export default Hero;
```

> Faltou um import: `Botao` precisa ser importado no topo do arquivo (`import Botao from "./Botao";`). Deixei de fora do bloco acima de propósito, como lembrete: sempre que usarem um componente dentro de outro, conferir se o import está lá — é o erro mais comum de aluno iniciante em React.

---

## Passo 8 — Componente `TituloSecao`

Um componente pequeno e reutilizável para o cabeçalho de cada seção (Sobre, Cardápio, Depoimentos) — evita repetir a mesma estrutura de título três vezes.

`src/components/TituloSecao.jsx`:

```jsx
// TituloSecao.jsx
// Cabeçalho reutilizável para o início de cada seção da página.
// Centraliza a estrutura (eyebrow + título + descrição) num único lugar.

function TituloSecao({ eyebrow, titulo, descricao }) {
  return (
    <div className="max-w-xl mx-auto text-center mb-14">
      {/* Eyebrow: texto pequeno acima do título, dá contexto rápido */}
      <span className="font-body text-sm uppercase tracking-widest text-musgo font-medium">
        {eyebrow}
      </span>

      <h2 className="font-display text-4xl font-semibold text-cafe-900 mt-3">
        {titulo}
      </h2>

      {descricao && (
        <p className="mt-4 text-cafe-600">{descricao}</p>
      )}
    </div>
  );
}

export default TituloSecao;
```

Note o `{descricao && (...)}`: renderização condicional — se a seção não receber `descricao`, o parágrafo simplesmente não é exibido, sem quebrar nada. Vocês já viram esse padrão nas aulas anteriores.

---

## Passo 9 — Componente `CardProduto`

`src/components/CardProduto.jsx`:

```jsx
// CardProduto.jsx
// Card de um item do cardápio. Puramente apresentacional —
// recebe tudo que precisa via props e não conhece a lista inteira de produtos.

function CardProduto({ nome, descricao, preco }) {
  return (
    <div className="bg-creme-claro rounded-2xl p-6 border border-cafe-900/10 hover:border-terracota/40 transition">
      {/* Placeholder de imagem — substitua pela foto real do produto */}
      <div className="w-full h-40 rounded-xl bg-cafe-900/5 border-2 border-dashed border-cafe-900/20 flex items-center justify-center mb-5">
        <span className="text-xs text-cafe-600 uppercase tracking-wide">
          Imagem do produto
        </span>
      </div>

      <h3 className="font-display text-xl font-semibold text-cafe-900">
        {nome}
      </h3>

      <p className="mt-2 text-sm text-cafe-600">{descricao}</p>

      <span className="mt-4 inline-block font-body font-semibold text-terracota">
        {preco}
      </span>
    </div>
  );
}

export default CardProduto;
```

Esse card usa um **placeholder visual** em vez de imagem — uma caixa com borda tracejada avisando onde a imagem real deve entrar. É uma prática comum quando o conteúdo final ainda não está pronto.

---

## Passo 10 — Componente `Depoimento`

`src/components/Depoimento.jsx`:

```jsx
// Depoimento.jsx
// Card de depoimento de cliente.

function Depoimento({ texto, autor, cargo }) {
  return (
    <div className="bg-creme-claro rounded-2xl p-8 border border-cafe-900/10">
      <p className="font-display text-lg italic text-cafe-900 leading-relaxed">
        "{texto}"
      </p>

      <div className="mt-6 flex items-center gap-3">
        {/* Placeholder circular de avatar */}
        <div className="w-10 h-10 rounded-full bg-cafe-900/10 border-2 border-dashed border-cafe-900/20" />

        <div>
          <p className="text-sm font-semibold text-cafe-900">{autor}</p>
          <p className="text-xs text-cafe-600">{cargo}</p>
        </div>
      </div>
    </div>
  );
}

export default Depoimento;
```

---

## Passo 11 — Componente `Footer`

`src/components/Footer.jsx`:

```jsx
// Footer.jsx
// Rodapé com informações de contato e ano dinâmico via prop.

function Footer({ ano, links }) {
  return (
    <footer id="contato" className="bg-cafe-900 text-creme-claro py-12 px-6">
      <div className="max-w-6xl mx-auto flex flex-col md:flex-row justify-between items-center gap-6">
        <span className="font-display text-xl">Grão</span>

        <nav className="flex gap-6">
          {links.map((link) => (
            <a
              key={link.texto}
              href={link.href}
              className="text-sm text-creme-claro/70 hover:text-creme-claro transition"
            >
              {link.texto}
            </a>
          ))}
        </nav>

        <p className="text-xs text-creme-claro/50">
          © {ano} Grão Café Especial. Todos os direitos reservados.
        </p>
      </div>
    </footer>
  );
}

export default Footer;
```

Aqui `ano` vem como prop em vez de fixo no texto — no `App.jsx` vamos usar `new Date().getFullYear()` para calcular o ano atual automaticamente, sem precisar de estado (é só uma expressão JS comum, calculada uma vez quando o componente renderiza).

---

## Passo 12 — Juntando tudo no `App.jsx`

```jsx
// App.jsx
// Componente raiz: importa os componentes e os dados, e monta a página.
// Repare que o App não tem nenhuma lógica de exibição — ele só organiza
// quem recebe o quê.

import Header from "./components/Header";
import Hero from "./components/Hero";
import TituloSecao from "./components/TituloSecao";
import CardProduto from "./components/CardProduto";
import Depoimento from "./components/Depoimento";
import Footer from "./components/Footer";

import { linksNav, produtos, depoimentos } from "./data/conteudo";

// Placeholder de imagem do hero — troque pelo arquivo real em src/assets
import heroImagem from "./assets/hero-placeholder.jpg";

function App() {
  return (
    <div className="font-body">
      <Header links={linksNav} />

      <Hero
        titulo="Café especial, torrado com cuidado"
        subtitulo="Grãos selecionados diretamente de produtores parceiros, torrados em pequenos lotes para preservar cada nota de sabor."
        imagem={heroImagem}
      />

      {/* Seção Sobre */}
      <section id="sobre" className="py-24 px-6 bg-creme-claro">
        <div className="max-w-6xl mx-auto flex flex-col md:flex-row items-center gap-12">
          <div className="flex-1 w-full h-72 rounded-2xl bg-cafe-900/5 border-2 border-dashed border-cafe-900/20 flex items-center justify-center">
            <span className="text-xs text-cafe-600 uppercase tracking-wide">
              Imagem da cafeteria
            </span>
          </div>

          <div className="flex-1">
            <span className="font-body text-sm uppercase tracking-widest text-musgo font-medium">
              Nossa história
            </span>
            <h2 className="font-display text-4xl font-semibold text-cafe-900 mt-3">
              Do produtor à xícara
            </h2>
            <p className="mt-4 text-cafe-600 leading-relaxed">
              A Grão nasceu da vontade de aproximar quem produz café de quem
              o aprecia. Trabalhamos direto com pequenos produtores,
              acompanhando cada etapa da torra até o preparo final na loja.
            </p>
          </div>
        </div>
      </section>

      {/* Seção Cardápio */}
      <section id="cardapio" className="py-24 px-6">
        <div className="max-w-6xl mx-auto">
          <TituloSecao
            eyebrow="Cardápio"
            titulo="O que preparamos"
            descricao="Uma seleção enxuta, pensada para valorizar cada grão."
          />

          <div className="grid grid-cols-1 md:grid-cols-3 gap-6">
            {produtos.map((produto) => (
              <CardProduto
                key={produto.id}
                nome={produto.nome}
                descricao={produto.descricao}
                preco={produto.preco}
              />
            ))}
          </div>
        </div>
      </section>

      {/* Seção Depoimentos */}
      <section className="py-24 px-6 bg-creme-claro">
        <div className="max-w-6xl mx-auto">
          <TituloSecao eyebrow="Depoimentos" titulo="Quem já provou" />

          <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
            {depoimentos.map((depoimento) => (
              <Depoimento
                key={depoimento.id}
                texto={depoimento.texto}
                autor={depoimento.autor}
                cargo={depoimento.cargo}
              />
            ))}
          </div>
        </div>
      </section>

      <Footer ano={new Date().getFullYear()} links={linksNav} />
    </div>
  );
}

export default App;
```

---

## Sobre as imagens

Todo lugar que precisa de imagem real está marcado de duas formas:

1. **Placeholders visuais** (caixas com borda tracejada e texto "Imagem do produto" etc.) — usados nos cards, onde ainda não existe nenhuma imagem.
2. **Import de arquivo** (`heroImagem`, no Hero) — para o Hero, que já tem um `<img>` estruturado, é só trocar o arquivo dentro de `src/assets/` mantendo o mesmo nome, ou atualizar o caminho do import.

Para trocar um placeholder visual por uma imagem real, o padrão é sempre o mesmo: substituir a `<div>` de placeholder por um `<img src={...} alt="..." className="..." />`, copiando as classes de arredondamento (`rounded-2xl`) e tamanho (`w-full h-40 object-cover`) que já estavam na div.

---

## Resumo do que esse projeto ensina

| Conceito | Onde aparece |
|---|---|
| Componentização | Cada seção/elemento é um componente separado |
| Props | Todo conteúdo entra via props, nada fixo dentro dos componentes |
| Renderização condicional | `{descricao && (...)}` no `TituloSecao` |
| Listas com `.map()` + `key` | Cards de produto, depoimentos, links de navegação |
| Separação de dados e apresentação | `data/conteudo.js` fica fora dos componentes |
| Tailwind: tema customizado | `@theme` com cores e fontes próprias do projeto |
| Tailwind: responsividade | `md:flex-row`, `hidden md:flex` |
| Tailwind: estados | `hover:bg-terracota-escuro`, `hover:border-terracota/40` |
| Design intencional | Paleta, tipografia e um elemento de assinatura pensados para o assunto, não genéricos |
