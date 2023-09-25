# Projeto Terrarium Parte 2: I a CSS

![Introdução a CSS](https://github.com/CursoExtensaoUFSC/DesenvolvimenteWebUFSC/blob/main/Arquivos/webdev101-css.png)

## Quiz pré-leitura 

[Quiz pré-leitura](https://ashy-river-0debb7803.1.azurestaticapps.net/quiz/17)

### Introdução:

CSS, ou Cascading Style Sheets, resolve um problema importante no desenvolvimento web: como fazer com que seu site pareça bom. Projetar suas aplicações as torna mais úteis e atraentes; você também pode usar CSS para criar um design web responsivo (RWD), o que permite que suas aplicações pareçam boas, independentemente do tamanho da tela em que são exibidas. CSS não se trata apenas de fazer com que sua aplicação pareça boa; sua especificação inclui animações e transformações que podem permitir interações sofisticadas para suas aplicações. O grupo de trabalho CSS ajuda a manter as especificações CSS atualizadas; você pode acompanhar seu trabalho no [site do World Wide Web Consortium](https://www.w3.org/Style/CSS/members).

> Tenha em mente que o CSS é uma linguagem que evolui, assim como tudo na web, e nem todos os navegadores suportam as partes mais recentes da especificação. Sempre verifique suas implementações consultando [CanIUse.com](caniuse.com).

Nesta lição, adicionaremos estilos ao nosso terrário online e aprenderemos mais sobre vários conceitos de CSS: a cascata, a herança e o uso de seletores, posicionamento e o uso de CSS para criar layouts. No processo, projetaremos o terrário e criaremos o próprio terrarium.

### Requisito prévio:

Você deve ter o HTML para o seu terrário construído e pronto para ser estilizado.

### Tarefa:

Na sua paste terrarium, crie um arquivo chamado `style.css`. Importe este arquiv na seção `<head>`:

```
<link rel="stylesheet" href="./style.css" />
```

---

## 1. A cachoeira

As folhas de estilo em cascata incorporam a ideia de que os estilos 'cascateiam' de modo que a aplicação de um estilo é guiada pela sua prioridade. Os estilos definidos pelo autor de um site têm prioridade sobre os definidos por um navegador. Os estilos configurados 'in-line' têm prioridade sobre os configurados em uma folha de estilo externa.

### Tarefa:

Adicione o estilo 'color: red' à sua tag `<h1>`:

```HTML
<h1 style="color: red">My Terrarium</h1>
```

Depois, adicione o seguinte código no seu arquivo: `style.css`:

```CSS
h1 {
 color: blue;
}
```

✅ Qual cor é exibida em sua aplicação web? Por quê? Você pode encontrar uma maneira de anular estilos? Quando você gostaria de fazer isso ou por que não?
---

## 2. Herança

Os estilos são herdados de um estilo ancestral para um estilo descendente, de modo que os elementos aninhados herdam os estilos de seus pais.

### Tarefa:

Defina a fonte do corpo para uma fonte específica e verifique a fonte de um elemento aninhado.

```
body {
	font-family: helvetica, arial, sans-serif;
}
```

Abra o console do seu navegador na guia 'Elementos' e observe a fonte do H1. Ele herda a fonte do corpo, como indicado no navegador.

![Fonte Heradada](../images/1.png)

✅ Você pode fazer com que um estilo aninhado herde uma propriedade diferente?

---

## 3. Seletores CSS

### Tags

Até agora, seu arquivo `style.css`  possui apenas algumas tags estilizadas, e a aplicação parece bastante estranha:

```
body {
	font-family: helvetica, arial, sans-serif;
}

h1 {
	color: #3a241d;
	text-align: center;
}
```

Esta forma de projetar uma etiqueta oferece controle sobre elementos individuais, mas você precisa controlar os estilos de muitas plantas em seu terrário. Para fazer isso, você precisa usar os seletores do CSS.

### ID

"Adicione algum estilo para projetar os contêineres esquerdo e direito. Dado que há apenas um contêiner esquerdo e apenas um contêiner direito, eles são identificados no markup. Para projetá-los, use `#`:

```
#left-container {
	background-color: #eee;
	width: 15%;
	left: 0px;
	top: 0px;
	position: absolute;
	height: 100%;
	padding: 10px;
}

#right-container {
	background-color: #eee;
	width: 15%;
	right: 0px;
	top: 0px;
	position: absolute;
	height: 100%;
	padding: 10px;
}
```

Aqui, você colocou esses contêineres com posicionamento absoluto no canto esquerdo e direito da tela e usou porcentagens para sua largura, para que eles possam se adaptar a telas móveis menores.

✅ Este código se repete bastante, portanto, não está 'DRY' (Don't Repeat Yourself: Não se repita); Você pode encontrar uma maneira melhor de projetar esses identificadores, talvez usando um ID e uma classe? Você precisaria alterar o markup e refatorar o CSS:

```html
<div id="left-container" class="container"></div>
```

### Classes

No exemplo anterior, você projetou dois elementos únicos na tela. Se deseja que os estilos se apliquem a muitos elementos na tela, pode usar classes CSS. Faça isso para colocar as plantas nos contêineres esquerdo e direito.

Observe que cada planta no markup HTML possui uma combinação de IDs e classes. Os IDs aqui são usados pelo JavaScript que será adicionado posteriormente para manipular a localização da planta no terrário. As classes, no entanto, aplicam um estilo específico a todas as plantas.

```html
<div class="plant-holder">
	<img class="plant" alt="plant" id="plant1" src="./images/plant1.png" />
</div>
```

Adicione o seguinte no seu arquivo `style.css`:

```css
.plant-holder {
	position: relative;
	height: 13%;
	left: -10px;
}

.plant {
	position: absolute;
	max-width: 150%;
	max-height: 150%;
	z-index: 2;
}
```

Neste trecho, destaca-se a combinação de posicionamento relativo e absoluto, que abordaremos na próxima seção. Observe como as alturas são tratadas em porcentagens:

Define a altura do suporte da planta como 13%, um número adequado para garantir que todas as plantas sejam exibidas em cada contêiner vertical sem a necessidade de rolagem.

Configura o suporte da planta para mover-se para a esquerda, permitindo que as plantas fiquem mais centralizadas dentro de seus contêineres. As imagens têm uma grande quantidade de fundo transparente, para que possam ser arrastadas mais facilmente, portanto, é necessário empurrá-las para a esquerda para que se encaixem melhor na tela.

Em seguida, a planta em si recebe uma largura máxima de 150%. Isso permite que ela diminua à medida que o navegador é redimensionado. Tente redimensionar seu navegador; as plantas permanecem em seus contêineres, mas diminuem para se ajustar.

Também é notável o uso do índice Z, que controla a altitude relativa de um elemento (para que as plantas fiquem no topo do contêiner e pareçam estar dentro do terrário).

✅ Por que você precisa tanto de um suporte para plantas quanto de um seletor CSS para plantas?

## 4. Posicionamento CSS

Misturar propriedades de posição (existem posições estáticas, relativas, fixas, absolutas e aderentes) pode ser um pouco complicado, mas quando feito corretamente, proporciona um bom controle sobre os elementos de suas páginas.

Os elementos com posição absoluta são posicionados em relação aos seus antecessores posicionados mais próximos e, se não houver nenhum, são posicionados de acordo com o corpo do documento.

Os elementos com posição relativa são posicionados de acordo com as diretrizes do CSS para ajustar sua localização longe de sua posição inicial.

Em nosso exemplo, o "plant-holder" é um elemento com posição relativa que é colocado dentro de um contêiner com posição absoluta. O resultado desse comportamento é que os contêineres das laterais são fixados à esquerda e à direita, e o porta-plantas se ajusta, encaixando-se entre as laterais, proporcionando espaço para que as plantas sejam colocadas em uma fileira vertical.

> A `planta` em si também possui um posicionamento absoluto, necessário para torná-la arrastável, como você descobrirá na próxima lição.

✅ Experimente alterar os tipos de posicionamento dos contêineres laterais e do porta-plantas. O que acontece?

## 5. Layouts CSS

Agora você usará o que aprendeu para construir o terrário em si, tudo usando CSS!

Primeiro, projete os elementos secundários `.terrarium` div como um retângulo arredondado usando CSS:

```css
.jar-walls {
	height: 80%;
	width: 60%;
	background: #d1e1df;
	border-radius: 1rem;
	position: absolute;
	bottom: 0.5%;
	left: 20%;
	opacity: 0.5;
	z-index: 1;
}

.jar-top {
	width: 50%;
	height: 5%;
	background: #d1e1df;
	position: absolute;
	bottom: 80.5%;
	left: 25%;
	opacity: 0.7;
	z-index: 1;
}

.jar-bottom {
	width: 50%;
	height: 1%;
	background: #d1e1df;
	position: absolute;
	bottom: 0%;
	left: 25%;
	opacity: 0.7;
}

.dirt {
	width: 60%;
	height: 5%;
	background: #3a241d;
	position: absolute;
	border-radius: 0 0 1rem 1rem;
	bottom: 1%;
	left: 20%;
	opacity: 0.7;
	z-index: -1;
}
```

Observe o uso de porcentagens aqui. Se você reduzir o zoom do seu navegador, também pode ver como os cantos do frasco se ajustam automaticamente. Observe também as porcentagens de largura e altura dos elementos do frasco e como cada elemento está posicionado absolutamente no centro, ancorado na parte inferior da janela gráfica.

✅ Tente alterar as cores e a opacidade do frasco em relação à sujeira. O que acontece? Por quê?

---

🚀 Desafio: adicione um brilho de 'bolha' à área inferior esquerda do frasco para que pareça mais semelhante ao vidro. Você estará projetando o `.jar-glossy-long` e o `.jar-glossy-short` para que pareçam um reflexo brilhante. É assim que ficaria:

![terrarium pronto](../images/terrarium-final.png)

## [Quiz pós-leitura](https://ashy-river-0debb7803.1.azurestaticapps.net/quiz/18)

## Revisão e auto-estudo

O CSS parece enganosamente simples, mas existem muitos desafios ao projetar uma aplicação perfeitamente compatível com todos os navegadores e tamanhos de tela. CSS Grid e Flexbox são ferramentas que foram desenvolvidas para tornar o trabalho um pouco mais estruturado e confiável. Aprenda sobre essas ferramentas jogando [Flexbox Froggy](https://flexboxfroggy.com/) e [Grid Garden](https://codepip.com/games/grid-garden/).

## Tarefa
---

[Refatoração CSS](assignment7.pt.md)

[Projete sua aplicação HTML com CSS](https://docs.microsoft.com/learn/modules/build-simple-website/4-css-basics/?WT.mc_id=academic-77807-sagibbon)
