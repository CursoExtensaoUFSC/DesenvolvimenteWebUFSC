# Noções básicas de JavaScript: métodos e funções

![Noções básicas de JavaScript - Funções](https://github.com/CursoExtensaoUFSC/DesenvolvimenteWebUFSC/blob/main/Arquivos/webdev101-js-functions.png)

## Quiz Pré-Aula
[Quiz Pré-Aula](https://ashy-river-0debb7803.1.azurestaticapps.net/quiz/9)

Quando pensamos em escrever código, sempre queremos garantir que nosso código seja legível. Embora isso pareça contra-intuitivo, o código é lido muito mais vezes do que escrito. Uma ferramenta central na caixa de ferramentas de uma pessoa desenvolvedora para garantir código sustentável é a **função**.

[![Métodos e Funções](https://img.youtube.com/vi/aAfSVldL6Vk/0.jpg)](https://youtube.com/watch?v=aAfSVldL6Vk "Métodos e Funções")

> Clique na imagem acima para ver um vídeo sobre métodos e funções.


## Funções

Em sua essência, uma função é um bloco de código que podemos executar sob demanda. Isso é perfeito para cenários em que precisamos realizar a mesma tarefa várias vezes; em vez de duplicar a lógica em vários locais (o que tornaria difícil atualizá-la quando chegar a hora), podemos centralizá-la em um local e chamá-la sempre que precisarmos que a operação seja realizada - você pode até chamar funções de outras funções !.

Também importante é a capacidade de nomear uma função. Embora isso possa parecer trivial, o nome fornece uma maneira rápida de documentar uma seção de código. Você pode pensar nisso como um rótulo em um botão. Se eu clicar em um botão que diz "Cancelar cronômetro", sei que o relógio vai parar de funcionar.

## Criar e chamar uma Função

A sintaxe de uma função é semelhante a esta:

```javascript
function nameOfFunction() { // definição de função
 // definição de função/corpo
}
```

Se eu quisesse criar uma função para exibir uma saudação, poderia ser assim:

```javascript
function displayGreeting() {
  console.log('Olá, mundo!');
}
```

Sempre que quisermos chamar (ou invocar) nossa função, usamos o nome da função seguido por `()`. É importante notar o fato de que nossa função pode ser definida antes ou depois de decidirmos chamá-la; o compilador JavaScript encontrará ela para você.

```javascript
// calling our function
displayGreeting();
```

> **NOTA:** Existe um tipo especial de função conhecido como **método**, que você já está usando! Na verdade, vimos isso em nossa demonstração acima, quando usamos `console.log`. O que torna um método diferente de uma função é que um método é anexado a um objeto (`console` em nosso exemplo), enquanto uma função está flutuando livremente. Você ouvirá muitas pessoas desenvolvedoras usarem esses termos alternadamente.

### Práticas recomendadas de função

Existem várias práticas recomendadas para se ter em mente ao criar funções

- Como sempre, use nomes descritivos para sabermos o que a função fará
- Use **camelCasing** para combinar palavras
- Mantenha suas funções focadas em uma tarefa específica

## Passando informações para uma função

Para tornar uma função mais reutilizável, você frequentemente desejará passar informações para ela. Se considerarmos nosso exemplo `displayGreeting` acima, ele exibirá apenas **Olá, mundo!**. Não é a função mais útil que se poderia criar. Se quisermos torná-lo um pouco mais flexível, como permitir que alguém especifique o nome da pessoa a ser cumprimentada, podemos adicionar um **parâmetro**. Um parâmetro (às vezes também chamado de **argumento**), é uma informação adicional enviada para uma função.

Os parâmetros são listados na parte de definição entre parênteses e são separados por vírgulas, assim:

```javascript
function name(param, param2, param3) {

}
```

Podemos atualizar nosso `displayGreeting` para aceitar um nome e exibi-lo.

```javascript
function displayGreeting(name) {
  const message = `Hello, ${name}!`;
  console.log(message);
}
```

Quando queremos chamar nossa função e passar o parâmetro, especificamos entre parênteses.

```javascript
displayGreeting('Christopher');
// displays "Hello, Christopher!" when run
```

## Valores padrão

Podemos tornar nossa função ainda mais flexível adicionando mais parâmetros. Mas e se não quisermos que todos os valores sejam especificados? Seguindo nosso exemplo de saudação, poderíamos deixar o nome conforme necessário (precisamos saber quem estamos saudando), mas queremos permitir que a própria saudação seja personalizada conforme desejado. Se alguém não quiser personalizá-lo, fornecemos um valor padrão. Para fornecer um valor padrão a um parâmetro, nós o definimos da mesma forma que definimos um valor para uma variável - `parameterName = 'defaultValue'`. Para ver um exemplo completo:

```javascript
function displayGreeting(name, salutation='Hello') {
  console.log(`${salutation}, ${name}`);
}
```

Quando chamamos a função, podemos decidir se queremos definir um valor para `salutation`.

```javascript
displayGreeting('Christopher');
// displays "Hello, Christopher"

displayGreeting('Christopher', 'Hi');
// displays "Hi, Christopher"
```

## Valores de retorno

Até agora, a função que construímos sempre será enviada para o [console](https://developer.mozilla.org/docs/Web/API/console). Às vezes, isso pode ser exatamente o que estamos procurando, especialmente quando criamos funções que chamarão outros serviços. Mas e se eu quiser criar uma função auxiliar para realizar um cálculo e fornecer o valor de volta para que eu possa usá-lo em outro lugar?

Podemos fazer isso usando um **valor de retorno**. Um valor de retorno é retornado pela função e pode ser armazenado em uma variável da mesma forma que podemos armazenar um valor literal, como uma string ou número.

Se uma função retornar algo, então a palavra-chave `return` é usada. A palavra-chave `return` espera um valor ou referência do que está sendo retornado assim:

```javascript
return myVariable;
```  

Poderíamos criar uma função para criar uma mensagem de saudação e retornar o mensagem para a pessoa usuária.

```javascript
function createGreetingMessage(name) {
  const message = `Hello, ${name}`;
  return message;
}
```

Ao chamar esta função, armazenaremos o valor em uma variável. É quase da mesma forma que definiríamos uma variável com um valor estático (como `const name = 'Christopher'`).

```javascript
const greetingMessage = createGreetingMessage('Christopher');
```

##  Funções como parâmetros para funções

Conforme você progride em sua carreira de programação, encontrará funções que aceitam funções como parâmetros. Esse truque interessante é normalmente usado quando não sabemos quando algo vai ocorrer ou se completar, mas sabemos que precisamos realizar uma operação em resposta.

Como exemplo, considere [setTimeout](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/setTimeout), que inicia um cronômetro e executará o código quando ele for concluído. Precisamos dizer a ele qual código queremos executar. Parece um trabalho perfeito para uma função!

Se você executar o código abaixo, após 3 segundos verá a mensagem **3 segundos decorreram**.

```javascript
function displayDone() {
  console.log('3 seconds has elapsed');
}
// timer value is in milliseconds
setTimeout(displayDone, 3000);
```

### Funções Anônimas

Vamos dar outra olhada no que construímos. Estamos criando uma função com um nome que será usado uma vez. Conforme nossa aplicação fica mais complexa, podemos nos ver criando uma série de funções que serão chamadas apenas uma vez. Isso não é o ideal. Acontece que nem sempre precisamos fornecer um nome!

Quando passamos uma função como parâmetro, podemos pular a criação de uma com antecedência e, em vez disso, construir uma como parte do parâmetro. Usamos a mesma palavra-chave `function`, mas em vez disso, a construímos como um parâmetro.

Vamos reescrever o código acima para usar uma função anônima:

```javascript
setTimeout(function() {
  console.log('3 seconds has elapsed');
}, 3000);
```

Se você executar nosso novo código, notará que obtemos os mesmos resultados. Criamos uma função, mas não precisamos dar um nome a ela!

### Arrow Functions

Um atalho comum em muitas linguagens de programação (incluindo JavaScript) é a capacidade de usar o que é chamado de função **arrow** ou **arrow function**. Ela usa um indicador especial de `=>`, que se parece com uma flecha (arrow, em inglês) - daí o nome! Usando `=>`, podemos pular a palavra-chave `function`.

Vamos reescrever nosso código mais uma vez para usar arrow function:

```javascript
setTimeout(() => {
  console.log('3 seconds has elapsed');
}, 3000);
```

### Quando usar cada estratégia

Agora você viu que temos três maneiras de passar uma função como parâmetro e você pode estar se perguntando quando usar cada uma delas. Se você sabe que usará a função mais de uma vez, crie-a normalmente. Se você for usá-lo apenas para um local, geralmente é melhor usar uma função anônima. Sobre usar Arrow Functions ou a sintaxe mais tradicional de `function` é com você, mas você notará que a maioria das pessoas desenvolvedoras modernas prefere` => `.

---

## 🚀 Desafio

Você pode articular em uma frase a diferença entre funções e métodos? Tente!

## Quiz Pós-Aula
[Quiz Pós-Aula](https://ashy-river-0debb7803.1.azurestaticapps.net/quiz/10)

## Revisão e autoestudo

Vale a pena [ler um pouco mais sobre arrow functions](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Functions/Arrow_functions), como elas são cada vez mais usados em bases de código. Pratique escrever uma função e, em seguida, reescrevê-la com esta sintaxe.
##  Tarefa

[Diversão com funções](https://github.com/CursoExtensaoUFSC/DesenvolvimenteWebUFSC/blob/main/Arquivos/Li%C3%A7%C3%B5es%20de%20Casa/assignment3.pt.md)
