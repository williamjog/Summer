# Usando o Redux no React

## O que vamos aprender? 

  Você irá aprender a integrar o Redux à suas aplicações React.

## Você será capaz de:

  * Criar a *store* do **Redux** em aplicações React
  * Criar *reducers* no **Redux** em aplicações React
  * Criar *actions* no **Redux** em aplicações React
  * Criar *dispatchers* no **Redux** em aplicações React
  * Integrar o **Redux** à sua aplicação React

## Por que isso é importante?

  A medida que a sua aplicação **React** torna-se grande e complexa, você irá notar que surgirá um grande incômodo nos momentos
  em que for necessário passar ***props*** para um componente muito distante na árvore hierárquica da sua aplicação. 

  Por exemplo:

  Vamos imaginar que na sua aplicação seja necessário passar uma *****props***** para um componente em quatro níveis abaixo do atual, neste
  cenário, seria necessário passar a determinada *****props***** para o componente logo abaixo, depois passar novamente para o outro 
  componente logo abaixo do anterior, ainda assim passar a ***props*** para o componente logo abaixo do segundo anterior para que assim
  seja possível acessar a tal ***props*** no componente alvo.

  Ufa! Viu que trabalho? Só de pensar dá pra se perder! :scream:

  Agora imagine que você não tenha uma ***props*** e sim dezenas delas, como você faria? :sweat:

  Já pensou no trabalhão que seria isso e na quantidade de possíveis erros que poderiam acontecer? :grimacing:

  Pensando neste cenário, seria ótimo se houvesse uma maneira de **armazenar** tudo em um único local de fácil acesso, por exemplo,
  em um estado unificado, não é mesmo?

  Pois é aí que entra o **Redux**! Ele é o responsável por gerenciar um *state* de forma global e acessível de qualquer lugar da sua
  aplicação, desta maneira não é necessário fazer uso de *prop drilling*, ou seja, passar informações para componentes que não as 
  necessitam, que neste cenário hipotético, possuem como único objetivo fazer a intermediação da ***props*** para o componente desejado.

## Conteúdos
#### <sup>Tempo sugerido para realização: 120 minutos</sup>
  
  Redux é uma biblioteca externa que pode ser utilizada com React, Vue, Angular e JavaScript puro, para dar *poucos* exemplos.

  Ele é um ecossistema completo e robusto, tornando o compartilhamento de vários *states* algo possível na sua aplicação React.

  Atualmente, é muito comum o a combinação de **Redux** e ***React***. Para que isto possa ocorrer, é necessário realizar uma **conexão**
  entre o Redux e o framekwork escolhido.

  Para realizar essa conexão utilizamos a biblioteca **React Redux**, ela é a responsável por fazer esse ***bind*** e você pode instalá-la
  utilizando o comando abaixo no seu terminal:

  ```bash
  npm install react-redux
  ```
  <sup><b><i><code>react redux</i></b></code> é a biblioteca oficial para realizar a conexão entre <i><b>React</b></i> e o <i><b>Redux</b></i>.</sup>

  Antes de continuarmos para o entendimento da estrutura do React com Redux, vamos recapitular alguns conceitos importantes:

  ### Store
  <blockquote>
  Uma <i>store</i> contém e <i><b>compartilha</b></i> toda a árvore de estado da sua aplicação. A única maneira de alterar o estado dentro dela é enviando uma <i>action</i> 
  através de um <i>dispatch</i>.
  </blockquote>

  ### Action
  <blockquote>
  As <i><b>actions</b></i> são <b>objetos javascript</b> que indicam diretamente uma <b>intenção</b> de mudança/alteração no <i>State</i> aplicação, essa <i><b>action</b></i> 
  pode ser algo disparado pelo próprio usuário ou algo disparado pela própria aplicação.
  </blockquote>

  ### Reducer
  <blockquote>
  Reducers são funções que recebem o estado atual (<i>current state</i>) e uma <i>action</i> como argumentos e retornam um novo estado (<i>new state</i>). Em outras palavras,
  <code>(estado, action) => novoEstado </code>.
  </blockquote>

  ### Dispatch
  <blockquote>
  É uma função cuja responsabilidade é enviar uma ação (<i>action</i>) para processamento no <b><i>Reducer</i></b>.
  </blockquote>

## Configurando Redux com React

  Agora que recapitulamos estes conceitos muito importantes, estamos prontos para criar e configurar uma aplicação React que fará uso do ***Redux***.

  Primeiro passo, vamos criar uma aplicação React tradicional:

  ```bash
  npx create-react-app my-app
  ```

  Após, vamos instalar as dependências necessárias:

  ```bash
  npm install --save redux react-redux
  ```
  <h4><sup><code>redux</code> é a biblioteca que possui toda a implementação do <b><i>Redux</i></b>.</h4>

  <h4><sup><code>react redux</code> é a biblioteca que realiza a conexão necessária para a implementação conjunta do <b><i>React</i></b> e do <b><i>Redux</i></b>.</h4>

  Agora, vamos imaginar que precisamos implementar uma solução com <b><i>Redux</i></b> para uma aplicação que simula um almoxarifado, neste almoxarifado podemos adicionar 
  e excluir itens, tudo baseado em seu estado atual.

  O nosso primeiro passo é criar o armazém universal de todos estados da nossa aplicação, o <b><i>Store</b></i>. É nele que ficarão <b>todos</b> os possíveis estados da nossa
  aplicação, pense nele como a única fonte da verdade no que diz respeito aos estados (<i>states</i>) da sua aplicação. 

  Portanto, vamos criar um arquivo <code>src/store/index.js que irá conter o seguinte conteúdo:

  ```javascript
  import { createStore } from 'redux';

  const store = createStore();

  export default store;
  ```