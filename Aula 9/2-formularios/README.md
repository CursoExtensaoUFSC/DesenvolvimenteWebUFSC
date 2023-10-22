# Construindouma Banking App Parte 2: Construindo um Login e um formulário de Registro

## Quiz pré-lição

[Quiz pré-lição](https://ashy-river-0debb7803.1.azurestaticapps.net/quiz/43)

### Introdução

Em quase todos os aplicativos da web modernos, você pode criar uma conta para ter seu próprio espaço privado. Como vários usuários podem acessar um aplicativo da web ao mesmo tempo, você precisa de um mecanismo para armazenar os dados pessoais de cada usuário separadamente e selecionar quais informações exibir. Nós não vamos abordar como manejar [identidade de usuário com segurança](https://en.wikipedia.org/wiki/Authentication) pois é um tópico extenso por si póprio, mas vamos garantir que cada usuário será capaz de criar uma conta(ou mais) contas bancárias em nosso app.

Nesta parte usaremos formulários HTML para adicionar login e registro ao nosso aplicativo web. Veremos como enviar os dados para uma API de servidor de forma programática e, por fim, como definir regras básicas de validação para entradas do usuário.

### Pré-requisitos

Você precisa ter completado [HTML templates e rotas](../1-template-route/README.md) de nosso web app para essa lição. Precisa ter instalado o [Node.js](https://nodejs.org) e [rodar o servidor API](../api/README.md) localmente para podermos enviar os dados para criar as contas.

**Tomando nota**
Você deve ter dois terminais rodando ao mesmo tempo como listado abaixo.
1. Para o código principal do nosso bank app que construimos na lição de [HTML templates e rotas](../1-template-route/README.md).
2. Para o [servidor da API](../api/README.md) que configuramos logo acima.

Você precisa de dois servidores em funcionamento para prosseguir com o restante da lição. Eles estão escutando em portas diferentes (porta `3000` e porta `5000`), então tudo deve funcionar bem.

Você pode verificar se o servidor está rodando de forma correnta executando o comando abaixo no terminal:

```sh
curl http://localhost:5000/api
# -> should return "Bank API v1.0.0" as a result
```

---

## Formulários e Controles

O elemento `<form>` encapsula uma seção de um documento HTML onde o usuário pode colocar uma entrada e enviar um dado atráves de controles interativos. Existem uma variedade em tipos de controles de interface de usuário (UI) que podem ser usados dentro de um formulário, sendo os mais comuns os elementos `<input>` e `<button>`.

Há diversos [tipos](https://developer.mozilla.org/docs/Web/HTML/Element/input) diferentes de `<input>`, por exemplo para criar um campot onde o usuário pode entrar com um "nome de usuário" você pode usar:

```html
<input id="username" name="username" type="text">
```

O atributo `name` será usado como nome da propriedade quando os dados do formulário forem enviados. O atributo `id` é usado para associar um `<label>` ao controle do formulário.

> Dê uma olhada em uma lista completa de [tipos de `<input>`](https://developer.mozilla.org/docs/Web/HTML/Element/input) e [outros controles de formulário](https://developer.mozilla.org/docs/Learn/Forms/Other_form_controls) para ter uma ideia de todos os elementos nativos de UI você pode usar quando está construindo sua interface.

✅ Note que `<input>` é um [elemento vazio](https://developer.mozilla.org/docs/Glossary/Empty_element) que no qual você não deve adicionar uma tag de fechamento. Você pode, entretanto, usar a notação auto-fechada `<input/>`, mas isso não é necessário.

O elemento `<button>` dentro de um formulário é um pouco especial. Se você não especificar seu atributo `type`, ele vai automaticamente enviar os dados do formulário para o servidor quando pressionado. Aqui os possíveis valores de `type`:

- `submit`: O comportamento padrão dentro de um `<form>`, o botão vai acionar a ação de envio do formulário.
- `reset`: O botão coloca todos os controles de formulário para seus valores iniciais.
- `button`: Não coloca no comportamento padrão quando o botão é pressionado. Você pode então colocar ações customizaveis nele, usando JavaScript.

### Tarefa

Vamos começar adicionando um formulário no nosso template `login`. Nós vamos precisar de um campo *nome de usuário* e um de *login*.

```html
<template id="login">
  <h1>Bank App</h1>
  <section>
    <h2>Login</h2>
    <form id="loginForm">
      <label for="username">Nome de usuário</label>
      <input id="username" name="user" type="text">
      <button>Login</button>
    </form>
  </section>
</template>
```

Se você olhar com atenção, pode notar que também adicionamos um elemento `<label>` aqui. Elementos `<label>` são usados para adicionar um nome para os controles de interface, assim como nosso campo de "nome de usuário". Labels são importantes para a leitura de nossos formulários, mas também vem com benefícios adicionais:

- Ao associar uma label a um controle de formulário, ajuda os usuários que usam tecnologias assistivas (como um leitor de tela) a entender quais dados devem fornecer.
- Você pode clicar na label para focar diretamente na entrada associada, facilitando o acesso em dispositivos baseados em tela sensível ao toque.

> [Acessibilidade](https://developer.mozilla.org/docs/Learn/Accessibility/What_is_accessibility) na internet é um tópico importante que frequentemente é esquecido. Graças a [semântica dos elementos HTML](https://developer.mozilla.org/docs/Learn/Accessibility/HTML) não é difícil de criar conteúdos acesessíveis se você usar eles adequadamente. Você pode [ler mais sobre acessibilidade](https://developer.mozilla.org/docs/Web/Accessibility) para evitar erros comuns e se tornar um desenvolvedor responsavel.

Now we'll add a second form for the registration, just below the previous one:
Agora nós vamos adicionar um segundo `<form>` para o registro de usuário, logo abaixo do anterior.

```html
<hr/>
<h2>Registro</h2>
<form id="registerForm">
  <label for="user">Nome de usuário</label>
  <input id="user" name="user" type="text">
  <label for="currency">Moeda</label>
  <input id="currency" name="currency" type="text" value="$">
  <label for="description">Descrição</label>
  <input id="description" name="description" type="text">
  <label for="balance">Balanço atual</label>
  <input id="balance" name="balance" type="number" value="0">
  <button>Registrar</button>
</form>
```

Usando o atributo `value` nós podemos definir uma entrada padrão para o dado campo.
Note também que a entrada para `balance` tem o tipo `number`. Ele parece diferente das outras entradas? Tente interagir com ele.

✅ Você consegue navegar e interagir com os formulários usando apenas um teclado? Como você faria isso?

## Enviando dados para o servidor

Agora que temos uma UI funcional, o próximo passo é enviar os dados para o nosso servidor. Vamos fazer um teste rápido usando nosso código atual: o que acontece se você clicar no botão *Login* ou *Registrar*?

Você notou a mudança na seção URL do seu navegador?

A ação padrão de um `<form>` é de enviar o formulário para o atual servidor da URL usando o [Método GET](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html#sec9.3), anexando os dados do formulário diretamente ao URL. Este método tem algumas deficiências:

- Os dados enviados são de tamanho muito limitado (cerca de 2.000 caracteres)
- Os dados são diretamente visíveis na URL (não é bom para senhas)
- Não funciona com upload de arquivos

É por isso que você pode mudar isso usando o [Método POST](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html#sec9.5) que envia os dados do formulário para o servidor em um corpo de uma requisição HTTP, sem as limitações faladas anteriormente.

> Enquanto o POST é o método mais comumente usado para enviar dados adiante, [em alguns cenários especificos](https://www.w3.org/2001/tag/doc/whenToUseGet.html) se prefere usar o método GET, enquanto implementa um campo de busca por exemplo.

### Tarefa

Adicionar as propriedades `action` e `method` no formulário de registro:

```html
<form id="registerForm" action="//localhost:5000/api/accounts" method="POST">
```

Agora tente registrar uma nova conta com seu nome. Depois de clicar no botão *Registrar* você deve ver algo como isso: After clicking on the *Register* button you should see something like this:

![A browser window at the address localhost:5000/api/accounts, showing a JSON string with user data](https://github.com/CursoExtensaoUFSC/DesenvolvimenteWebUFSC/blob/main/Aula%209/images/form-post.png)

Se tudo ocorrer bem, o servidor deve te responder sua requisição com uma resposta [JSON](https://www.json.org/json-en.html) contendo os dados da conta que foi criada.

✅ Tente se registrar novamente com o mesmo nome. O que acontece?

## Enviando dados sem recarregar a página

Como você provavelmente notou, há um pequeno problema com a abordagem que acabamos de usar: ao enviar o formulário, saímos do nosso aplicativo e o navegador redireciona para a URL do servidor. Estamos tentando evitar todas as atualizações de páginas com nosso aplicativo da web, já que estamos fazendo uma [Aplicação de Página Única (SPA)](https://en.wikipedia.org/wiki/Single-page_application).

Para enviar os dados do formulário ao servidor sem forçar o recarregamento da página, temos que usar o código JavaScript. Em vez de colocar uma URL na propriedade `action` de um elemento `<form>`, você pode usar qualquer código JavaScript prefixado pela string `javascript:` para executar uma ação personalizada. Usar isso também significa que você terá que implementar algumas tarefas que antes eram feitas automaticamente pelo navegador:

- Recuperar os dados do formulário
- Converta e codifique os dados do formulário em um formato adequado
- Crie a solicitação HTTP e envie-a para o servidor

### Tarefa

Substituir o `action` no formulário de registro por:

```html
<form id="registerForm" action="javascript:register()">
```

Abra o `app.js` e adicione uma nova função de nome `register`:

```js
function register() {
  const registerForm = document.getElementById('registerForm');
  const formData = new FormData(registerForm);
  const data = Object.fromEntries(formData);
  const jsonData = JSON.stringify(data);
}
```

Aqui nós estamos obtendo o elemento `form` usando `getElementById()` e usando [`FormData`](https://developer.mozilla.org/docs/Web/API/FormData) para ajudar na extração dos valores que estão nos controladores de formulários em uma configuração de pares de chave/valor. Então nós convertemos os dados em um objeto regular usando [`Object.fromEntries()`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object/fromEntries) e finalmente serializando os dados para um [JSON](https://www.json.org/json-en.html), formato comumente usado para trocas de dados na web.

Os dados agora estão prontos para serem enviados ao servidor. Crie uma nova função chamada `createAccount`:

```js
async function createAccount(account) {
  try {
    const response = await fetch('//localhost:5000/api/accounts', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: account
    });
    return await response.json();
  } catch (error) {
    return { error: error.message || 'Unknown error' };
  }
}
```

O que essa função está fazendo? Primeiro, note a palavra-chave `async`. Isso significa que a função contêm um código que será executado [**de forma assíncrona**](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/async_function). Quando usado junto com a palavra-chave `await`, permite aguardar a execução do código assíncrono - como esperar pela resposta do servidor - antes de continuar.

Aqui está um vídeo rápido sobre o uso de `async/await`:

[![Async and Await for managing promises](https://img.youtube.com/vi/YwmlRkrxvkk/0.jpg)](https://youtube.com/watch?v=YwmlRkrxvkk "Async and Await for managing promises")

> 🎥 Clique na imagem acima para ver um vídeo sobre async/await.

Usamos a API `fetch()` para enviar dados JSON para o servidor. Este método leva 2 parâmetros:

- A URL do servidor, então colocamos novamente `//localhost:5000/api/accounts` aqui.
- As configurações da solicitação. É aí que definimos o método como `POST` e fornecemos o `body` para a solicitação. Como estamos enviando dados JSON para o servidor, também precisamos definir o cabeçalho `Content-Type` como `application/json` para que o servidor saiba como interpretar o conteúdo.

Como o servidor responderá à solicitação com JSON, podemos usar `await response.json()` para analisar o conteúdo JSON e retornar o objeto resultante. Observe que este método é assíncrono, então usamos a palavra-chave `await` aqui antes de retornar para garantir que quaisquer erros durante a análise também sejam detectados.

Agora adicione mais um código à função `register` para chamar `createAccount()`:

```js
const result = await createAccount(jsonData);
```

Como usamos a palavra-chave `await` aqui, precisamos adicionar a palavra-chave `async` antes da função de registro:

```js
async function register() {
```

Finalmente, vamos adicionar alguns logs para verificar o resultado. A função final deverá ficar assim:

```js
async function register() {
  const registerForm = document.getElementById('registerForm');
  const formData = new FormData(registerForm);
  const jsonData = JSON.stringify(Object.fromEntries(formData));
  const result = await createAccount(jsonData);

  if (result.error) {
    return console.log('An error occurred:', result.error);
  }

  console.log('Account created!', result);
}
```

Se você abrir sua [ferramenta de desenvolvedor](https://developer.mozilla.org/docs/Learn/Common_questions/What_are_browser_developer_tools), e tente registrar uma nova conta, você não deverá ver nenhuma alteração na página da web, mas uma mensagem aparecerá no console confirmando que tudo funciona.

✅Você acha que os dados são enviados para o servidor com segurança? E se alguém conseguisse interceptar a solicitação? Você pode ler sobre [HTTPS](https://en.wikipedia.org/wiki/HTTPS) para saber mais sobre comunicação segura de dados.

## Validação de dados

Se você tentar registrar uma nova conta sem definir um nome de usuário primeiro, poderá ver que o servidor retorna um erro com o código de status [400 (Bad Request)](https://developer.mozilla.org/docs/Web/HTTP/Status/400#:~:text=The%20HyperText%20Transfer%20Protocol%20(HTTP,%2C%20or%20deceptive%20request%20routing).).

Antes de enviar os dados para o servidor é uma boa pratica [validar os dados de formulário](https://developer.mozilla.org/docs/Learn/Forms/Form_validation) de antemão quando possível, para ter certeza de enviar uma solicitação válida. Os controles de formulários HTML5 fornecem validação integrada usando vários atributos:

- `required`: o campo precisa ser preenchido, caso contrário o formulário não será enviado.
- `minlength` and `maxlength`: define o máximo e mínimo número de caracteres no campo de texto.
- `min` and `max`: define um máximo e mínimo nos valores em campos de números.
- `type`: define o tipo esperado de dado, como `number`, `email`, `file` ou [outros tipos integrados](https://developer.mozilla.org/docs/Web/HTML/Element/input). Este atributo também pode alterar a renderização visual do controle de formulário.
- `pattern`: permite definir um padrão de [expressão regular](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Regular_Expressions) para testar a entrada se os dados de entrada são validos ou não.

> Dica: você pode personalizar a aparência dos controles do seu formulário dependendo se eles são válidos ou não usando as pseudoclasses CSS `:valid` e `:invalid`.

### Task

Existem 2 campos obrigatórios para criar uma nova conta válida, o nome de usuário e a moeda, sendo os demais campos opcionais. Atualize o HTML do formulário, usando o atributo `required` e o texto no rótulo do campo para isso:

```html
<label for="user">Nome de usuário (required)</label>
<input id="user" name="user" type="text" required>
...
<label for="currency">Moeda (required)</label>
<input id="currency" name="currency" type="text" value="$" required>
```

Embora esta implementação de servidor específica não imponha limites específicos ao comprimento máximo dos campos, é sempre uma boa prática definir limites razoáveis para qualquer entrada de texto do usuário.

Adicione um atributo `maxlength` aos campos de texto:

```html
<input id="user" name="user" type="text" maxlength="20" required>
...
<input id="currency" name="currency" type="text" value="$" maxlength="5" required>
...
<input id="description" name="description" type="text" maxlength="100">
```

Validação como essa realizada *antes* de enviar qualquer dado ao servidor é chamada de validação **client-side**. Mas observe que nem sempre é possível realizar todas as verificações sem enviar os dados. Por exemplo, não podemos verificar aqui se já existe uma conta com o mesmo nome de usuário sem enviar uma solicitação ao servidor. A validação adicional realizada no servidor é chamada de validação **server-side**.

Normalmente, ambos precisam ser implementados e, embora o uso da validação do lado do cliente melhore a experiência do usuário, fornecendo feedback instantâneo ao usuário, a validação do lado do servidor é crucial para garantir que os dados do usuário que você manipula sejam sólidos e seguros.

---

## 🚀 Desafio

Mostrar uma mensagem de erro no HTML se o usuário já existir.

## Quiz pós-lição

[Quiz pós-lição](https://ashy-river-0debb7803.1.azurestaticapps.net/quiz/44)

## Revisão e autoestudo

Os desenvolvedores têm sido muito criativos em seus esforços de construção de formulários, especialmente em relação às estratégias de validação. Aprenda sobre os diferentes fluxos de formulários consultando [CodePen](https://codepen.com); você pode encontrar algumas formas interessantes e inspiradoras?

## Atribuição

# Estilize seu aplicativo

## Instruções

Crie um novo arquivo `styles.css` e adicione um link para ele em seu arquivo `index.html` atual. No arquivo CSS que você acabou de criar, adicione algum estilo para fazer com que a página *Login* e *Dashboard* pareçam bonitas e organizadas. Tente criar um tema de cores para dar ao seu aplicativo uma marca própria.

> Dica: você pode modificar o HTML e adicionar novos elementos e classes se necessário.
> 
## Correção

| Criterios | Exemplar                                                                                                               | Adequado                                                                       | Pode Melhorar                                                                            |
| -------- | ----------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------- |
|          | Todas as páginas parecem limpas e legíveis, com um tema de cores consistente e as diferentes seções se destacando adequadamente. | As páginas são estilizadas, mas sem tema ou com seções não claramente delimitadas. | As páginas não têm estilo, as seções parecem desorganizadas e as informações são difíceis de ler. |
