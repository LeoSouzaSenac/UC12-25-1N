# HTML Básico

# O que é HTML?

HTML significa:

HyperText Markup Language

É a linguagem usada para estruturar páginas web.

Com HTML criamos:
- títulos
- textos
- imagens
- links
- tabelas
- listas
- formulários

HTML NÃO é uma linguagem de programação.

Ele é uma linguagem de marcação.

---

# Estrutura básica do HTML

```html
<!DOCTYPE html>
<html>

<head>
    <title>Meu Site</title>
</head>

<body>

    <h1>Olá Mundo</h1>

</body>

</html>
````

---

# Explicando cada parte

## <!DOCTYPE html>

Informa ao navegador que estamos usando HTML5.

---

## <html>

Elemento principal da página.

Tudo fica dentro dele.

---

## <head>

Contém configurações da página.

---

## <title>

Define o nome da aba do navegador.

---

## <body>

Contém tudo que aparece visualmente na página.

---

# Títulos

HTML possui 6 níveis de títulos.

```html
<h1>Título Principal</h1>
<h2>Título</h2>
<h3>Título</h3>
<h4>Título</h4>
<h5>Título</h5>
<h6>Título</h6>
```

---

# Parágrafos

```html
<p>Este é um parágrafo.</p>
```

---

# Quebra de linha

```html
<br>
```

---

# Linha horizontal

```html
<hr>
```

---

# Comentários

Comentários não aparecem no site.

```html
<!-- Isto é um comentário -->
```

---

# Links

```html
<a href="https://google.com">
    Acessar Google
</a>
```

---

# Imagens

```html
<img src="imagem.jpg">
```

Com tamanho:

```html
<img src="imagem.jpg" width="300">
```

---

# Listas

## Lista não ordenada

```html
<ul>
    <li>Maçã</li>
    <li>Banana</li>
</ul>
```

---

## Lista ordenada

```html
<ol>
    <li>Primeiro</li>
    <li>Segundo</li>
</ol>
```

---

# Divs

A div é usada para organizar conteúdos.

```html
<div>
    <h1>Título</h1>
    <p>Texto</p>
</div>
```

---

# Botões

```html
<button>Clique Aqui</button>
```

---

# Inputs

## Campo de texto

```html
<input type="text">
```

---

## Senha

```html
<input type="password">
```

---

## Número

```html
<input type="number">
```

---

## Email

```html
<input type="email">
```

---

# Formulários

```html
<form>

    <input type="text">

    <button>
        Enviar
    </button>

</form>
```

---

# Labels

```html
<label>Nome</label>
<input type="text">
```

---

# Tabelas

```html
<table border="1">

    <tr>
        <th>Nome</th>
        <th>Idade</th>
    </tr>

    <tr>
        <td>Leonardo</td>
        <td>25</td>
    </tr>

</table>
```

---

# Estrutura completa de exemplo

```html
<!DOCTYPE html>
<html>

<head>
    <title>Meu Primeiro Site</title>
</head>

<body>

    <h1>Meu Site</h1>

    <p>
        Este é meu primeiro site.
    </p>

    <button>
        Clique Aqui
    </button>

</body>

</html>
```

---

# Boas práticas

* usar indentação
* organizar o código
* fechar as tags corretamente
* manter o HTML limpo

---

# Conclusão

HTML é a base da criação de sites.

Todo desenvolvimento web começa com HTML.



