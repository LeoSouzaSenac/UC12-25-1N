# Flexbox

# O que é Flexbox

Flexbox é um sistema de layout do CSS criado para:

* alinhar elementos
* distribuir espaço
* organizar componentes
* facilitar responsividade

O Flexbox trabalha em UMA dimensão:

* linha
* OU coluna

---

# O que significa display:flex

Quando usamos:

```css
.container{
    display:flex;
}
```

O elemento vira um:

* Flex Container

E seus filhos diretos viram:

* Flex Items

---

# Exemplo básico

## HTML

```html
<div class="container">

    <div class="item">Item 1</div>
    <div class="item">Item 2</div>
    <div class="item">Item 3</div>

</div>
```

---

## CSS

```css
/*
====================================
           FLEX CONTAINER
====================================
*/

.container{

    display:flex;

    background:#eeeeee;

    padding:20px;

    gap:20px;
}

/*
====================================
             FLEX ITEMS
====================================
*/

.item{

    background:#4caf50;

    color:white;

    padding:20px;
}
```

---

# Eixos do Flexbox

O Flexbox trabalha com:

* eixo principal
* eixo cruzado

---

## Eixo principal

Por padrão:

```text
Horizontal
```

---

## Eixo cruzado

Por padrão:

```text
Vertical
```

---

# flex-direction

Define a direção dos itens.

---

## row

```css
.container{
    flex-direction:row;
}
```

Itens em linha.

---

## row-reverse

```css
.container{
    flex-direction:row-reverse;
}
```

Linha invertida.

---

## column

```css
.container{
    flex-direction:column;
}
```

Itens em coluna.

---

## column-reverse

```css
.container{
    flex-direction:column-reverse;
}
```

Coluna invertida.

---

# justify-content

Controla alinhamento no eixo principal.

---

# flex-start

```css
.container{
    justify-content:flex-start;
}
```

Itens no início.

---

# flex-end

```css
.container{
    justify-content:flex-end;
}
```

Itens no final.

---

# center

```css
.container{
    justify-content:center;
}
```

Itens centralizados.

---

# space-between

```css
.container{
    justify-content:space-between;
}
```

Espaço entre os itens.

---

# space-around

```css
.container{
    justify-content:space-around;
}
```

Espaço ao redor.

---

# space-evenly

```css
.container{
    justify-content:space-evenly;
}
```

Espaçamento totalmente igual.

---

# align-items

Controla alinhamento no eixo cruzado.

---

## flex-start

```css
.container{
    align-items:flex-start;
}
```

Itens no topo.

---

## flex-end

```css
.container{
    align-items:flex-end;
}
```

Itens no final.

---

## center

```css
.container{
    align-items:center;
}
```

Itens centralizados.

---

## stretch

```css
.container{
    align-items:stretch;
}
```

Itens esticam.

---

## baseline

```css
.container{
    align-items:baseline;
}
```

Alinha pela linha base do texto.

---

# Centralização perfeita

Uma das coisas mais famosas do Flexbox.

```css
.container{

    display:flex;

    justify-content:center;

    align-items:center;
}
```

---

# flex-wrap

Define se os itens quebram linha.

---

## nowrap

```css
.container{
    flex-wrap:nowrap;
}
```

Tudo fica em uma linha.

---

## wrap

```css
.container{
    flex-wrap:wrap;
}
```

Itens quebram linha.

---

## wrap-reverse

```css
.container{
    flex-wrap:wrap-reverse;
}
```

Quebra invertida.

---

# gap

Cria espaçamento entre itens.

```css
.container{
    gap:20px;
}
```

---

# align-content

Usado quando existem múltiplas linhas.

Funciona junto com:

```css
flex-wrap:wrap;
```

---

## Exemplo

```css
.container{

    display:flex;

    flex-wrap:wrap;

    align-content:center;
}
```

---

# Propriedades dos itens

Agora veremos propriedades usadas nos filhos.

---

# flex-grow

Define se o item pode crescer.

---

## Exemplo

```css
.item{
    flex-grow:1;
}
```

Todos os itens crescem igualmente.

---

# flex-shrink

Define se o item pode encolher.

---

## Exemplo

```css
.item{
    flex-shrink:1;
}
```

O item poderá diminuir.

---

# flex-basis

Define tamanho inicial.

---

## Exemplo

```css
.item{
    flex-basis:200px;
}
```

---

# shorthand flex

Atalho extremamente usado.

```css
.item{
    flex:1;
}
```

Equivale aproximadamente a:

```css
.item{
    flex-grow:1;
    flex-shrink:1;
    flex-basis:0;
}
```

---

# align-self

Permite alinhar um item individualmente.

---

## Exemplo

```css
.item{
    align-self:center;
}
```

---

# order

Altera a ordem visual.

---

## Exemplo

```css
.item{
    order:1;
}
```

---

# Exemplo profissional

## Navbar moderna

### HTML

```html
<header>

    <h1>Wanderland</h1>

    <nav>

        <a href="#">Home</a>
        <a href="#">About</a>
        <a href="#">Contact</a>

    </nav>

</header>
```

---

## CSS

```css
header{

    display:flex;

    justify-content:space-between;

    align-items:center;

    padding:20px;
}

nav{

    display:flex;

    gap:20px;
}
```

---

# Cards responsivos

## CSS

```css
.cards{

    display:flex;

    flex-wrap:wrap;

    gap:20px;
}

.card{

    flex:1 1 300px;
}
```

---

# Explicando flex:1 1 300px

```css
flex-grow:1;
flex-shrink:1;
flex-basis:300px;
```

---

# Boas práticas

## Use gap

Evite:

```css
margin-right
margin-left
```

Prefira:

```css
gap
```

---

## Evite height fixa desnecessária

Layouts rígidos quebram.

---

## Use flex-wrap

Ajuda responsividade.

---

## Evite exagero de flexbox

Nem tudo precisa ser flex.

---

# Erros comuns

## 1. Esquecer display:flex

```css
justify-content:center;
```

Sem:

```css
display:flex;
```

Não funciona.

---

## 2. Confundir justify-content e align-items

### justify-content

Trabalha no eixo principal.

### align-items

Trabalha no eixo cruzado.

---

## 3. Não usar flex-wrap

Os itens podem esmagar.

---

# Quando usar Flexbox

Flexbox é excelente para:

* navbar
* menus
* alinhamentos
* centralização
* formulários
* botões
* componentes
* listas horizontais
* cards

---

# Resumo Visual

| Propriedade     | Função                 |
| --------------- | ---------------------- |
| display:flex    | Ativa o Flexbox        |
| flex-direction  | Direção dos itens      |
| justify-content | Alinhamento principal  |
| align-items     | Alinhamento cruzado    |
| flex-wrap       | Quebra de linha        |
| gap             | Espaçamento            |
| flex-grow       | Crescimento            |
| flex-shrink     | Encolhimento           |
| flex-basis      | Tamanho inicial        |
| order           | Ordem visual           |
| align-self      | Alinhamento individual |

---

# Fluxo mental profissional

```text
justify-content -> eixo principal
align-items     -> eixo cruzado
```

---

# Conclusão

Flexbox revolucionou a criação de layouts no CSS.

Antes dele, alinhamentos eram extremamente difíceis.

Hoje o Flexbox resolve facilmente:

* centralização
* alinhamento
* espaçamento
* distribuição de elementos
* responsividade

Ele é uma das ferramentas mais importantes do front-end moderno.
