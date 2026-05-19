
# Containers Semânticos no HTML


---

# O que é HTML Semântico

HTML semântico significa:

"usar a tag correta para o conteúdo correto"

Ou seja:

* usar `header` para cabeçalhos
* usar `nav` para navegação
* usar `main` para conteúdo principal
* usar `footer` para rodapé

Em vez de fazer tudo usando apenas `div`.

---

# O problema de usar div para tudo

Muitos iniciantes fazem isso:

```html
<div class="header"></div>
<div class="menu"></div>
<div class="content"></div>
<div class="footer"></div>
```

Isso funciona visualmente.

Mas estruturalmente é ruim.

O navegador não entende:

* o que é menu
* o que é conteúdo principal
* o que é rodapé
* o que é navegação

Para o navegador:

Tudo é apenas uma caixa genérica.

---

# Por que HTML semântico é importante

## 1. Organização

O código fica mais limpo.

---

## 2. Leitura mais fácil

Outros desenvolvedores entendem a estrutura rapidamente.

---

## 3. SEO

Motores de busca entendem melhor sua página.

Isso ajuda no ranking do site.

---

## 4. Acessibilidade

Leitores de tela conseguem identificar:

* menus
* conteúdo principal
* artigos
* navegação
* rodapés

Isso melhora acessibilidade para usuários com deficiência.

---

## 5. Padrão profissional

Projetos profissionais usam HTML semântico.

---

# Estrutura básica semântica

```html
<body>

    <header>
        Cabeçalho
    </header>

    <nav>
        Navegação
    </nav>

    <main>
        Conteúdo principal
    </main>

    <footer>
        Rodapé
    </footer>

</body>
```

---

# header

## O que é

Representa o cabeçalho da página ou de uma seção.

---

## Quando usar

Use para:

* logo
* título
* menu
* barra superior
* introdução

---

## Exemplo

```html
<header>

    <img src="logo.png" alt="Logo">

    <h1>Meu Site</h1>

</header>
```

---

# nav

## O que é

Representa uma área de navegação.

---

## Quando usar

Use para:

* menus principais
* links de navegação
* menu lateral
* paginação

---

## Exemplo

```html
<nav>

    <a href="#">Home</a>
    <a href="#">Produtos</a>
    <a href="#">Contato</a>

</nav>
```

---

# main

## O que é

Representa o conteúdo principal da página.

---

## Regras importantes

A página deve possuir apenas:

* UM `main`

---

## Quando usar

Use para:

* conteúdo principal
* artigos
* seções centrais
* conteúdo único da página

---

## Exemplo

```html
<main>

    <h1>Produtos</h1>

    <p>Lista de produtos...</p>

</main>
```

---

# footer

## O que é

Representa o rodapé.

---

## Quando usar

Use para:

* copyright
* links finais
* redes sociais
* contatos
* informações legais

---

## Exemplo

```html
<footer>

    <p>© 2026 Meu Site</p>

</footer>
```

---

# section

## O que é

Representa uma seção temática.

---

## Quando usar

Use quando existir:

* um assunto específico
* um agrupamento de conteúdo
* um bloco com sentido próprio

---

## Exemplo

```html
<section>

    <h2>Sobre Nós</h2>

    <p>Lorem ipsum...</p>

</section>
```

---

# article

## O que é

Representa um conteúdo independente.

---

## Quando usar

Use para:

* posts
* notícias
* cards de blog
* artigos
* comentários
* conteúdos independentes

---

## Exemplo

```html
<article>

    <h2>Título da notícia</h2>

    <p>Conteúdo...</p>

</article>
```

---

# aside

## O que é

Representa conteúdo secundário.

---

## Quando usar

Use para:

* sidebar
* anúncios
* widgets
* conteúdo relacionado
* menus laterais

---

## Exemplo

```html
<aside>

    <h2>Posts relacionados</h2>

</aside>
```

---

# div

## O que é

A `div` é uma caixa genérica.

Ela NÃO possui significado semântico.

---

# Quando usar div

Use div quando:

* nenhuma tag semântica fizer sentido
* for apenas uma caixa de estilização
* precisar agrupar elementos visualmente

---

## Exemplo

```html
<div class="card">

    <img src="produto.png">

    <h2>Produto</h2>

</div>
```

---

# O erro comum

## Fazer tudo com div

Errado:

```html
<div class="header"></div>
<div class="menu"></div>
<div class="main"></div>
<div class="footer"></div>
```

---

## Forma correta

```html
<header></header>
<nav></nav>
<main></main>
<footer></footer>
```

---

# Estrutura profissional moderna

## Exemplo completo

```html
<body>

    <header>

        <h1>Wanderland</h1>

        <nav>
            <a href="#">Home</a>
            <a href="#">About</a>
            <a href="#">Contact</a>
        </nav>

    </header>


    <main>

        <section>

            <h2>Banner Principal</h2>

        </section>


        <section>

            <h2>Pacotes</h2>


            <article>
                <h3>Pacote 1</h3>
            </article>

            <article>
                <h3>Pacote 2</h3>
            </article>

        </section>


        <aside>
            Publicidade
        </aside>

    </main>


    <footer>
        Rodapé
    </footer>

</body>
```

---

# Como pensar semanticamente

Pergunte:

## "Qual o significado deste conteúdo?"

Se for:

* navegação → `nav`
* conteúdo principal → `main`
* rodapé → `footer`
* seção temática → `section`
* conteúdo independente → `article`
* conteúdo secundário → `aside`

Se nenhuma tag fizer sentido:

* use `div`

---

# Benefícios reais

## Código mais organizado

```html
<header>
<nav>
<main>
<section>
<footer>
```

Muito mais fácil de entender.

---

## Melhor manutenção

Projetos grandes ficam mais organizados.

---

## Melhor SEO

Google entende melhor a página.

---

## Melhor acessibilidade

Leitores de tela navegam corretamente.

---

# Resumo Visual

| Tag     | Função                |
| ------- | --------------------- |
| header  | Cabeçalho             |
| nav     | Navegação             |
| main    | Conteúdo principal    |
| section | Seção temática        |
| article | Conteúdo independente |
| aside   | Conteúdo secundário   |
| footer  | Rodapé                |
| div     | Caixa genérica        |

---

# Regra profissional importante

## Primeiro pense:

"Existe uma tag semântica para isso?"

Se existir:

* use ela

Se não existir:

* use `div`

---

# Conclusão

HTML semântico não é apenas estética.

Ele melhora:

* organização
* acessibilidade
* SEO
* manutenção
* legibilidade
* profissionalismo

A `div` ainda é importante.

Mas ela deve ser usada:

* quando necessário
* e não para construir tudo.
