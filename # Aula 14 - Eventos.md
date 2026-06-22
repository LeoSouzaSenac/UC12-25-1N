# Eventos em JavaScript

Eventos são o mecanismo que permite ao JavaScript reagir ao usuário.

A ideia central é simples:

> "Quando algo acontecer, execute uma função"

---

# addEventListener — ponto de partida

Tudo começa aqui:

```js
elemento.addEventListener("evento", funcao);
````

Exemplo base:

```js
const botao = document.querySelector("button");

botao.addEventListener("click", () => {
    console.log("clicou");
});
```

Tradução:

* "click" → o que você quer escutar
* função → o que você quer fazer

---

# Eventos de mouse

## click (clique simples)

```js
const btn = document.querySelector("#btn");

btn.addEventListener("click", () => {
    console.log("Clique detectado");
});
```

Use quando:

* botão
* interação básica

---

## dblclick (duplo clique)

```js
btn.addEventListener("dblclick", () => {
    console.log("Duplo clique");
});
```

Observe:

* só dispara com dois cliques rápidos

---

## mouseenter (mouse entrou no elemento)

```js
const caixa = document.querySelector(".box");

caixa.addEventListener("mouseenter", () => {
    console.log("Entrou na caixa");
});
```

Use para:

* efeitos de hover mais controlados que CSS

---

## mouseleave (mouse saiu)

```js
caixa.addEventListener("mouseleave", () => {
    console.log("Saiu da caixa");
});
```

---

## mousemove (movimento do mouse)

```js
caixa.addEventListener("mousemove", (event) => {
    console.log("X:", event.clientX, "Y:", event.clientY);
});
```

Observe:

* dispara MUITAS vezes por segundo
* pode impactar performance

---

# Eventos de teclado

Esses normalmente ficam no document.

## keydown (quando a tecla é pressionada)

```js
document.addEventListener("keydown", (event) => {
    console.log("Tecla:", event.key);
});
```

Observe:

* dispara mesmo segurando a tecla

---

## keyup (quando solta a tecla)

```js
document.addEventListener("keyup", (event) => {
    console.log("Soltou:", event.key);
});
```

Diferença importante:

* keydown → início da ação
* keyup → final da ação

---

## Comparando keydown vs keyup

```js
document.addEventListener("keydown", () => {
    console.log("pressionou");
});

document.addEventListener("keyup", () => {
    console.log("soltou");
});
```

Isso é muito usado em:

* jogos
* controles de movimento

---

# Eventos de formulário

## submit (envio de formulário)

```js
const form = document.querySelector("form");

form.addEventListener("submit", (event) => {
    event.preventDefault();
    console.log("Form enviado");
});
```

Sem preventDefault:

* página recarrega

Com preventDefault:

* você controla o envio

---

## input (digitação em tempo real)

```js
const input = document.querySelector("input");

input.addEventListener("input", (event) => {
    console.log("Valor:", event.target.value);
});
```

Use para:

* validação
* busca em tempo real

---

## change (quando o valor muda e perde foco)

```js
input.addEventListener("change", (event) => {
    console.log("Mudou:", event.target.value);
});
```

Diferença importante:

* input → a cada tecla
* change → só quando termina (perde foco)

---

## focus (quando entra no campo)

```js
input.addEventListener("focus", () => {
    console.log("Campo ativo");
});
```

---

## blur (quando sai do campo)

```js
input.addEventListener("blur", () => {
    console.log("Saiu do campo");
});
```

Muito usado para:

* validação de formulário

---

# Eventos de carregamento

## DOMContentLoaded

```js
document.addEventListener("DOMContentLoaded", () => {
    console.log("HTML carregado");
});
```

Use quando:

* precisa garantir que os elementos existem antes de usar

---

## load

```js
window.addEventListener("load", () => {
    console.log("Tudo carregado (imagens, etc)");
});
```

Diferença:

* DOMContentLoaded → só HTML
* load → tudo (mais lento)

---

# Objeto event (aplicado nos exemplos)

Sempre que um evento acontece (clique, tecla, envio de formulário, etc.), o navegador cria automaticamente um objeto com todas as informações sobre esse evento.

Esse objeto é o event.
> Evento = “algo aconteceu”
> event = “detalhes do que aconteceu”

Esse event dentro da função é criado automaticamente pelo navegador. Você não cria ele.
Você só recebe e usa.

Sempre que houver evento, use:

```js
elemento.addEventListener("click", (event) => {
    console.log(event);
});
```

Agora exemplos práticos reais:

---

## Descobrir qual elemento foi clicado

```js
document.addEventListener("click", (event) => {
    console.log(event.target);
});
```

---

## Saber qual tecla foi pressionada

```js
document.addEventListener("keydown", (event) => {
    console.log(event.key);
});
```

---

## Posição do clique

```js
document.addEventListener("click", (event) => {
    console.log(event.clientX, event.clientY);
});
```

---

# Exercício guiado (importante)

Crie isso no HTML:

```html
<input type="text" id="nome">
<button id="btn">Enviar</button>
```

Agora implemente:

```js
const input = document.getElementById("nome");
const btn = document.getElementById("btn");

input.addEventListener("input", (event) => {
    console.log("Digitando:", event.target.value);
});

btn.addEventListener("click", () => {
    console.log("Botão clicado");
});
```



