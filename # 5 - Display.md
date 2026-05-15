# Display no CSS – block, inline, inline-block

## 1. O que é `display`

A propriedade `display` define **como um elemento se comporta no layout da página**.

Ela controla:

* Se o elemento quebra linha ou não
* Se ocupa toda a largura
* Se aceita largura (`width`) e altura (`height`)

---

## 2. `display: block`

```css
display: block;
```

### Comportamento

* Ocupa **toda a largura disponível**
* Sempre começa em uma nova linha
* Aceita `width` e `height`

### Exemplo

```html
<div>Caixa 1</div>
<div>Caixa 2</div>
```

Resultado:

```
[ Caixa 1        ]
[ Caixa 2        ]
```

### Explicação

Mesmo que o conteúdo seja pequeno, o elemento se estica horizontalmente.

Você pode limitar isso com:

```css
width: 200px;
```

### Elementos que já são block

* `div`
* `p`
* `h1` até `h6`

### Quando usar

* Estrutura da página
* Seções
* Containers

---

## 3. `display: inline`

```css
display: inline;
```

### Comportamento

* Fica na mesma linha
* Ocupa apenas o espaço do conteúdo
* **Não aceita `width` e `height`**

### Exemplo

```html
<span>Olá</span>
<span>Mundo</span>
```

Resultado:

```
Olá Mundo
```

### Explicação

O elemento se comporta como texto.

Se você tentar:

```css
width: 200px;
```

Não terá efeito.

### Elementos inline

* `span`
* `a`
* `strong`

### Quando usar

* Partes de texto
* Links
* Destaques dentro de texto

---

## 4. `display: inline-block`

```css
display: inline-block;
```

### Comportamento

* Fica na mesma linha (como inline)
* Aceita `width` e `height` (como block)

### Exemplo

```css
.box {
  display: inline-block;
  width: 100px;
  height: 100px;
}
```

```html
<div class="box"></div>
<div class="box"></div>
```

Resultado:

```
[  ] [  ]
```

### Explicação

É uma combinação dos dois comportamentos:

* Layout horizontal
* Controle de tamanho

### Quando usar

* Botões
* Cards simples
* Elementos lado a lado

---

## 5. Comparação direta

| Tipo         | Quebra linha | Width/Height | Uso principal |
| ------------ | ------------ | ------------ | ------------- |
| block        | sim          | sim          | layout        |
| inline       | não          | não          | texto         |
| inline-block | não          | sim          | interface     |

---

## 6. `display: none`

```css
display: none;
```

### Comportamento

* Remove o elemento da tela
* Não ocupa espaço

### Diferença importante

```css
visibility: hidden;
```

* Fica invisível
* Mas ainda ocupa espaço

---

## 7. Erro comum

```css
span {
  width: 200px;
}
```

Isso não funciona porque `span` é inline.

Solução:

```css
span {
  display: inline-block;
  width: 200px;
}
```

---

## 8. Regra prática

* Quer ocupar a linha inteira → `block`
* Quer comportamento de texto → `inline`
* Quer lado a lado com controle → `inline-block`

---

# Exercícios

## Exercício 1 – Identificação

Dado o HTML:

```html
<div>Texto</div>
<span>Texto</span>
```

Responda:

1. Qual elemento ocupa a linha inteira?
2. Qual fica na mesma linha?

---

## Exercício 2 – Testando block

Crie duas `div`:

* Defina largura de 200px
* Adicione cor de fundo

Observe:

* Elas ficam lado a lado ou uma abaixo da outra?
* Por quê?

---

## Exercício 3 – Testando inline

Crie dois `span`:

* Tente aplicar `width: 200px`
* Adicione cor de fundo

Pergunta:

* A largura funcionou?
* Por quê?

---

## Exercício 4 – Corrigindo inline

Pegue o exercício anterior e altere:

```css
display: inline-block;
```

Agora responda:

* O `width` funciona?
* O comportamento mudou?

---

## Exercício 5 – Layout lado a lado

Crie 3 caixas:

* largura: 100px
* altura: 100px
* cor de fundo

Faça elas ficarem lado a lado usando:

* `inline-block`

---

## Exercício 6 – Comparação prática

Crie três elementos:

* um com `block`
* um com `inline`
* um com `inline-block`

Aplique:

```css
width: 150px;
height: 80px;
background: gray;
```

Pergunta:

* Qual respeita width/height?
* Qual ignora?

---

## Exercício 7 – Ocultar elemento

Crie uma `div` e aplique:

```css
display: none;
```

Depois troque por:

```css
visibility: hidden;
```

Pergunta:

* Qual diferença visual você percebe?

---

## Exercício 8 – Desafio

Crie um menu simples:

* 4 links (`<a>`)
* todos lado a lado
* com espaçamento entre eles

Dica:

* use `inline-block`

---

## Exercício 9 – Debug

Corrija o problema:

```css
a {
  width: 200px;
  background: gray;
}
```

Pergunta:

* Por que não funciona?
* Como corrigir?

---

## Exercício 10 – Misturando conceitos

Crie um card:

* largura fixa
* padding interno
* título + botão

Faça o botão:

* ficar ao lado do texto
* ter largura definida

Pergunta:

* qual display você usou no botão?
* por quê?

---
