# Como Criar Formulários HTML

Formulários HTML são usados para coletar informações dos usuários em uma página da web. Eles podem conter diversos tipos de campos como texto, e-mail, senha, caixas de seleção, botões e muito mais.

---

## Estrutura do Formulário

Um formulário HTML é criado usando a tag `<form>`. Essa tag define a área da página que agrupa os campos de entrada do usuário. Nela também são definidos atributos como:

- **`action`**: indica para onde os dados do formulário devem ser enviados (como um script PHP ou uma API).
- **`method`**: define o método de envio dos dados, geralmente `GET` ou `POST`.

---

## Tipos de Campos

Dentro de um formulário, é possível adicionar diferentes tipos de campos, dependendo da informação que se deseja obter:

- **Campo de texto**: usado para entrada de nomes, títulos e outros dados curtos.
- **Campo de e-mail**: permite ao usuário inserir um endereço de e-mail com validação básica.
- **Campo de senha**: oculta os caracteres digitados, ideal para logins.
- **Área de texto**: usada para mensagens ou textos longos.
- **Botão de envio**: envia o formulário quando clicado.
- **Caixas de seleção (checkbox)**: permitem múltiplas escolhas.
- **Botões de opção (radio)**: permitem apenas uma escolha entre várias opções.
- **Listas suspensas (select)**: oferecem uma lista de opções a serem escolhidas.

---

## Atributos Comuns

Alguns atributos são frequentemente utilizados nos campos de formulários:

- **`name`**: identifica o campo e permite acessar seu valor.
- **`id`**: é usado junto com o `<label>` para melhorar a acessibilidade.
- **`required`**: torna o preenchimento obrigatório.
- **`placeholder`**: mostra um texto de exemplo dentro do campo antes do usuário digitar.

---

## Finalidade

Os formulários são essenciais para interações como:

- Cadastros e logins  
- Envio de mensagens e feedbacks  
- Pesquisas e votações  
- Compras online (formulário de pagamento)

Eles podem ser usados sozinhos, com linguagens como **PHP**, **JavaScript** ou integrados a **APIs**.

---

## Exemplo Prático de Formulário HTML

```html
<form action="#" method="POST">
  <h2>Cadastro de Usuário</h2>

  <!-- Campo de texto -->
  <label for="nome">Nome:</label><br>
  <input type="text" id="nome" name="nome" required><br><br>

  <!-- Campo de e-mail -->
  <label for="email">E-mail:</label><br>
  <input type="email" id="email" name="email" required><br><br>

  <!-- Campo de senha -->
  <label for="senha">Senha:</label><br>
  <input type="password" id="senha" name="senha" required><br><br>

  <!-- Checkbox (múltipla escolha) -->
  <label>Interesses:</label><br>
  <input type="checkbox" name="interesse" value="html"> HTML<br>
  <input type="checkbox" name="interesse" value="css"> CSS<br>
  <input type="checkbox" name="interesse" value="js"> JavaScript<br><br>

  <!-- Radio (escolha única) -->
  <label>Sexo:</label><br>
  <input type="radio" name="sexo" value="M"> Masculino<br>
  <input type="radio" name="sexo" value="F"> Feminino<br><br>

  <!-- Select (lista suspensa) -->
  <label for="curso">Curso desejado:</label><br>
  <select id="curso" name="curso">
    <option value="html">HTML</option>
    <option value="css">CSS</option>
    <option value="js">JavaScript</option>
  </select><br><br>

  <!-- Textarea -->
  <label for="mensagem">Mensagem:</label><br>
  <textarea id="mensagem" name="mensagem" rows="4" cols="40"></textarea><br><br>

  <!-- Botão de envio -->
  <button type="submit">Enviar</button>
</form>
````

---

## Explicação dos Principais Atributos de Campos em Formulários HTML

Ao criar campos em formulários HTML (como `<input>`, `<textarea>`, `<select>`), podemos usar diversos atributos para controlar o comportamento e a validação desses campos.

---

### `name`

* **Descrição**: Identifica o nome do campo que será usado no envio dos dados.
* **Importância**: Fundamental para que os dados possam ser recuperados no backend (como PHP, Node.js, etc.).
* **Exemplo**:

  ```html
  <input type="text" name="usuario">
  ```

---

### `id`

* **Descrição**: Identifica o elemento unicamente na página.
* **Importância**: Usado para associar o campo a uma `<label>`, ou ser manipulado por CSS/JavaScript.
* **Exemplo**:

  ```html
  <input id="email">
  ```

---

### `type`

* **Descrição**: Define o tipo de dado que o campo deve receber.
* **Exemplos mais comuns**:

  * `text`, `email`, `password`, `checkbox`, `radio`, `number`, `submit`
* **Exemplo**:

  ```html
  <input type="email" name="email">
  ```

---

### `placeholder`

* **Descrição**: Texto de exemplo que aparece dentro do campo antes do usuário digitar algo.
* **Exemplo**:

  ```html
  <input type="text" placeholder="Digite seu nome">
  ```

---

### `required`

* **Descrição**: Torna o campo obrigatório.
* **Exemplo**:

  ```html
  <input type="text" required>
  ```

---

### `value`

* **Descrição**: Define um valor inicial para o campo.
* **Exemplo**:

  ```html
  <input type="text" value="João da Silva">
  ```

---

### `maxlength` e `minlength`

* **Descrição**: Limitam a quantidade de caracteres permitidos.
* **Exemplo**:

  ```html
  <input type="text" maxlength="20" minlength="3">
  ```

---

### `checked`

* **Descrição**: Marca um `checkbox` ou `radio` como selecionado por padrão.
* **Exemplo**:

  ```html
  <input type="radio" name="sexo" value="M" checked> Masculino
  ```

---

### `selected`

* **Descrição**: Marca uma `<option>` como selecionada por padrão.
* **Exemplo**:

  ```html
  <option value="html" selected>HTML</option>
  ```

---

### `disabled`

* **Descrição**: Desativa o campo (não interativo e não enviado).
* **Exemplo**:

  ```html
  <input type="text" disabled>
  ```

---

### `readonly`

* **Descrição**: O campo pode ser lido, mas não editado.
* **Diferente de** `disabled` pois ainda é enviado no formulário.
* **Exemplo**:

  ```html
  <input type="text" value="Somente leitura" readonly>
  ```

---

### `multiple`

* **Descrição**: Permite múltiplas seleções em `<select>` ou `<input type="file">`.
* **Exemplo**:

  ```html
  <select multiple>
    <option value="html">HTML</option>
    <option value="css">CSS</option>
  </select>
  ```

---

## Dica Final

Usar corretamente os atributos dos campos melhora:

* A **experiência do usuário**
* A **acessibilidade da página**
* A **validação** dos dados antes de enviá-los ao servidor

---
