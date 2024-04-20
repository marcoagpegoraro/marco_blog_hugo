+++
  author = "Marco Pegoraro"
  title = "Utilizando CompletableFuture para fazer requisições assíncronas em uma aplicação Spring Cloud"
  date = "2020-08-22 00:00:00"
  description = ""
  tags = ["Completable Future","Java","Spring"] 
+++
  
![](https://miro.medium.com/v2/resize:fit:1400/1*eWlh07xrpxE2LHGjGqqjzg.png)Com um mundo cada vez mais demandando velocidade e usuários cada vez mais exigentes, surge a necessidade de realizar otimizações em aplicações rest para o tempo de requisição ser o menor possível.

Imagine um cenário onde você possui um orquestrador e para devolver a resposta para seu usuário, você precisa realizar outras requisições _HTTP_ em outros microsserviços,

Para demonstrar como realizar requisições assíncronas no _Spring_ _Cloud_ utilizando o _CompletableFuture_ do Java 8, será criado um projeto orquestrador que retornará os dados de um usuário, e para agrupar os dados desse usuário, será necessário fazer três requisições para três _REST APIs_ diferentes. o intuito será mostrar como realizar essas requisições simultaneamente e não sequenciais, diminuindo assim o tempo de resposta da _API_.

# Exemplo prático

A aplicação principal será uma aplicação _spring_ _cloud_ criada no site [https://start.spring.io](https://start.spring.io/) como mostrado na figura 1:

![](https://miro.medium.com/v2/resize:fit:1400/1*HYUHDAVQCp9BXo7JjlOwlA.png)

Figura 1: Configuração utilizada no microserviço orquestrador

Será utilizado a dependencia OpenFeign para realizar as requisições Http para os microsserviços secundários e o lombok para facilitar a criação das classes Java.

## Criação dos microsserviços que serão consumidos

Para simular os serviços a serem consumidos, foi feito três scripts utilizando Node.js com o framework Express.js, e para roda-los simultaneamente, foi utilizado um script para rodar três processos do Node para não ter nenhuma interferência entre os serviços.

A figura 2 mostra respectivamente os scripts do serviço um, que será utilizado para buscar informações de musicas do usuário, serviço dois que será utilizado para buscar informações gerais e serviço três que será utilizado para buscar informações de postagens desse usuário. Logo abaixo, existe o script _bash_ para rodar os servers simultaneamente e logo ao lado, a declaração de uma função _sleepOneSecond_ que será utilizada para simular um delay em um banco de dados fictício, o que não existe no exemplo pois os dados estão _mockados_.

![](https://miro.medium.com/v2/resize:fit:1400/1*gR1GPYGtHdJNgXrA_u0vQQ.png)

Figura 2 — Estrutura dos serviços em Node que serão consumidos.

## Preparando o orquestrador

Com o projeto criado no _Spring Initializr_, será criado primeiramente algumas classes para preenchimento dos dados que serão recebidos pela api, como pode ser visto na figura 3:

![](https://miro.medium.com/v2/resize:fit:1400/1*4U7Uku3IJ7hSwxZyhXo4ZQ.png)

Figura 3 — POJOs que serão utilizadas no projeto, a classe UserResponse será utilizada como a classe de saída do endpoint, já as demais serão as classes que serão preenchidas ao realizar as requisições.

Também será criado uma camada de _services_ e _clients_ na aplicação, que ficará responsável principalmente pela responsabilidade de realizar requisições _HTTP_ e retornar um objeto para quem estiver consumindo a camada de _service_, bem semelhante o que existe no _framework frontend Angular_, a implementação pode ser vista na figura 4:

![](https://miro.medium.com/v2/resize:fit:1400/1*hSKtoil1DrkreghWqmijJQ.png)

Figura 4 — Classes responsáveis por realizar as requisições HTTP, note que cada endpoint que será chamado tem sua própria camada de service e client.

Feito isso, agora será estruturado o _controller_ da aplicação, a estrutura do método principal conterá as três chamadas para os serviços, logo em seguida, será montado o objeto final do usuário utilizando o padrão de projeto _Builder_, já implementado pelo plugin _Lombok_, a figura 5 demonstra como ficou o resultado final da classe:

![](https://miro.medium.com/v2/resize:fit:1400/1*70ndlTX_to3ekmkSa31qPg.png)

Figura 5 — Controller principal da aplicação

> Não se esqueça de colocar a _annotation_ @EnableFeignClients logo acima da classe _main_ do seu projeto, para a injeção de dependencia @Autowired do _Spring_ funcionar corretamente.

Observe que também foi colocado um _println_ para mostrar no console quanto tempo durou a requisição, agora subindo a aplicação e enviando uma requisição pelo _postman_, verificamos que está tudo funcionando (lembre-se de subir as aplicações em Node também!) , como mostra a figura 6:

![](https://miro.medium.com/v2/resize:fit:1400/1*NE8Gvg2oZYmL86xNURTMIA.png)

Figura 6 — Resposta da API funcionando, i see this as an absolute win… right?

Perceba que os dados foram retornados corretamente, porém perceba que o tempo da requisição ficou cerca de quatro segundos, o que, dependendo do caso de uso, pode ser bem lento.

Isto ocorre pois as requisições _HTTP_ estão acontecendo uma atrás da outra, e se fosse possível realizar todas as requisições ao mesmo tempo e só obter as respostas no momento de montar o objeto de saída da aplicação? isto que será feito a seguir!

A versão 8 do Java possui uma API muito simples de ser utilizada chamada _CompletableFuture_, com ela, é possível solucionar o problema visto anteriormente com bastante facilidade, para demonstrar como é utilizado, será criado um novo método no _controller_ só que desta vez, utilizado requisições assíncronas, como visto na figura 7:

![](https://miro.medium.com/v2/resize:fit:1400/1*BOfXdmWv1x3FlQRsr41kmw.png)

Figura 7 — Novo método utilizando a API do CompletableFuture.

A principal diferença do novo método para o antigo é a de que utilizamos o _CompletableFuture.supplyAsync(Supplier<U> supplier)_ e passamos como argumento um _supplier_ retornando a função no qual é desejado o retorno, no caso, é feito isso nas três chamadas _HTTP_, o que acontece por debaixo dos panos é que cada supplyAsync cria uma nova thread para rodar a requisição paralelamente com a thread principal da aplicação.

Outra principal diferença é que o objeto que retornará das requisições é do tipo _CompletableFuture<T>_, isto acontece pois ainda não existe esse objeto em memória, já que ele será obtido no futuro, então as linhas 61 até 66 irão ser rodadas ao mesmo tempo sem uma bloquear a outra.

> Vale apontar também que os métodos do service precisam ter a annotation @Async para sinalizar para o Spring que o método é assíncrono, para mais detalhes, consultar o segundo link que está nas referencias no final do artigo.

A aplicação só será bloqueante novamente quando é feito um _.get()_ no objeto de retorno, pois assim é sinalizado para o Java que a aplicação não pode continuar a partir daquele ponto sem os dados retornados da API.

Com as alterações feitas, será feito então uma nova requisição pelo _postman_ só que pelo novo endpoint, como mostra a figura 8:

![](https://miro.medium.com/v2/resize:fit:1400/1*Qfol7zdRSRfTy_AD3ezywg.png)

Figura 8 — Requisição feita no novo _endpoint_ usando programação assíncrona, observe que durou 1/3 do tempo original da requisição.

Com isso, é possível observar o ganho obtido utilizando programação assíncrona com CompletableFuture, que é uma API que já está presente no Java 8 ou versões mais novas.

Vou estar deixado meu Linkedin aqui no post caso alguém tenha alguma duvida, critica ou sugestão a respeito do artigo, eu ficaria bem feliz pelo feedback! também estarei deixando o repositório da aplicação criada, junto com os scripts em Node utilizados para simular os microsserviços

Until then, stay safe!

## [Marco Antonio G. - FATEC Americana - Campinas, São Paulo, Brazil | LinkedIn](https://www.linkedin.com/in/marco-antonio-goncalves/?source=post%5Fpage-----44dd721f60e9--------------------------------)

### [View Marco Antonio G.'s profile on LinkedIn, the world's largest professional community. Marco Antonio's education is…](https://www.linkedin.com/in/marco-antonio-goncalves/?source=post%5Fpage-----44dd721f60e9--------------------------------)

[www.linkedin.com](https://www.linkedin.com/in/marco-antonio-goncalves/?source=post%5Fpage-----44dd721f60e9--------------------------------)

  
## [marcoagpegoraro/spring-cloud-requisicoes-assincronas](https://github.com/marcoagpegoraro/spring-cloud-requisicoes-assincronas?source=post%5Fpage-----44dd721f60e9--------------------------------)

### [Contribute to marcoagpegoraro/spring-cloud-requisicoes-assincronas development by creating an account on GitHub.](https://github.com/marcoagpegoraro/spring-cloud-requisicoes-assincronas?source=post%5Fpage-----44dd721f60e9--------------------------------)

[github.com](https://github.com/marcoagpegoraro/spring-cloud-requisicoes-assincronas?source=post%5Fpage-----44dd721f60e9--------------------------------)

  
## Referências

## [CompletableFuture (Java Platform SE 8 )](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/CompletableFuture.html?source=post%5Fpage-----44dd721f60e9--------------------------------)

### [supplyAsync Returns a new CompletableFuture that is asynchronously completed by a task running in the given executor…](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/CompletableFuture.html?source=post%5Fpage-----44dd721f60e9--------------------------------)

[docs.oracle.com](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/CompletableFuture.html?source=post%5Fpage-----44dd721f60e9--------------------------------)

## [Creating Asynchronous Methods](https://spring.io/guides/gs/async-method/?source=post%5Fpage-----44dd721f60e9--------------------------------)

### [This guide walks you through creating asynchronous queries to GitHub. The focus is on the asynchronous part, a feature…](https://spring.io/guides/gs/async-method/?source=post%5Fpage-----44dd721f60e9--------------------------------)

[spring.io](https://spring.io/guides/gs/async-method/?source=post%5Fpage-----44dd721f60e9--------------------------------)
  
  