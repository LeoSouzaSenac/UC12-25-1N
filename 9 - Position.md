# Guia Completo de `position` em CSS

(com explicações detalhadas, responsividade e exemplos práticos)

O `position` em CSS controla como um elemento será posicionado na página.

Ele altera:

* o comportamento do elemento no fluxo do layout;
* a forma como propriedades como `top`, `right`, `bottom` e `left` funcionam;
* a relação do elemento com outros elementos da página;
* o comportamento durante a rolagem.

Os valores principais são:

* `static`
* `relative`
* `absolute`
* `fixed`
* `sticky`

---

# Entendendo o fluxo normal da página

Antes de estudar `position`, é importante entender o conceito de **fluxo normal do documento**.

Por padrão, os elementos HTML são renderizados um abaixo do outro seguindo a estrutura do HTML.

Exemplo:

```html
<h1>Título</h1>
<p>Parágrafo</p>
<button>Botão</button>
```

Nesse caso:

* o `<h1>` aparece primeiro;
* o `<p>` aparece abaixo;
* o `<button>` aparece depois.

Isso é o fluxo normal.

Alguns tipos de `position` mantêm o elemento nesse fluxo.
Outros removem o elemento completamente do fluxo.

---

# 1. `position: static`

## Descrição

É o valor padrão de todos os elementos HTML.

O elemento continua no fluxo normal da página e ignora propriedades como:

* `top`
* `right`
* `bottom`
* `left`

## Características

* Não sai do fluxo.
* Não pode ser deslocado usando `top`, `left`, etc.
* É totalmente responsivo naturalmente.

## Exemplo

```html
<p style="position: static; background: #eee; padding: 10px;">
  Parágrafo com position: static
</p>
```

## Quando usar

Use `static` quando não houver necessidade de posicionamento especial.

Exemplos:

* textos;
* títulos;
* seções;
* cards comuns;
* elementos do layout padrão.

---

# 2. `position: relative`

## Descrição

O elemento continua ocupando seu espaço normal no layout, mas agora pode ser deslocado visualmente usando:

* `top`
* `right`
* `bottom`
* `left`

## Importante

Mesmo sendo deslocado, o espaço original do elemento continua reservado.

---

## Exemplo simples

```html
<div style="
  position: relative;
  left: 20px;
  top: 10px;
  background: lightblue;
  padding: 10px;">
  
  Elemento deslocado
</div>
```

---

# Como funciona o deslocamento

```css
position: relative;
top: 10px;
left: 20px;
```

Significa:

* mover 10px para baixo;
* mover 20px para a direita.

---

# Uso mais importante do `relative`

O uso mais comum de `position: relative` é servir como referência para elementos com `position: absolute`.

---

# Exemplo prático: ícone com contador

```html
<div style="position: relative; width: 40px;">
  
  <img 
    src="https://cdn-icons-png.flaticon.com/512/107/107831.png" 
    width="40" 
  />

  <span style="
    position: absolute;
    top: -5px;
    right: -5px;
    background: red;
    color: white;
    border-radius: 50%;
    font-size: 12px;
    padding: 2px 5px;">
    3
  </span>

</div>
```

---

# Por que usamos `position: relative` no `<div>`?

O `<div>` se torna a referência de posicionamento do `<span>`.

Quando um elemento possui:

```css
position: absolute;
```

ele procura o ancestral mais próximo que tenha:

```css
position: relative;
absolute;
fixed;
sticky;
```

Se encontrar, usará esse elemento como referência.

Se não encontrar, usará o `body` ou o viewport da página.

---

# O que acontece aqui?

O `<span>`:

```css
position: absolute;
top: -5px;
right: -5px;
```

fica posicionado no canto superior direito do `<div>`.

Isso cria o efeito clássico de:

* notificações;
* badges;
* contadores;
* alertas em ícones.

---

# Responsividade do `relative`

O `relative` é bastante seguro para layouts responsivos.

Prefira:

```css
rem
em
%
vw
vh
```

Evite excesso de:

```css
px
```

---

# 3. `position: absolute`

## Descrição

Remove o elemento do fluxo normal da página.

O elemento pode ser posicionado livremente usando:

* `top`
* `right`
* `bottom`
* `left`

---

# Comportamento importante

O elemento com `absolute` procura um ancestral com:

```css
position: relative;
absolute;
fixed;
sticky;
```

para usar como referência.

Se não encontrar, usa o viewport.

---

# Exemplo simples

```html
<div style="
  position: relative;
  width: 300px;
  height: 150px;
  background: #ddd;">

  <div style="
    position: absolute;
    top: 10px;
    right: 10px;
    background: red;
    color: white;
    padding: 10px;">
    
    Caixa absoluta

  </div>

</div>
```

---

# O que acontece aqui?

* O container pai possui `position: relative`.
* A caixa vermelha usa `absolute`.
* Então ela será posicionada dentro do container.

---

# O que significa “sair do fluxo”?

O elemento:

* deixa de ocupar espaço;
* não empurra outros elementos;
* pode sobrepor conteúdos.

---

# Exemplos práticos de `absolute`

## Menus dropdown

```css
.menu {
  position: relative;
}

.dropdown {
  position: absolute;
  top: 100%;
  left: 0;
}
```

---

## Botão de fechar modal

```css
.fechar {
  position: absolute;
  top: 10px;
  right: 10px;
}
```

---

## Tooltips

```css
.tooltip {
  position: absolute;
  bottom: 120%;
}
```

---

# Cuidados com responsividade

`absolute` pode quebrar layouts facilmente.

Problemas comuns:

* elementos saindo da tela;
* sobreposição;
* desalinhamentos em mobile.

---

# Boas práticas

Prefira:

```css
top: 10%;
left: 50%;
transform: translateX(-50%);
```

em vez de muitos valores fixos em `px`.

---

# 4. `position: fixed`

## Descrição

O elemento fica fixo na janela do navegador.

Mesmo rolando a página, ele continua visível.

---

# Características

* Sai do fluxo.
* Usa o viewport como referência.
* Permanece fixo durante a rolagem.

---

# Exemplo: botão voltar ao topo

```html
<style>
.botao-topo {
  position: fixed;
  bottom: 20px;
  right: 20px;

  background: #ff6600;
  color: white;

  padding: 12px;
  border-radius: 50%;

  font-size: 20px;
  cursor: pointer;
}
</style>

<button class="botao-topo">
  ↑
</button>
```

---

# Exemplos comuns

* chats flutuantes;
* botões de ação;
* menu mobile fixo;
* barras de cookies;
* suporte online;
* botão de WhatsApp.

---

# Problemas comuns do `fixed`

Em telas pequenas:

* pode cobrir conteúdo;
* atrapalhar cliques;
* ocupar muito espaço.

---

# Boas práticas

Use `media queries`.

Exemplo:

```css
@media (max-width: 768px) {

  .botao-topo {
    bottom: 10px;
    right: 10px;
    padding: 8px;
  }

}
```

---

# 5. `position: sticky`

## Descrição

O elemento se comporta como `relative` até atingir um limite definido.

Depois disso, ele “gruda” na tela.

---

# Exemplo

```html
<div style="
  height: 250px;
  overflow-y: scroll;
  border: 1px solid gray;">

  <div style="
    position: sticky;
    top: 0;
    background: #ffd54f;
    padding: 10px;">
    
    Cabeçalho fixo

  </div>

  <div style="height: 600px;">
    Muito conteúdo rolável...
  </div>

</div>
```

---

# Como o `sticky` funciona

```css
position: sticky;
top: 0;
```

Significa:

* enquanto o elemento estiver no fluxo normal, age como `relative`;
* ao atingir `top: 0`, fica preso no topo.

---

# Requisitos importantes do `sticky`

O elemento pai:

* não pode ter `overflow: hidden`;
* deve permitir rolagem;
* precisa ter espaço suficiente.

---

# Exemplos comuns

* cabeçalhos;
* menus;
* filtros laterais;
* índices;
* tabelas;
* navegação de seções.

---

# Comparação completa

| position | Sai do fluxo? | Usa referência? | Fixa na tela? | Responsivo |
| -------- | ------------- | --------------- | ------------- | ---------- |
| static   | Não           | Não             | Não           | Sim        |
| relative | Não           | Próprio local   | Não           | Sim        |
| absolute | Sim           | Pai posicionado | Não           | Parcial    |
| fixed    | Sim           | Viewport        | Sim           | Parcial    |
| sticky   | Parcialmente  | Container       | Parcialmente  | Sim        |

---

# Diferença entre `absolute`, `fixed` e `sticky`

## `absolute`

Fica preso ao pai posicionado.

## `fixed`

Fica preso à tela.

## `sticky`

Fica normal até certo ponto da rolagem.

---

# Propriedade `z-index`

Muito usada junto com `position`.

Controla quem fica “na frente”.

---

# Exemplo

```css
.caixa {
  position: absolute;
  z-index: 10;
}
```

Quanto maior o valor:

* mais na frente o elemento aparece.

---

# Exemplo visual

```html
<div style="
  position: relative;
  width: 300px;
  height: 200px;">

  <div style="
    position: absolute;
    width: 100px;
    height: 100px;
    background: red;
    z-index: 1;">
  </div>

  <div style="
    position: absolute;
    left: 40px;
    top: 40px;
    width: 100px;
    height: 100px;
    background: blue;
    z-index: 2;">
  </div>

</div>
```

A caixa azul ficará na frente.

---

# Boas práticas para responsividade

## Use unidades relativas

Prefira:

```css
%
em
rem
vw
vh
```

---

## Evite muitos valores fixos

Muito `px` pode quebrar layouts.

---

## Use Flexbox e Grid

`position` não deve ser usado para estruturar layouts inteiros.

Prefira:

* Flexbox;
* CSS Grid.

Use `position` apenas para:

* detalhes;
* sobreposições;
* elementos especiais.

---

# Quando NÃO usar `position`

Evite usar para:

* alinhar layouts inteiros;
* criar colunas;
* centralizar páginas completas.

Nesses casos, use:

* Flexbox;
* Grid.

---

# Exemplo moderno combinando Flexbox + Position

```html
<div class="card">

  <span class="badge">
    Novo
  </span>

  <h2>Produto</h2>

</div>
```

```css
.card {
  position: relative;

  display: flex;
  align-items: center;
  justify-content: center;

  width: 300px;
  height: 200px;

  background: #eee;
}

.badge {
  position: absolute;

  top: 10px;
  right: 10px;

  background: crimson;
  color: white;

  padding: 5px 10px;
}
```

---

# Como criar um header que acompanha a rolagem da página

Existem duas formas principais:

* usando `fixed`;
* usando `sticky`.

---

# Opção 1 — Header com `fixed`

O header fica sempre visível na tela.

## HTML

```html
<header class="header">
  Meu Site
</header>

<main>
  <p>Conteúdo...</p>
</main>
```

---

## CSS

```css
body {
  margin: 0;
}

.header {
  position: fixed;

  top: 0;
  left: 0;

  width: 100%;

  background: #222;
  color: white;

  padding: 20px;

  z-index: 1000;
}

main {
  padding-top: 80px;
}
```

---

# Explicação

## `position: fixed`

Fixa o header na tela.

---

## `top: 0`

Mantém no topo.

---

## `width: 100%`

Faz ocupar toda largura.

---

## `z-index: 1000`

Mantém o header acima dos outros elementos.

---

## `padding-top` no `main`

Muito importante.

Sem isso, o conteúdo ficará escondido atrás do header.

---

# Resultado

O header permanece visível mesmo durante a rolagem.

---

# Opção 2 — Header com `sticky`

Mais moderno e geralmente melhor para UX.

---

## HTML

```html
<header class="header">
  Meu Site
</header>

<main>
  <p>Conteúdo...</p>
</main>
```

---

## CSS

```css
body {
  margin: 0;
}

.header {
  position: sticky;

  top: 0;

  background: #222;
  color: white;

  padding: 20px;

  z-index: 1000;
}
```

---

# Diferença entre `fixed` e `sticky`

## `fixed`

* sempre fixo;
* sai do fluxo;
* cobre conteúdo se não houver espaçamento.

---

## `sticky`

* participa do fluxo;
* só “gruda” ao alcançar o topo;
* costuma gerar menos problemas responsivos.

---

# Qual usar?

## Use `fixed` quando:

* o menu precisa ficar sempre visível;
* houver barra de navegação principal;
* existir botão flutuante.

---

## Use `sticky` quando:

* quiser comportamento mais natural;
* quiser evitar sobreposição;
* quiser melhor adaptação responsiva.

---

# Conclusão

O `position` é essencial para construir interfaces modernas.

Os principais pontos são:

* `static` → comportamento padrão;
* `relative` → desloca mantendo espaço;
* `absolute` → posicionamento livre;
* `fixed` → elemento fixo na tela;
* `sticky` → elemento acompanha rolagem parcialmente.

Use `position` com cuidado.

Layouts modernos devem usar:

* Flexbox;
* CSS Grid;

e utilizar `position` apenas para:

* sobreposição;
* elementos flutuantes;
* menus;
* badges;
* cabeçalhos;
* componentes especiais.
