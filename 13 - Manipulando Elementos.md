# Manipulação de Elementos (DOM)

Se no módulo anterior você aprendeu a **reagir a eventos**,  
agora você vai aprender a **mudar a página**.

Esse é o ponto onde o JavaScript deixa de ser invisível.

---

# O problema que estamos resolvendo

HTML sozinho:
- mostra conteúdo
- não muda sozinho

JavaScript:
- altera conteúdo
- cria elementos
- remove elementos
- modifica atributos

Mas pra fazer isso, você precisa primeiro:

> Encontrar o elemento dentro da página

---

# Selecionando elementos (como "pegar" algo do HTML)

## getElementById (mais direto)

```js
const titulo = document.getElementById("titulo");
````

HTML:

```html
<h1 id="titulo">Olá</h1>
```

Quando usar:

* quando você tem um ID único
* mais rápido e direto

Limitação:

* só funciona com ID

---

## querySelector (o mais importante)

```js
const titulo = document.querySelector("#titulo");
```

Você pode usar qualquer seletor CSS:

```js
document.querySelector(".classe");
document.querySelector("p");
document.querySelector("#id");
```

Quando usar:

* praticamente sempre

Por quê:

* é flexível
* funciona como CSS

---

## querySelectorAll (vários elementos)

```js
const itens = document.querySelectorAll("li");
```

Isso retorna uma lista (NodeList), basicamente um array

Exemplo:

```js
itens.forEach((item) => {
    console.log(item.textContent);
});
```

Use quando:

* precisa trabalhar com vários elementos

---

## getElementsByClassName

```js
const caixas = document.getElementsByClassName("box");
```

Diferença:

* retorna uma coleção "viva" (atualiza sozinha)
* não é um array de verdade

Hoje em dia:

* menos usado que querySelectorAll

---

## getElementsByTagName

```js
const paragrafos = document.getElementsByTagName("p");
```

Seleciona por tipo de tag.

---

# Regra prática (importante ensinar)

Use isso como padrão:

* Um elemento → `querySelector`
* Vários elementos → `querySelectorAll`
* ID específico → `getElementById`

---

# Alterando conteúdo

Agora que você pegou o elemento, você pode mudar ele.

---

## textContent (texto puro)

```js
const titulo = document.querySelector("h1");

titulo.textContent = "Novo título";
```

O que acontece:

* substitui o texto
* ignora HTML

---

## innerHTML (interpreta HTML)

```js
titulo.innerHTML = "<span>Texto com HTML</span>";
```

Agora ele cria HTML dentro do elemento.

Diferença crítica:

* textContent → texto literal
* innerHTML → interpreta HTML

---

## Exemplo comparando

```js
titulo.textContent = "<b>Olá</b>";
```

Mostra na tela:

```
<b>Olá</b>
```

```js
titulo.innerHTML = "<b>Olá</b>";
```

Mostra:
Olá (em negrito)

---

# Pegando conteúdo

```js
const texto = titulo.textContent;
console.log(texto);
```

---

# Manipulando atributos

Atributos são coisas como:

* id
* class
* src
* href

---

## getAttribute

```js
const link = document.querySelector("a");

console.log(link.getAttribute("href"));
```

---

## setAttribute

```js
link.setAttribute("href", "https://google.com");
```

---

## removeAttribute

```js
link.removeAttribute("target");
```

---

# Forma moderna (mais usada na prática)

Em vez de setAttribute:

```js
link.href = "https://google.com";
```

Em vez de getAttribute:

```js
console.log(link.href);
```

Isso é mais simples e mais comum no dia a dia.

---

# Trabalhando com classes

Muito usado para alterar CSS.

---

## classList.add

```js
const box = document.querySelector(".box");

box.classList.add("ativo");
```

---

## classList.remove

```js
box.classList.remove("ativo");
```

---

## classList.toggle

```js
box.classList.toggle("ativo");
```

Isso faz:

* se tem → remove
* se não tem → adiciona

Muito usado com clique.

---

# Exemplo prático (ligando com eventos)

```html
<button id="btn">Ativar</button>
<div id="box"></div>

<script>
const btn = document.getElementById("btn");
const box = document.getElementById("box");

btn.addEventListener("click", () => {
    box.classList.toggle("ativo");
});
</script>
```

Agora você tem:

* evento
* seleção
* manipulação de classe

---

# Criando elementos (começo do dinamismo real)

```js
const novo = document.createElement("p");

novo.textContent = "Novo elemento";

document.body.appendChild(novo);
```

Isso cria um elemento do zero.

---

# Removendo elementos

```js
novo.remove();
```

---


# Exercícios

1. Criar um botão que muda o texto de um título
2. Criar um botão que adiciona/remova uma classe
3. Criar um botão que adiciona um item em uma lista
4. Criar um botão que remove um elemento
5. Criar um input que altera um texto em tempo real
