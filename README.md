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

  Agora, vamos imaginar que precisamos implementar uma solução com <b><i>Redux</i></b> para uma aplicação que simula uma lista de compras, neste caso podemos adicionar 
  e excluir itens, tudo baseado em seu estado atual.

  O nosso primeiro passo é criar o armazém universal de todos estados da nossa aplicação, o <b><i>Store</b></i>. É nele que ficarão <b>todos</b> os possíveis estados da nossa
  aplicação, pense nele como a única fonte da verdade no que diz respeito aos estados (<i>states</i>) da sua aplicação. 

  Portanto, vamos criar um arquivo <code>src/store/index.js</code> que irá conter o seguinte conteúdo:

  ```javascript
  import { createStore } from 'redux';

  const store = createStore();

  export default store;
  ```

  Mas, atenção! A função <code>createStore</code> necessita receber como parâmetro um <code>reducer</code>. Portanto, vamos criar um no arquivo <code>src/reducers/index.js</code>.

  O <code>reducer</code>, no nosso caso em específico, deverá ser capaz de adicionar e remover itens da lista de compras, tudo baseado no <code>state</code> retornado.

  ```javascript
  const INITIAL_STATE = [];

  function shoppingListReducer(state = INITIAL_STATE, action) {
    switch (action.type) {
      case 'ADD_PRODUCT':
        return [...state, action.value];
      default:
        return state;
    }
  }

  export default shoppingListReducer;
  ```

  Vamos entender o que está acontecendo:

  * Definimos um estado inicial para o nosso <code>reducer</code> de lista de compras, neste caso na varíavel <code>INITAL_STATE</code> que recebe um array vazio inicialmente, pois
  vamos precisar adicionar itens que pretendemos comprar no futuro.

  * A função <code>shoppingListReducer</code> é o nosso <code>reducer</code>, que recebe como parâmetro default, ou seja, se não enviarmos um novo estado o <code>reducer</code> irá receber o que 
  existe dentro do array <code>INITAL_STATE</code>, o estado inicial que acabamos de definir e uma <b><i>action</i></b>.

  * A <b><i>action</i></b>, por convenção, é um objeto que deverá possuir uma chave (<i>key</i>) chamada <code>type</code>. É exatamente com essa chave que o <code>reducer</code> irá manipular o estado, através de um <code>switch</code> na maioria dos casos.

  * No exemplo acima, estamos querendo adicionar um produto a nossa lista de compras, então caso o <code>type</code> da action seja <code>ADD_PRODUCT</code>, vamos utilizar o operador
  <code>spread</code> para que assim seja mantido as informações anteriores, e vamos passar também o value da <b><i>action</i></b> para incrementar o estado atual com esta nova informação.

  * Caso o <code>type</code> da action não seja <code>ADD_PRODUCT</code>, então o <code>reducer</code> irá retornar somente o estado atual, não alterando nenhuma informação.

  Agora, vamos criar o arquivo <code>src/actions/index.js</code>, que irá conter a nossa <code>action</code>:

  ```javascript
  export const addAssignment = (value) => ({ type: 'ADD_PRODUCT', value });
  ```

  Essa <b><i>action</i></b>, que é um objeto javascript, deve possuir a <i>key</i> type ou mais outras <i>keys</i>, caso seja interessante. Perceba que também criamos uma varíavel chamada 
  <code>value</code> que irá guardar o valor recebido da <b><i>action</i></b>.

  Depois, precisamos voltar ao nosso <b><i>store</b></i> e passar como parâmetro o nosso <code>reducer</code>, para que a função <code>createStore</code> esteja escrita corretamente.

  ```javascript
  import { createStore, combineReducers } from 'redux';
  import shoppingListReducer from '../reducers';

  const rootReducer = combineReducers({ shoppingListReducer });

  const store = createStore(rootReducer);

  export default store;
  ```

  Note que utilizamos a função <code>combineReducer</code>, que faz exatamente o que o nome sugere combina todos os reducers passados como parâmetro, passando como parâmetro um objeto que contém o nosso <code>reducer</code>. 
  
  <blockquote> Dica: Perceba que é interessante, mesmo que tenhamos apenas um <code>reducer</code> na nossa aplicação, utilizar o método <code>combineReducer</code>, pois se for necessário (e provavelmente será!) adicionar novos <code>reducers</code> caso a aplicação
  cresca, não precisaremos alterar toda a lógica constrúida anteriormente. :sunglasses:
  </blockquote>

  Continuando, passamos para a função <code>createStore</code> o <code>rootReducer</code> que contém o nosso <code>reducer</code> criado anteriormente.

  Quando a função <code>combineReducers</code> é utilizada, o estado da nossa aplicação fica disposto em um objeto. Nesse objeto, cada reducer será representado por uma chave com o seu respectivo
  nome e terá como valor o estado que é responsável por controlar.

  Observe como o estado inicial da nossa aplicação ficaria:

  ```javascript
  {
  shoppingListReducer: [],
  }
  ```

 <blockquote> Dica super valiosa: Para facilitar o uso cotidiano do Redux nas suas aplicações, sugerimos baixar a extensão <code>Redux Devtools</code>, na seção de <code>Recursos Adicionais</code>
 você encontrará um link explicando como instalá-la e utilizá-la. :smiley:
 </blockquote>

 Pronto! <code>Store</code>, <code>Reducer</code> e <code>Action</code> corretamente criados. Pode parecer que isso está fora de contexto de uma aplicação React, e até o momento realmente está 
 porque essa é puramente a estrutura do <code>Redux</code>. O próximo passo é conectá-lo ao *React*.

 Para fazermos uso do estado compartilhado que o **Redux** provê, vamos editar o arquivo <code>src/App.js</code> com as seguintes informações:

 ```javascript
  import React from 'react';
  import { Provider } from 'react-redux'; // importamos de forma desestruturada o Provider da biblioteca react-redux
  import store from './store';

  class App extends React.Component {
    render() {
      return (
        <div>
          <Provider store={ store }> 
            // o provider é o meio pelo qual disponibilizamos o Store para a nossa aplicação.
            // componentes aqui
          </Provider>
        </div>
      );
    }
  }

  export default App;
 ```

 Agora vamos adicionar os nossos componentes shoppingList.js e inputsList e conectá-los ao Redux utilizando o Provider com uma ***prop*** chamada store que contém o nosso store importado.
 Não podemos nos esquecer de importar e adicionar os componentes ao componente App:

 ```javascript
  import React from 'react';
  import { Provider } from 'react-redux';
  import store from './store';
  import ShoppingList from './ShoppingList';
  import InputsList from './InputsList';

  class App extends React.Component {
    render() {
      return (
        <div>
          <Provider store={ store }>
            <InputsList />
            <ShoppingList />
          </Provider>
        </div>
      );
    }
  }

  export default App;
 ```

 Depois, vamos realizar a implementação do componente InputsList:

  ```javascript
  import React from 'react';
  import { connect } from 'react-redux';
  import { addAssignment } from './actions';

  class InputsList extends React.Component {
    constructor(props) {
      super(props);
      this.state = { product: '' };
    }

    render() {
      return (
        <div>
          <input
            type="text"
            placeholder="Digite o nome do produto"
            onChange={event => this.setState({ product: event.target.value })}
          />
          <button onClick={() => this.props.add(this.state.product)}>
            Adicionar um novo produto à lista de compras
          </button>
        </div>
      );
    }
  }

  const mapDispatchToProps = dispatch => ({
    add: product => dispatch(addAssignment(product))});

  export default connect(null, mapDispatchToProps)(InputsList);
 ```
