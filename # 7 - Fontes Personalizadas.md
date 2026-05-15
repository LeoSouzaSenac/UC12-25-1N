# 🖋️ Como Usar Fontes Personalizadas no Seu Site

Neste guia, você aprenderá a usar:

1. 📥 Fontes **baixadas** (como `.ttf`, `.otf`, `.woff`)
2. 🌐 Fontes do **Google Fonts**

---

## ✅ 1. Usando uma Fonte Baixada

### 🧩 Passo 1: Baixe a fonte

Baixe a fonte desejada em formato `.ttf`, `.otf`, `.woff` ou `.woff2`.

### 🗂️ Passo 2: Organize a pasta

Crie uma estrutura assim:

```

/meuSite
│
├── index.html
├── /fonts
│     └── minhaFonte.woff2
└── /css
└── style.css

````

### 🧾 Passo 3: Declare a fonte no CSS

No seu `style.css`:

```css
@font-face {
  font-family: 'MinhaFonte';
  src: url('../fonts/minhaFonte.woff2') format('woff2');
  font-weight: normal;
  font-style: normal;
}
````

> 💡 Se a fonte for `.ttf`:

```css
src: url('../fonts/minhaFonte.ttf') format('truetype');
```

### 🎨 Passo 4: Use a fonte no seu site

```css
body {
  font-family: 'MinhaFonte', sans-serif;
}
```

---

## ✅ 2. Usando uma Fonte do Google Fonts

### 🌐 Passo 1: Acesse o site

Acesse: [https://fonts.google.com](https://fonts.google.com)

Escolha a fonte desejada e clique em "Select this style" (ou "Selecionar estilo").

### 🔗 Passo 2: Copie o link `<link>`

Você verá um código como este:

```html
<link href="https://fonts.googleapis.com/css2?family=Poppins&display=swap" rel="stylesheet">
```

Cole esse `<link>` dentro da `<head>` do seu `index.html`:

```html
<head>
  <meta charset="UTF-8">
  <title>Site com Google Fonts</title>
  <link href="https://fonts.googleapis.com/css2?family=Poppins&display=swap" rel="stylesheet">
  <link rel="stylesheet" href="css/style.css">
</head>
```

### 🖌️ Passo 3: Use a fonte no CSS

No seu `style.css`:

```css
body {
  font-family: 'Poppins', sans-serif;
}
```

---

## 📌 Dicas Finais

* Prefira `.woff2` para fontes locais: é mais leve e compatível.
* Evite usar muitas fontes diferentes para manter seu site limpo e rápido.
* Teste em diferentes navegadores para garantir compatibilidade.
