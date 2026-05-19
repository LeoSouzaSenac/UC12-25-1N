# Grid

> Material para estudo de CSS Grid.


## O que é CSS Grid

CSS Grid é um sistema de layout do CSS criado para organizar elementos em linhas e colunas.

O Grid trabalha em duas dimensões ao mesmo tempo:

* linhas
* colunas

Isso torna o Grid ideal para:

* layouts completos de páginas
* galerias
* dashboards
* áreas administrativas
* seções complexas
* sistemas responsivos

---

# Quando usar Grid

Use Grid quando:

* precisar controlar linhas e colunas
* quiser criar layouts mais organizados
* precisar posicionar elementos com precisão
* estiver montando a estrutura geral do site
* precisar criar áreas fixas como header, sidebar e footer
* quiser facilitar responsividade

---

# Quando NÃO usar Grid

Evite usar Grid quando:

* o layout for extremamente simples
* não existir necessidade de colunas
* apenas um elemento estiver sendo exibido

Mesmo assim, neste material vamos focar totalmente em Grid.

---

# Ativando o Grid

Para transformar um elemento em Grid:

```css
/*
   Transforma o elemento em um Grid Container
   Todos os filhos diretos viram Grid Items
*/

.container{
    display:grid;
}
```

Todos os filhos diretos passam a ser Grid Items.

---

# Criando Colunas

## grid-template-columns

Define quantas colunas existirão.

```css
/*
   Cria 3 colunas fixas
   Cada coluna terá 200px
*/

.container{
    display:grid;

    grid-template-columns:
    200px
    200px
    200px;
}
```

Resultado:

* 3 colunas
* cada uma com 200px

---

# Unidade fr

A unidade `fr` significa:

"fração do espaço disponível"

```css
/*
   fr = fraction
   Divide o espaço disponível
*/

.container{
    display:grid;

    grid-template-columns:
    1fr 1fr;
}
```

Resultado:

* duas colunas
* cada uma ocupa metade do espaço

---

# Misturando tamanhos

```css
/*
   Primeira coluna fixa
   Segunda ocupa o restante
*/

.container{
    display:grid;

    grid-template-columns:
    250px 1fr;
}
```

Resultado:

* primeira coluna fixa
* segunda ocupa o restante

Muito usado em:

* dashboards
* sidebars
* layouts administrativos

---

# Criando Linhas

## grid-template-rows

```css
.container{
    display:grid;
    grid-template-rows:100px 300px 100px;
}
```

Resultado:

* 3 linhas
* alturas definidas manualmente

---

# Espaçamento entre elementos

## gap

```css
/*
   Espaçamento entre linhas e colunas
*/

.container{
    display:grid;

    gap:20px;
}
```

Também pode usar:

```css
row-gap:20px;
column-gap:10px;
```

---

# Repetição com repeat()

Evita repetir valores.

```css
.container{
    display:grid;
    grid-template-columns:repeat(3, 1fr);
}
```

Resultado:

* 3 colunas iguais

Equivale a:

```css
grid-template-columns:1fr 1fr 1fr;
```

---

# Responsividade com auto-fit

```css
.container{
    display:grid;
    grid-template-columns:repeat(auto-fit, minmax(250px, 1fr));
}
```

Isso cria:

* colunas automáticas
* layout responsivo
* quebra automática

Muito usado em:

* cards
* galerias
* produtos
* blogs

---

# minmax()

Define tamanho mínimo e máximo.

```css
grid-template-columns:repeat(auto-fit, minmax(200px, 1fr));
```

Significa:

* mínimo de 200px
* máximo ocupando espaço disponível

---

# Posicionando elementos

## grid-column

```css
.item{
    grid-column:1/3;
}
```

O elemento ocupará:

* da coluna 1 até a 3

---

## grid-row

```css
.item{
    grid-row:1/3;
}
```

O elemento ocupará:

* da linha 1 até a 3

---

# Exemplo visual

```css
.container{
    display:grid;
    grid-template-columns:1fr 1fr 1fr;
}

.item1{
    grid-column:1/3;
}
```

Resultado:

* item1 ocupa duas colunas

---

# Grid Areas

Uma das funcionalidades mais poderosas do Grid.

Permite criar áreas nomeadas.

---

## Exemplo

```css
.container{
    display:grid;

    grid-template-areas:
    "header header"
    "sidebar main"
    "footer footer";
}
```

Agora cada elemento recebe uma área:

```css
header{
    grid-area:header;
}

aside{
    grid-area:sidebar;
}

main{
    grid-area:main;
}

footer{
    grid-area:footer;
}
```

---

# Exemplo completo de layout

---

## Estrutura visual do layout

```text
-----------------------------------
|             HEADER              |
-----------------------------------
| SIDEBAR |        MAIN           |
|         |                       |
|         |                       |
-----------------------------------
|             FOOTER              |
-----------------------------------
```

Esse tipo de estrutura é extremamente comum em:

* dashboards
* sistemas administrativos
* painéis
* sites corporativos
* plataformas web

## HTML

```html
<body>

<header>Header</header>
<aside>Sidebar</aside>
<main>Conteúdo</main>
<footer>Footer</footer>

</body>
```

## CSS

```css
/*
====================================
           LAYOUT PRINCIPAL
====================================
*/

body{

    /* Altura mínima da tela */
    min-height:100vh;

    display:grid;

    grid-template-columns:250px 1fr;

    grid-template-rows:
    100px
    1fr
    100px;

    grid-template-areas:
    "header header"
    "sidebar main"
    "footer footer";
}

header{
    grid-area:header;
}

aside{
    grid-area:sidebar;
}

main{
    grid-area:main;
}

footer{
    grid-area:footer;
}
```

---

# Centralização no Grid

## justify-items

Alinha horizontalmente.

```css
.container{
    justify-items:center;
}
```

---

## align-items

Alinha verticalmente.

```css
.container{
    align-items:center;
}
```

---

## place-items

Atalho:

```css
.container{
    place-items:center;
}
```

---

# Alinhamento do Grid inteiro

## justify-content

Move o Grid horizontalmente.

---

## align-content

Move o Grid verticalmente.

---

# Grid Implícito

O Grid pode criar linhas automaticamente.

```css
.container{
    display:grid;
    grid-template-columns:1fr 1fr 1fr;
}
```

Se existirem muitos elementos:

* novas linhas são criadas automaticamente

---

# grid-auto-rows

Define tamanho das linhas automáticas.

```css
.container{
    grid-auto-rows:200px;
}
```

---

# Grid Responsivo Profissional

## Estrutura moderna

```css
.cards{
    display:grid;

    grid-template-columns:
    repeat(auto-fit, minmax(250px, 1fr));

    gap:20px;
}
```

Isso é extremamente usado em:

* marketplaces
* blogs
* dashboards
* landing pages
* sistemas modernos

---

# Erros comuns

## 1. Definir tamanhos fixos demais

Ruim:

```css
grid-template-columns:300px 300px 300px;
```

Melhor:

```css
grid-template-columns:repeat(3, 1fr);
```

Ou:

```css
grid-template-columns:
repeat(auto-fit, minmax(250px, 1fr));
```

---

## 3. Ignorar responsividade

Layouts fixos quebram no celular.

---

# Organização moderna com Grid

```css
/*
====================================
          GRID NA PÁGINA
====================================
*/

body{
    display:grid;
}

/*
====================================
          GRID NOS CARDS
====================================
*/

.cards{
    display:grid;

    grid-template-columns:
    repeat(auto-fit, minmax(250px, 1fr));

    gap:20px;
}
```

O Grid pode ser usado tanto:

* na estrutura principal
* quanto em seções internas

---

# Quando usar Grid na prática

## Use Grid para:

* layout principal
* páginas completas
* dashboards
* galerias
* blogs
* cards responsivos
* sistemas administrativos
* layouts complexos

---

# Resumo Visual

| Situação                  | Grid      |
| ------------------------- | --------- |
| Layout completo da página | Excelente |
| Dashboard                 | Excelente |
| Galeria de imagens        | Excelente |
| Cards responsivos         | Excelente |
| Landing pages             | Excelente |
| Blogs                     | Excelente |
| Sistemas administrativos  | Excelente |

---

# Fluxo moderno usando Grid

````text
GRID -> controla linhas e colunas
GRID -> organiza áreas da página
GRID -> facilita responsividade
GRID -> reduz gambiarra no CSS
```text
GRID  -> Estrutura da página
FLEX  -> Componentes internos
````

---

# Conclusão

CSS Grid é uma das ferramentas mais importantes do front-end moderno.

Ele resolve problemas que antigamente exigiam:

* float
* tabelas
* hacks de margin
* cálculos manuais
* frameworks inteiros

Hoje o Grid é uma das ferramentas mais importantes para construção de layouts modernos.

Com ele é possível criar:

* páginas completas
* dashboards
* galerias
* sistemas responsivos
* estruturas complexas

Tudo de forma organizada, limpa e profissional.
