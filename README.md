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

  Agora imagine que você não tenha uma ***prop*** e sim dezenas delas, como você faria? :sweat:

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

  Atualmente, é muito comum a combinação de **Redux** e ***React***. Para que isto possa ocorrer, é necessário realizar uma **conexão**
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
  const INITIAL_STATE = []; // por convenção utilizamos letras maíusculas para nomear constantes

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

  * Definimos um estado inicial para o nosso <code>reducer</code> de lista de compras, neste caso na varíavel <code>INITIAL_STATE</code> que recebe um array vazio inicialmente, pois
  vamos precisar adicionar itens que pretendemos comprar no futuro.

  * A função <code>shoppingListReducer</code> é o nosso <code>reducer</code>, que recebe como parâmetro default, ou seja, se não enviarmos um novo estado o <code>reducer</code> irá receber o que 
  existe dentro do array <code>INITIAL_STATE</code>, o estado inicial que acabamos de definir e uma <b><i>action</i></b>.

  * A <b><i>action</i></b>, por convenção, é um objeto que deverá possuir uma chave (<i>key</i>) chamada <code>type</code>. É exatamente com essa chave que o <code>reducer</code> irá manipular o estado, através de um <code>switch</code> na maioria dos casos.

  * No exemplo acima, estamos querendo adicionar um produto a nossa lista de compras, então caso o <code>type</code> da <b><i>action</i></b> seja <code>ADD_PRODUCT</code>, vamos utilizar o operador
  <code>spread (...)</code> para que assim seja mantido as informações anteriores, e vamos passar também o value da <b><i>action</i></b> para incrementar o estado atual com esta nova informação.

  * Caso o <code>type</code> da action não seja <code>ADD_PRODUCT</code>, então o <code>reducer</code> irá retornar somente o estado atual, não alterando nenhuma informação.

  Agora, vamos criar o arquivo <code>src/actions/index.js</code>, que irá conter a nossa <code>action</code>:

  ```javascript
  export const addProduct = (value) => ({ type: 'ADD_PRODUCT', value });
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

  Note que utilizamos a função 
  , que faz exatamente o que o nome sugere combina todos os reducers passados como parâmetro, passando como parâmetro um objeto que contém o nosso <code>reducer</code>. 
  
  <blockquote> Dica: Perceba que é interessante, mesmo que tenhamos apenas um <code>reducer</code> na nossa aplicação, utilizar o método <code>combineReducers</code>, pois se for necessário (e provavelmente será!) adicionar novos <code>reducers</code> caso a aplicação
  cresca, não precisaremos alterar toda a lógica constrúida anteriormente. :sunglasses:
  </blockquote>

  Continuando, passamos para a função <code>createStore</code> o <code>rootReducer</code> que contém o nosso <code>reducer</code> criado anteriormente.

  Quando a função <code>combineReducers</code> é utilizada, o estado da nossa aplicação fica inserido em um objeto. Nesse objeto, cada <code>reducer</code> será representado por uma chave com o seu próprio nome que definimos e terá como valor o estado que é responsável por controlar.

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
 porque essa é puramente a estrutura do <code>Redux</code>. O próximo passo é conectá-la ao *React*.

 Para fazermos uso do estado compartilhado que o **Redux** provê, vamos editar o arquivo <code>src/App.js</code> com as seguintes informações:

 ```javascript
  import React from 'react';
  import { Provider } from 'react-redux'; // importamos de forma { desestruturada } o Provider da biblioteca react-redux;
  import store from './store';

  class App extends React.Component {
    render() {
      return (
        <div>
          <Provider store={ store }> 
            // o provider é o meio pelo qual disponibilizamos o Store para os componentes da nossa aplicação.
            // componentes aqui
          </Provider>
        </div>
      );
    }
  }

  export default App;
 ```

 Agora vamos adicionar os nossos componentes shoppingList.js e inputsList e conectá-los ao Redux utilizando o Provider com uma ***prop*** chamada <i>store</i> que contém o nosso <b><i>store</i></b> 
 importado.

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
  import { connect } from 'react-redux'; // importamos de forma { desestruturada } o connect da biblioteca react-redux;
  import { addProduct } from './actions'; // como definimos o arquivo como index.js não precisamos especificar a qual arquivo estamos nos referindo!
  
  class InputsList extends React.Component {
    constructor() {
      super();
      this.state = { product: '' };
    }

    render() {
      return (
        <div>
          <input
            type="text"
            placeholder="Digite o nome do produto a ser adicionado"
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
    add: product => dispatch(addProduct(product))
  });

  export default connect(null, mapDispatchToProps)(InputsList); // O null ocupa o lugar do mapStateToProps, afinal não precisamos ler nada do estado global, apenas enviar uma prop para lá!
 ```

 Calma! Vamos entender o que está acontecendo aqui:

 * Primeiro de tudo, nós estamos definindo um estado local do componente chamado <code>product</code> que inicia com uma string vazia. Interessante notar que, apesar de estarmos usando o Redux, que centraliza todos os states, caso exista algum estado o qual não precisamos inserir no estado global, podemos declará-lo localmente sem problema algum! Isto é normal e muito comum, pois nem tudo precisa, necessariamente, estar disponível globalmente na aplicação.

 * Criamos um <code>input</code> do tipo texto para a pessoa usuária possa adicionar um produto à sua lista de compras. A cada mudança no valor do <code>input</code>, este é salvo na chave <code>product</code> por meio da propriedade <code>onChange</code>.

 * Mas, calma lá! Tem um botão com a propriedade <code>onClick</code> criado, passando para uma função <code>add</code> que está presente nas <code>props</code> do componente. O que é? Como vive? Do que se alimenta? E o que é esse <code>connect</code>? Vamos entender logo abaixo.


## mapDispatchToProps

  A função <code>mapDispatchToProps</code> é a responsável por enviar uma ação para o reducer. Ou seja, é como se fosse um setter de uma propriedade para o estado global, fazendo uma analogia aos nossos <code>get</code> e <code>set</code> famosos nesse nosso mundo da programação.

  Para podermos ter acesso às essas funcionalidades do Redux, seja a de ler (mapStateToProps, que veremos a seguir!) os dados ou enviá-los (mapDispatchToProps), precisamos acessá-las como ***props*** de um componente. Por isso, como o próprio nome da função infere, ela mapeia (envia) os <code>dispatchs</code> para as ***props*** do componente no qual estamos trabalhando.

  Perceba que no ínicio do arquivo estamos importanto a action addProduct, criada por nós anteriormente. Neste caso, estamos nomeando arbitrariamente (pode ser o nome que você quiser!) uma propriedade chamada <code>add</code>, que faz o dispatch da action addProduct com um parâmetro arbitrário, porém é interessante associar um nome que faça sentido, chamado product.

  O mapDispatchToProps (setter para o estado global das ***props***), assim como o mapStateToProps (já iremos ver!), podem ser criados via funções convencionais (<code>functions</code>) ou arrow functions (<code>=></code>). O que importa é que o retorno seja um objeto, pois o <b><i>Redux</i></b> espera assim.

  Não esqueça! Podemos apenas enviar uma <code>action</code> para um <code>reducer</code> através de um <code>dispatch</code>, como demonstrado no código acima. Ou seja, o dispatch é uma função que recebe outra função, definida por nós, que dentro dela está a <code>action ({ type: 'ADD_PRODUCT', value })</code>.

  E por fim estamos utilizando a função <code>connect</code> para, como o próprio nome dá a entender, <i>conectar</i> o <code>Redux</code> ao nosso componente.


## connect

 O método connect possui uma sintaxe um pouco incomum, mas não se assuste, não se preocupe em entender o porquê dela ser desta maneira, mas sim em como usá-la neste primeiro momento!

 A sintaxe dela é assim: <code>connect()()</code> é essa função que faz a ponte entre o <code>Store</code> do ***Redux*** e o componente.

 No primeiro parênteses, devem estar presentes os métodos nativos do ***Redux***, que são o <code>mapStateToProps</code> (iremos ver logo abaixo!) e o <code>mapDispatchToProps</code>. Mas para termos condições de entender o contexto vamos fazer a analogia com o <code>get</code> e o<code>set</code>, o <code>mapStateToProps</code> seria o getter e o <code>mapDispatchToProps</code> seria o setter.
 Quando queremos passar uma ***prop*** para que ela exista no estado global, usamos o <code>mapDispatchToProps</code> e quando queremos ler do estado global utilizamos <code>mapStateToProps</code>.

 No segundo parênteses, passamos o próprio componente.

 Agora, vamos criar o nosso <code>ShoppingList</code>:

 ```javascript
  import React from 'react';
  import { connect } from 'react-redux'; // importamos de forma { desestruturada } o Provider da biblioteca react-redux;

  class ShoppingList extends React.Component {
    render() {
      return (
        <div>
          <div>
            {this.props.shoppingList.map(product => (
              <p>{product}</p>
            ))}
          </div>
        </div>
      );
    }
  }

  const mapStateToProps = state => ({
    shoppingList: state.shoppingListReducer
  });

  export default connect(mapStateToProps, null)(ShoppingList); // o mapDispatchToProps está null porque não estamos enviando nada para o estado global, apenas lendo dele!
```
 Vamos dar uma analisada nesse código:

 * Mas, calma aí! Nós estamos fazendo um map com os elementos presentes no array <code>shoppingList</code> que, por sua vez, está presente no componente como ***props***. Mas como isso foi parar lá? Foi mágica? Não! Vamos entender, finalmente, o <code>mapStateToProps</code>.


## mapStateToProps

A função <code>mapStateToProps</code> realiza a "<b><i>leitura</i></b>" das informações armazenadas nos estados para uma ***prop***, ela funciona como um <code>getter</code> do estado global e a insere em uma ***prop*** que você, como pessoa desenvolvedora, irá escolher qualquer bom nome, no exemplo anterior nomeamos a ***prop*** como <code>shoppingList</code>, mas poderia ser qualquer outro nome!

Perceba que as estruturas dos métodos nativos <code>mapStateToProps</code> e <code>mapDispatchToProps</code> sempre seguirão o mesmo padrão, o que irá mudar são as propriedades que vamos acessar ou <i><b>actions</b></i> que vamos disparar! 

No caso acima, o que iria mudar seria a ***prop*** lida do estado global, porém, a estrutura permaneceria igual, ainda continuaríamos precisando passar o <code>state</code> como parâmetro e note que escolhemos, também, o <code>reducer</code> no qual está armazenada essa informação, no nosso caso é no <code>shoppingListReducer</code>.

Por fim, <b><i>conectamos</i></b> o ***Redux*** ao componente, fazendo uso do <code>connect()()</code>. Como, neste caso, estamos fazendo apena a leitura dos dados, passamos a função  <code>mapStateToProps</code> e em seguida <code>null</code> no primeiro parênteses e o componente no segundo, respeitando assim a sintaxe.

A estrutura react-redux está concluída! Perceba que a estrutura pura do ***Redux*** constitui-se em: 

 * <code>store</code>, <code>actions</code> e <code>reducers</code>. 

Por sua vez, a estrutura de *conexão* entre o *React* e o ***Redux*** é composta basicamente por: 

 * <code>provider</code>, <code>connect</code>, <code>dispatch</code>, <code>mapDispatchToProps</code> e <code>mapStateToProps</code>. 


## Fluxo das informações no Redux

 Abaixo, temos uma imagem que demonstra, de forma simplificada, o caminho das informações dentro de uma aplicação React com ***Redux***:

 ![reduxInformationTraffic](https://miro.medium.com/max/1200/0*95tBOgxEPQAVq9YO.png)

 Depois de tudo isso, vamos recapitular os principais pontos abordados hoje:

 1. Um <code>Store</code> é criado para servidr de armazém para todos os estados da aplicação;

 2. O <code>Store</code> é provido através do <code>Provider</code> para todos os componentes da aplicação;

 3. É através do <code>connect</code> que os componentes se <i>conectam</i> ao <code>Store</code>;

 4. Devemos utilizar o método <code>combineReducers</code> passando como parâmetro o(s) reducer(s) que existe(m), ainda que tenhamos apenas um <code>reducer</code>, de forma a permitir a escalabilidade da aplicação;

 5. Existem métodos nativos do ***Redux***, o primeiro deles chamado de <code>mapStateToProps</code> que é responsável pela leitura do estado global;

 6. O segundo deles, chamado de <code>mapDispatchToProps</code> é responsável por enviar as <b><i>Actions</i></b>, que por sua vez, irão instruir o reducer a alterar o seu estado atual;

 7. As pessoas usuárias que utilizam a aplicação interagem com ela e disparam eventos;

 8. Esses <i>eventos</i> são chamados de <b><i>Actions</i></b> e são enviadas ao <code>Store</code> e processadas no <code>Reducer</code> por meio de um <code>dispatch</code>;

 9. <b><i>Actions</i></b>, por convenção, devem possuir ser um <i>objeto javascript</i> que deverá possuir uma chave (<i>key</i>) chamada <code>type</code>;

 10. O(s) <code>reducer(s)</code> recebe(m) essas <b><i>Actions</i></b> e realiza(m) alguma alteração no estado da aplicação e salvando o novo estado no <code>Store</code> para manter a imutabilidade do mesmo;

 11. Os componentes conectados ao <code>Store</code> "observam" tais mudanças e atualizam a visualização da aplicação (<i>View</i>);

## Exercícios
#### <sup>Tempo sugerido para realização: 80 minutos</sup>

### Agora a prática

Você irá desenvolver 2 exercícios para ampliar seus conhecimentos de Redux com React:
 
 1. 

 2.

## BÔNUS

 1. 

## Recursos adicionais (opcional)

 * <a href="https://medium.com/the-web-tub/managing-your-react-state-with-redux-affab72de4b1">Managing your React state with Redux</a>

 * <a href="https://medium.com/@hliojnior_34681/entenda-react-e-redux-de-uma-vez-por-todas-c761bc3194ca">Entendendo React e Redux de uma vez por todas</a>

 * <a href="https://react-redux.js.org/introduction/why-use-react-redux">Why Use React Redux?</a>

 * <a href="https://redux.js.org/introduction/getting-started">Getting Started with Redux</a>
