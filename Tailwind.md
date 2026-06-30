# Tailwind CSS — o básico

## O que é

Tailwind é uma biblioteca CSS **utility-first**: em vez de criar classes semânticas próprias (`.card`, `.botao-primario`) e escrever as propriedades dentro delas, você usa classes pequenas e prontas — cada uma faz **uma coisa só** — direto no HTML/JSX.

```html
<!-- CSS tradicional (o que vocês fizeram no módulo de CSS Modular) -->
<button class="botao botao--primario">Enviar</button>
```
```css
.botao--primario {
  background: var(--color-primary);
  color: white;
  padding: 0.5rem 1rem;
  border-radius: 8px;
}
```

```html
<!-- Tailwind -->
<button class="bg-blue-500 text-white px-4 py-2 rounded-lg">Enviar</button>
```

Não existe arquivo `.css` separado para estilizar esse botão — a aparência inteira está nas classes do próprio elemento.

### Por que isso muda em relação ao que vocês aprenderam (BEM)

Vocês aprenderam BEM porque, em HTML/CSS puro, **nomear bem as classes é a única forma de reutilizar estilo** sem repetir código. Em React, a reutilização já vem do componente: `Botao.jsx` é reaproveitado em vários lugares, então não precisamos de uma classe `.botao--primario` reutilizável — o componente já cumpre esse papel. Por isso Tailwind funciona bem dentro de componentes: a "unidade reutilizável" virou o componente, não mais a classe CSS.

As variáveis de CSS que vocês usaram (`--color-primary`, `--spacing-md`) não desaparecem — elas viram a configuração do tema do Tailwind, é só outro lugar para a mesma ideia.

### Vantagens
- Não precisa inventar nome de classe nem pular entre arquivo `.jsx` e `.css`
- Consistência automática (a escala de espaçamento e cores é padronizada)
- Fácil de apagar/remover sem medo de quebrar outra parte do projeto (a classe só afeta aquele elemento)

### Desvantagens
- HTML/JSX fica mais "poluído" visualmente
- Curva de aprendizado: precisa decorar/consultar os nomes das classes no início

---

## Instalação (projeto Vite, como o de vocês)

```bash
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

No `index.css` (o único import necessário):
```css
@import "tailwindcss";
```

---

## Entendendo os números (a escala)

Antes de ir pra lista de classes: o número depois do hífen (o `4` em `p-4`, o `64` em `w-64`) **não é livre** — é um índice de uma escala pré-definida pelo Tailwind. Cada categoria de classe (espaçamento, largura, fonte, etc.) tem sua própria escala de valores aceitos. Por isso `p-5` funciona mas `p-17` não existe por padrão.

### Escala de espaçamento (padding, margin, gap, width, height)

A regra geral: **`número × 0.25rem`**. Essa mesma escala é reaproveitada em `p-`, `m-`, `gap-`, `w-`, `h-`, `top-`, `left-` etc.

| Classe | Cálculo | Resultado |
|---|---|---|
| `p-0` | 0 × 0.25rem | 0px |
| `p-1` | 1 × 0.25rem | 4px |
| `p-2` | 2 × 0.25rem | 8px |
| `p-3` | 3 × 0.25rem | 12px |
| `p-4` | 4 × 0.25rem | 16px |
| `p-6` | 6 × 0.25rem | 24px |
| `p-8` | 8 × 0.25rem | 32px |
| `p-12` | 12 × 0.25rem | 48px |
| `p-16` | 16 × 0.25rem | 64px |
| `p-64` | 64 × 0.25rem | 256px |

Ou seja, `w-64` é "largura de 256px" — o `64` não tem nenhum significado especial além de ser o índice nessa escala. Nem todo número inteiro existe na escala (ex: não tem `p-13` por padrão, mas tem `p-12` e `p-14`); a lista completa fica na [documentação oficial](https://tailwindcss.com/docs/padding).

**Valores especiais que fogem da escala numérica:**

| Classe | Faz o quê |
|---|---|
| `w-full` | 100% |
| `w-screen` | 100vw |
| `w-auto` | auto |
| `w-1/2` `w-1/3` `w-2/3` `w-1/4` | frações (50%, 33%, 66%, 25%) |
| `w-fit` | se ajusta ao conteúdo |

### Outras categorias têm escalas próprias (não numéricas)

Nem tudo no Tailwind usa a escala `× 0.25rem`. Algumas categorias usam **nomes**, não números:

| Categoria | Escala usada | Exemplo |
|---|---|---|
| Tamanho de fonte | nomes (`xs` → `9xl`) | `text-sm` = 14px, `text-xl` = 20px |
| Tons de cor | números de 50 a 950 (não é rem!) | `bg-blue-500` — aqui o número é a *intensidade* da cor, do mais claro (`50`) ao mais escuro (`950`) |
| Cantos arredondados | nomes (`sm` → `3xl`, ou `full`) | `rounded-md` = 6px, `rounded-full` = círculo |
| Sombras | nomes (`sm` → `2xl`) | `shadow-md`, `shadow-lg` |

Ou seja: **se a classe usa número puro (`p-4`, `w-64`, `gap-8`), é a escala de espaçamento.** Se usa um nome (`text-lg`, `rounded-md`) ou um número de 50–950 (`bg-blue-500`), é uma escala diferente, específica daquela propriedade.

### E se eu precisar de um valor fora da escala?

Dá pra usar colchetes para um valor arbitrário, mas é exceção, não regra — prefira sempre a escala padrão pra manter consistência visual:

```html
<div class="w-[327px] p-[10px]">...</div>
```

---

## Principais classes

### Espaçamento (padding e margin)

A escala numérica segue: `valor × 0.25rem` (então `4` = `1rem` = `16px`).

| Classe | Faz o quê |
|---|---|
| `p-4` | padding em todos os lados (1rem) |
| `px-4` | padding horizontal (esquerda + direita) |
| `py-4` | padding vertical (cima + baixo) |
| `pt-4` `pr-4` `pb-4` `pl-4` | padding em um lado só (top/right/bottom/left) |
| `m-4` | margin em todos os lados |
| `mx-auto` | margin horizontal automática (centraliza um bloco) |
| `gap-4` | espaço entre itens filhos (flex/grid) |

### Dimensões

| Classe | Faz o quê |
|---|---|
| `w-full` | largura 100% |
| `w-1/2` | largura 50% |
| `w-64` | largura fixa (16rem) |
| `h-screen` | altura 100% da viewport |
| `min-h-screen` | altura mínima 100% da viewport |
| `max-w-md` | largura máxima (útil pra limitar texto/cards) |

### Cores

Seguem o padrão `propriedade-cor-tom`, com tons de `50` (mais claro) a `950` (mais escuro).

| Classe | Faz o quê |
|---|---|
| `bg-blue-500` | cor de fundo |
| `text-white` | cor do texto |
| `border-gray-300` | cor da borda |

### Tipografia

| Classe | Faz o quê |
|---|---|
| `text-sm` `text-base` `text-lg` `text-xl` `text-2xl` | tamanho da fonte |
| `font-light` `font-normal` `font-bold` | peso da fonte |
| `text-center` `text-left` `text-right` | alinhamento |
| `italic` | itálico |
| `underline` | sublinhado |
| `leading-relaxed` | altura da linha (line-height) |

### Flexbox

| Classe | Faz o quê |
|---|---|
| `flex` | `display: flex` |
| `flex-col` | direção em coluna |
| `items-center` | alinha no eixo cruzado (vertical, por padrão) |
| `justify-center` | alinha no eixo principal (horizontal, por padrão) |
| `justify-between` | espaça os itens nas extremidades |
| `flex-wrap` | permite quebrar linha |

### Grid

| Classe | Faz o quê |
|---|---|
| `grid` | `display: grid` |
| `grid-cols-3` | 3 colunas iguais |
| `col-span-2` | item ocupa 2 colunas |

### Bordas e cantos

| Classe | Faz o quê |
|---|---|
| `border` | borda de 1px em todos os lados |
| `border-2` | borda de 2px |
| `rounded` | cantos levemente arredondados |
| `rounded-lg` | cantos bem arredondados |
| `rounded-full` | formato circular/pílula |

### Sombras e efeitos

| Classe | Faz o quê |
|---|---|
| `shadow` `shadow-md` `shadow-lg` | sombra (intensidade crescente) |
| `opacity-50` | 50% de opacidade |

### Estados (pseudo-classes)

Qualquer classe pode ganhar um prefixo de estado:

| Classe | Faz o quê |
|---|---|
| `hover:bg-blue-600` | aplica no hover |
| `focus:ring-2` | aplica um anel de destaque no foco |
| `disabled:opacity-50` | aplica quando o elemento está desabilitado |
| `active:scale-95` | aplica enquanto está sendo clicado |

### Responsividade (mobile-first)

Prefixos que aplicam a partir de um tamanho de tela mínimo:

| Prefixo | A partir de |
|---|---|
| `sm:` | 640px |
| `md:` | 768px |
| `lg:` | 1024px |
| `xl:` | 1280px |

```html
<!-- texto pequeno no celular, grande a partir de telas médias -->
<h1 class="text-lg md:text-3xl">Título</h1>
```

### Transições

| Classe | Faz o quê |
|---|---|
| `transition` | ativa transição suave nas propriedades |
| `duration-300` | duração da transição (300ms) |

---

## Exemplo: o botão da aula, em Tailwind

```jsx
function Botao({ func, text }) {
  return (
    <button
      onClick={func}
      className="bg-blue-500 hover:bg-blue-600 text-white px-4 py-2 rounded-lg
                 transition disabled:opacity-50 disabled:cursor-not-allowed"
    >
      {text}
    </button>
  );
}
```

## Combinando Tailwind com lógica JS

```jsx
function Botao({ func, text, ativo }) {
  return (
    <button
      onClick={func}
      className={`px-4 py-2 rounded-lg text-white transition
        ${ativo ? "bg-blue-500 hover:bg-blue-600" : "bg-gray-400 cursor-not-allowed"}`}
    >
      {text}
    </button>
  );
}
```

