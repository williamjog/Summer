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

  Pensando neste cenário, seria ótimo se houvesse uma maneira de **armazenar** tudo em um único local de fácil acesso, por exemplo
  em um estado unificado, não é mesmo?

  Pois é aí que entra o **Redux**! Ele é o responsável por gerenciar um *state* de forma global e acessível de qualquer lugar da sua
  aplicação, desta maneira não é necessário fazer uso de *prop drilling*, ou seja, passar informações para componentes que não as 
  necessitam, que neste cenário hipotético, possuem como único objetivo fazer a intermediação da ***props*** para o componente desejado.

## Conteúdos
###### Tempo sugerido para realização: 120 minutos
  
  Redux é uma biblioteca externa que pode ser utilizada com React, Vue, Angular e JavaScript puro, para dar *poucos* exemplos.

  Ele é um ecossistema completo e robusto, tornando o compartilhamento de vários *states* algo possível na sua aplicação React.

  Atualmente, é muito comum o a combinação de **Redux** e ***React***. Para que isto possa ocorrer, é necessário realizar uma **conexão**
  entre o Redux e o framekwork escolhido.

  Para realizar essa conexão utilizamos a biblioteca **React Redux**, ele é o responsável por fazer esse ***bind*** e podemos instalá-lo
  utilizando o comando abaixo no nosso terminal:

  ```bash
  npm install react-redux
  ```
  ***React Redux*** é a biblioteca oficial para realizar a conexão entre React e o Redux.

  Antes de continuarmos para o entendimento da estrutura do React com Redux, vamos recapitular alguns conceitos importantes:

  ### Store
  <blockquote>
  Uma <i>store<i> contém e <i><b>compartilha<b><i> toda a árvore de estado da sua aplicação. A única maneira de alterar o estado dentro dela é enviando uma <i>action<i>.
  <blockquote>