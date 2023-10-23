# Crie um aplicativo bancário, parte 1: modelos HTML e rotas em um aplicativo da web

## Quiz Pré-Lição

[Quiz pré-lição](https://ashy-river-0debb7803.1.azurestaticapps.net/quiz/41)

### Introdução

Desde o advento do JavaScript nos navegadores, os sites estão se tornando mais interativos e complexos do que nunca. As tecnologias da Web agora são comumente usadas para criar aplicativos totalmente funcionais que são executados diretamente em um navegador que chamamos de aplicativos da Web. Como os aplicativos da Web são altamente interativos, os usuários não querem esperar o recarregamento completo da página sempre que uma ação é executada. É por isso que o JavaScript é usado para atualizar o HTML diretamente usando o DOM, para fornecer uma experiência de usuário mais tranquila.

Nesta lição, apresentaremos as bases para criar um aplicativo web bancário, usando modelos HTML para criar várias telas que podem ser exibidas e atualizadas sem a necessidade de recarregar a página HTML inteira.

### Pré-requisitos

Você precisa de um servidor web local para testar o aplicativo web que construiremos nesta lição. Se não tiver um, você pode instalar o [Node.js](https://nodejs.org) e usar o comando `npx lite-server` da pasta do seu projeto. Ele criará um servidor web local e abrirá seu aplicativo em um navegador.

### Preparação

ONo seu computador, crie uma pasta chamada `banco` com um arquivo chamado `index.html` dentro dela. Começaremos com este padrão HTML chamado [boilerplate](https://en.wikipedia.org/wiki/Boilerplate_code):

```html
<!DOCTYPE html>
<html lang="pt-br">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bank App</title>
  </head>
  <body>
    <!-- aqui que iremos trabalhar! -->
  </body>
</html>
```

---

## Templates de HTML

Se você deseja criar múltiplas telas para uma página web, uma solução seria criar um arquivo HTML para cada tela que deseja exibir. No entanto, esta solução traz alguns inconvenientes:

- Você precisa recarregar todo o HTML ao mudar de tela, o que pode ser lento.
- É difícil compartilhar dados entre as diferentes telas.

Outra abordagem é ter apenas um arquivo HTML e definir vários [HTML templates](https://developer.mozilla.org/docs/Web/HTML/Element/template) usando os elementos `<template>`. Um template é um bloco HTML reutilizável que não é exibido pelo navegador e precisa ser instanciado em tempo de execução usando JavaScript.

### Tarefa

Criaremos um aplicativo bancário com duas telas: a página de login e o painel. Primeiro, vamos adicionar no corpo HTML um elemento placeholder que usaremos para instanciar as diferentes telas do nosso aplicativo:

```html
<div id="app">Loading...</div>
```

Estamos dando a ele um `id` para facilitar a localização posterior com JavaScript.

> Dica: como o conteúdo deste elemento será substituído, podemos colocar uma mensagem ou indicador de carregamento que será mostrado enquanto o aplicativo estiver carregando.

A seguir, vamos adicionar abaixo o modelo HTML para a página de login. Por enquanto colocaremos ali apenas um título e uma seção contendo um link que utilizaremos para realizar a navegação.

```html
<template id="login">
  <h1>Bank App</h1>
  <section>
    <a href="/dashboard">Login</a>
  </section>
</template>
```

Em seguida, adicionaremos outro modelo HTML para a página do painel. Esta página conterá diferentes seções:

- Um cabeçalho com um título e um link de logout
- O saldo atual da conta bancária
- Uma lista de transações, exibida em uma tabela

```html
<template id="dashboard">
  <header>
    <h1>Bank App</h1>
    <a href="/login">Logout</a>
  </header>
  <section>
    Balanço: 100$
  </section>
  <section>
    <h2>Transações</h2>
    <table>
      <thead>
        <tr>
          <th>Data</th>
          <th>Objeto</th>
          <th>Quantidade</th>
        </tr>
      </thead>
      <tbody></tbody>
    </table>
  </section>
</template>
```

> Dica: ao criar modelos HTML, se você quiser ver como ficará, você pode comentar as linhas `<template>` e `</template>` colocando-as entre `<!-- -->` .

✅ Por que você acha que usamos atributos `id` nos modelos? Poderíamos usar algo mais como classes?

## Mostrando templates com JavaScript

Se você tentar seu arquivo HTML atual em um navegador, verá que ele trava exibindo `Loading...`. Isso ocorre porque precisamos adicionar algum código JavaScript para instanciar e exibir os modelos HTML.

A instanciação de um modelo geralmente é feita em 3 etapas:

1. Recupere o elemento template no DOM, por exemplo usando [`document.getElementById`](https://developer.mozilla.org/docs/Web/API/Document/getElementById).
2. Clone o elemento do modelo, usando [`cloneNode`](https://developer.mozilla.org/docs/Web/API/Node/cloneNode).
3. Anexe-o ao DOM sob um elemento visível, por exemplo usando [`appendChild`](https://developer.mozilla.org/docs/Web/API/Node/appendChild).

✅ Por que precisamos clonar o modelo antes de anexá-lo ao DOM? O que você acha que aconteceria se pulássemos esta etapa?

### Tarefa

Crie um novo arquivo chamado `app.js` na pasta do seu projeto e importe esse arquivo na seção `<head>` do seu HTML:

```html
<script src="app.js" defer></script>
```

Agora em `app.js`, criaremos uma nova função `updateRoute`:

```js
function updateRoute(templateId) {
  const template = document.getElementById(templateId);
  const view = template.content.cloneNode(true);
  const app = document.getElementById('app');
  app.innerHTML = '';
  app.appendChild(view);
}
```

O que fazemos aqui são exatamente as 3 etapas descritas acima. Instanciamos o modelo com o id `templateId` e colocamos seu conteúdo clonado em nosso espaço reservado para aplicativo. Observe que precisamos usar `cloneNode(true)` para copiar toda a subárvore do modelo.

Agora chame esta função com um dos modelos e veja o resultado.

```js
updateRoute('login');
```

✅ Qual é o propósito deste código `app.innerHTML = '';`? O que acontece sem isso?

## Criando Rotas

Ao falar sobre um aplicativo web, chamamos de *Routing* a intenção de mapear **URLs** para telas específicas que devem ser exibidas. Em um site com vários arquivos HTML, isso é feito automaticamente, pois os caminhos dos arquivos são refletidos na URL. Por exemplo, com estes arquivos na pasta do seu projeto:

```
mywebsite/index.html
mywebsite/login.html
mywebsite/admin/index.html
```

Se você criar um servidor web com `mywebsite` como raiz, o mapeamento de URL será:

```
https://site.com            --> mywebsite/index.html
https://site.com/login.html --> mywebsite/login.html
https://site.com/admin/     --> mywebsite/admin/index.html
```

No entanto, para nosso aplicativo web estamos usando um único arquivo HTML contendo todas as telas, portanto esse comportamento padrão não nos ajudará. Temos que criar este mapa manualmente e atualizar o modelo exibido usando JavaScript.

### Tarefa

Usaremos um objeto simples para implementar um [mapa](https://en.wikipedia.org/wiki/Associative_array) entre caminhos de URL e nossos modelos. Adicione este objeto na parte superior do seu arquivo `app.js`.

```js
const routes = {
  '/login': { templateId: 'login' },
  '/dashboard': { templateId: 'dashboard' },
};
```

Agora vamos modificar um pouco a função `updateRoute`. Em vez de passar diretamente o `templateId` como argumento, queremos recuperá-lo olhando primeiro a URL atual e, em seguida, usar nosso mapa para obter o valor de ID do modelo correspondente. Nós podemos usar [`window.location.pathname`](https://developer.mozilla.org/docs/Web/API/Location/pathname) para obter apenas a seção do caminho do URL.

```js
function updateRoute() {
  const path = window.location.pathname;
  const route = routes[path];

  const template = document.getElementById(route.templateId);
  const view = template.content.cloneNode(true);
  const app = document.getElementById('app');
  app.innerHTML = '';
  app.appendChild(view);
}
```

Aqui mapeamos as rotas que declaramos para o modelo correspondente. Você pode testar se funciona corretamente alterando o URL manualmente em seu navegador.

✅ O que acontece se você inserir um caminho desconhecido na URL? Como poderíamos resolver isso?

## Adicionando Navegação

O próximo passo do nosso aplicativo é adicionar a possibilidade de navegar entre as páginas sem ter que alterar a URL manualmente. Isto implica duas coisas:

   1. Atualizando o URL atual
   2. Atualizando o modelo exibido com base no novo URL

Já cuidamos da segunda parte com a função `updateRoute`, então temos que descobrir como atualizar a URL atual.

Nós teremos que usar JavasScript e mais especificamente [`history.pushState`](https://developer.mozilla.org/docs/Web/API/History/pushState) que nos permite atualizar a URL e criar uma nova entrada no histórico do browser, sem ter que recarregar o HTML.

> Nota: Enquanto a âncora do HTML [`<a href>`](https://developer.mozilla.org/docs/Web/HTML/Element/a) pode ser usado sozinho para criar hiperlinks para diferentes URLs, fará com que o navegador recarregue o HTML por padrão. É necessário evitar esse comportamento ao tratar o roteamento com javascript customizado, utilizando a função preventDefault() no evento click.

### Tarefa

Vamos criar uma nova função que podemos usar para navegar em nosso aplicativo:

```js
function navigate(path) {
  window.history.pushState({}, path, path);
  updateRoute();
}
```

Este método primeiro atualiza o URL atual com base no caminho fornecido e, em seguida, atualiza o modelo. A propriedade `window.location.origin` retorna a raiz da URL, permitindo-nos reconstruir uma URL completa a partir de um determinado caminho.

Agora que temos esta função, podemos resolver o problema que temos se um caminho não corresponder a nenhuma rota definida. Modificaremos a função `updateRoute` adicionando um substituto a uma das rotas existentes se não conseguirmos encontrar uma correspondência.

```js
function updateRoute() {
  const path = window.location.pathname;
  const route = routes[path];

  if (!route) {
    return navigate('/login');
  }

  ...
```

Se uma rota não puder ser encontrada, iremos agora redirecionar para a página `login`.

Agora vamos criar uma função para obter a URL quando um link é clicado e para evitar o comportamento padrão do link do navegador:

```js
function onLinkClick(event) {
  event.preventDefault();
  navigate(event.target.href);
}
```

Vamos completar o sistema de navegação adicionando ligações aos nossos links *Login* e *Logout* no HTML.

```html
<a href="/dashboard" onclick="onLinkClick(event)">Login</a>
...
<a href="/login" onclick="onLinkClick(event)">Logout</a>
```

O objeto `event` acima captura o evento `click` e o passa para nossa função `onLinkClick`.

Usando o atributo [`onclick`](https://developer.mozilla.org/docs/Web/API/GlobalEventHandlers/onclick) vincula o evento `click` ao código JavaScript, aqui a chamada para a função `navigate()`.

Tente clicar nesses links, agora você poderá navegar entre as diferentes telas do seu aplicativo.

✅ O método `history.pushState` faz parte do padrão HTML5 e é implementado em [todos os browsers modernos](https://caniuse.com/?search=pushState). Se você estiver criando um aplicativo da web para navegadores mais antigos, há um truque que você pode usar no lugar desta API: usando uma [hash (`#`)](https://en.wikipedia.org/wiki/URI_fragment) antes do caminho você pode implementar um roteamento que funciona com navegação âncora regular e não recarrega a página, pois seu objetivo era criar links internos dentro de uma página.

## Manipulando os botões voltar e avançar do navegador

Usar `history.pushState` cria novas entradas no histórico de navegação do navegador.

Se você tentar clicar no botão Voltar algumas vezes, verá que o URL atual muda e o histórico é atualizado, mas o mesmo modelo continua sendo exibido.

Isso ocorre porque a aplicação não sabe que precisamos chamar `updateRoute()` toda vez que o histórico mudar. Se você der uma olhada na [documentação `history.pushState`](https://developer.mozilla.org/docs/Web/API/History/pushState), você pode ver isso se o estado mudar - o que significa que mudamos para um URL diferente - o evento [`popstate`](https://developer.mozilla.org/docs/Web/API/Window/popstate_event) foi acionado. Usaremos isso para corrigir esse problema.

### Tarefa

Para garantir que o modelo exibido seja atualizado quando o histórico do navegador for alterado, anexaremos uma nova função que chama `updateRoute()`. Faremos isso na parte inferior do nosso arquivo `app.js`:

```js
window.onpopstate = () => updateRoute();
updateRoute();
```

> Nota: Nós estamos usando uma [arrow function](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Functions/Arrow_functions) aqui para declarar nosso manipulador de eventos `popstate` para fins de concisão, mas uma função regular funcionaria da mesma forma.

Aqui está um vídeo de atualização sobre as arrow functions:

[![Arrow Functions](https://img.youtube.com/vi/OP6eEbOj2sc/0.jpg)](https://youtube.com/watch?v=OP6eEbOj2sc "Arrow Functions")

Agora tente usar os botões voltar e avançar do seu navegador e verifique se a rota exibida está atualizada corretamente desta vez.

---

## 🚀 Desafio!

Adicione um novo modelo e direcione para uma terceira página que mostre os créditos deste aplicativo.

## Quiz pós-lição

[Quiz pós-lição](https://ashy-river-0debb7803.1.azurestaticapps.net/quiz/42)

## Revisão & auto estudo

O roteamento é uma das partes surpreendentemente complicadas do desenvolvimento web, especialmente à medida que a web passa de comportamentos de atualização de página para atualizações de página de aplicativo de página única. Leia um pouco sobre [how the Azure Static Web App service](https://docs.microsoft.com/azure/static-web-apps/routes/?WT.mc_id=academic-77807-sagibbon) resolvendo roteamento. Você pode explicar por que algumas das decisões descritas nesse documento são necessárias?
