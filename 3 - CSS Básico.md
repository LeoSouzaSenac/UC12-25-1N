# CSS Básico – Guia Completo e Didático

## 1. O que é CSS?

CSS (**Cascading Style Sheets**) é a linguagem usada para **controlar a aparência** de uma página HTML.

Pensa assim:

* HTML = estrutura (esqueleto)
* CSS = aparência (roupa / visual)

Sem CSS, o site fica todo “cru”, sem design.

---

## 2. Como o CSS funciona

O CSS funciona assim:

> Você escolhe um elemento, e aí define como ele deve parecer

Exemplo:

```css
p {
  color: red; 
}
```

Tradução:

> “Todo parágrafo (`p`) deve ficar vermelho”

---

## 3. Formas de usar CSS

### 3.1 Inline (direto no elemento)

```html
<p style="color: red;">Texto</p>
```

✔ Vantagem: rápido
✖ Problema: bagunça o código

Use só em testes.

---

### 3.2 Interno (dentro do HTML)

```html
<style>
  p {
    color: red;
  }
</style>
```

✔ Melhor que inline
✖ Ainda mistura HTML com CSS

---

### 3.3 Externo (correto)

```html
<link rel="stylesheet" href="style.css">
```

✔ Organização
✔ Reutilização
✔ Profissional

**IMPORTANTE:** A tag `<link>` deve estar dentro do `<head></head>`! 

---

## 4. Sintaxe do CSS (como escrever)

```css
seletor {
  propriedade: valor;
}
```

Exemplo:

```css
p {
  color: blue;
  font-size: 20px;
}
```

* `p` → quem será afetado
* `color` → o que vai mudar
* `blue` → como vai ficar

---

## 5. Seletores (como escolher elementos)

### 5.1 Seletor de elemento

```css
p {
  color: red;
}
```

Afeta **todos os `<p>`**
Quando usamos um seletor de elemento, TODOS os elementos com aquela mesma tag serão afetados (a não ser que outros seletores os afetem também)

---

### 5.2 Classe (mais usada)

```css
.texto {
  color: green;
}
```

HTML:

```html
<p class="texto"></p>
```

✔ Pode repetir em vários elementos
✔ Mais flexível

Quando usamos seletores de classe, QUALQUER elemento que utilize a mesma classe no HTML será afetado

---

### 5.3 ID (único)

```css
#titulo {
  color: blue;
}
```

HTML:

```html
<h1 id="titulo"></h1>
```

✔ Só deve existir **um por página**

O seletor de ID é semelhante ao de classe, porém, ID é para apenas um único elemento, não vários

---

### 5.4 Universal

```css
* {
  margin: 0;
}
```

Afeta **tudo**. Simples assim. Usamos, normalmente, para resetar margin e padding, entre outros.

---

### 5.5 Descendente

```css
section p {
  color: red;
}
```

Tradução:

> Todo `<p>` dentro de `<div>`

Usando este tipo de seletor, qualquer elemento B que esteja dentro de um elemento A, mesmo que dentro de mais elementos, será afetado.
Exemplo:

```html
<section>
  <div>
    <p>Olá pessoal!</p>
  </div> 
</section>
```
No exemplo a cima, `<p>` seria afetado pois, mesmo que indiretamente, está localizado dentro de `<section>`

---

### 5.6 Filho direto

```css
div > p {
  color: blue;
}
```

Só funciona se for **filho direto**

---

## 6. Cores (como estilizar visualmente)

Você pode usar:

```css
color: red;
color: #ff0000;
color: rgb(255, 0, 0);
```

### Dica didática:

* Nome → simples
* HEX → padrão web (é o código que vem com #)
* RGB → controle total (vem de 'Red | Green | Blue', que formam as outras cores e tons dependendo da quantidade e intensidade de cada um)

---

## 7. Texto

```css
p {
  font-size: 16px;     /* tamanho */
  font-weight: bold;   /* negrito */
  text-align: center;  /* alinhamento */
  text-transform: uppercase; /* maiúsculo */
}
```

Pensa assim:

* font-size → tamanho da letra
* font-weight → “força” da letra
* text-align → posição

---

## 8. Background (fundo)

```css
div {
  background-color: yellow;
}
```

Com imagem:

```css
div {
  background-image: url('img.jpg');
  background-size: cover;
  background-position: center;
}
```

### Conceito importante:

* `cover` → ocupa tudo
* `center` → centraliza

---

## 9. Box Model (ESSENCIAL)

Todo elemento é uma caixa:

```
[ margin ]
  [ border ]
    [ padding ]
      [ conteúdo ]
```

Exemplo:

```css
div {
  width: 200px;
  padding: 20px;
  border: 2px solid black;
  margin: 10px;
}
```

### Entenda de verdade:

* **Content** → o conteúdo
* **Padding** → espaço interno
* **Border** → borda
* **Margin** → espaço externo

Se você não entender isso, vai sofrer no CSS.

---

## 10. Display (comportamento do elemento)

```css
display: block;
display: inline;
display: inline-block;
display: none;
```

### Diferença:

* block → ocupa linha inteira
* inline → fica lado a lado
* inline-block → mistura dos dois
* none → desaparece

---

## 11. Position (posição)

```css
position: relative;
position: absolute;
position: fixed;
```

### Explicação simples:

* **relative** → move baseado nele mesmo
* **absolute** → sai do fluxo e usa referência do pai
* **fixed** → fica preso na tela

Exemplo:

```css
div {
  position: absolute;
  top: 0;
  left: 0;
}
```

---

## 12. Flexbox (organizar layout fácil)

```css
.container {
  display: flex;
}
```

Agora você controla os filhos:

```css
.container {
  display: flex;
  justify-content: center; /* horizontal */
  align-items: center;     /* vertical */
  gap: 10px;
}
```

### Regra de ouro:

* justify → eixo horizontal
* align → eixo vertical

---

## 13. Grid (layout mais avançado)

```css
.container {
  display: grid;
  grid-template-columns: 1fr 1fr;
}
```

Tradução:

> 2 colunas iguais

---

## 14. Tamanhos e unidades

```css
width: 100px;
height: 50px;
```

Unidades:

* px → fixo
* % → relativo ao pai
* rem → baseado no root
* vw/vh → tela

---

## 15. Bordas

```css
border: 1px solid black;
border-radius: 10px;
```

* `solid` → tipo da borda
* `border-radius` → arredondar

---

## 16. Sombra

```css
box-shadow: 2px 2px 5px gray;
```

Ordem:

* horizontal
* vertical
* blur

---

## 17. Hover (interação)

```css
button:hover {
  background-color: blue;
}
```

Quando o mouse passa, muda.

---

## 18. Transições (animação suave)

```css
button {
  transition: 0.3s;
}
```

Sem isso → mudança brusca
Com isso → suave

---

## 19. Responsividade

```css
@media (max-width: 600px) {
  body {
    background: gray;
  }
}
```

Tradução:

> Se a tela for pequena, muda o estilo

---

## 20. Boas práticas

* Use classes (evite IDs)
* Separe CSS do HTML
* Nomeie bem (`.btn`, `.card`, `.container`)
* Organize o código
