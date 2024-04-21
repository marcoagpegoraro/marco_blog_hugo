+++
  author = "Marco Pegoraro"
  title = "Criando uma aplicação web com Dart. Parte 1: Aqueduct: Básico, conexão com banco e autenticação JWT"
  date = "2019-10-26 00:00:00"
  description = ""
  tags = ["Api","Aqueduct","Dart","Databases","JWT"] 
  header_image = "/images/1*E2_tBhriGjKZz3NM_Lh3QA.png"
+++
  
O Dart é uma linguagem de programação criada dentro da Google em 2011 com o objetivo de substituir o javascript como linguagem principal nos navegadores. porém, devido a sua facilidade e curva de aprendizado, acabou se tornando uma linguagem com propósito geral, podendo hoje ser utilizada no backend, frontend e principalmente no mobile e desktop utilizando o framework Flutter.

![](/images/1*E2_tBhriGjKZz3NM_Lh3QA.png)

Nesse tutorial, iremos utilizar o dart para construir o básico de uma aplicação “to-do list” utilizando o framework Aqueduct.

## Requisitos

É necessário estar instalado na maquina o SDK do dart, pela linguagem ser multi plataforma, sua instalação pode ser feita tanto para Windows, Linux ou macOS. neste tutorial, estaremos utilizando a versão 2.5.

## [Get the Dart SDK](https://dart.dev/get-dart?source=post%5Fpage-----bd3afad3ae1a--------------------------------)

### [The Dart SDK has the libraries and command-line tools that you need to develop Dart web, command-line, and server apps…](https://dart.dev/get-dart?source=post%5Fpage-----bd3afad3ae1a--------------------------------)

[dart.dev](https://dart.dev/get-dart?source=post%5Fpage-----bd3afad3ae1a--------------------------------)

  
Com o Dart instalado, abra um terminal e digite o seguinte comando para realizar a instalação do aqueduct pelo pub, que é o próprio gerenciador de pacotes do Dart.

pub global activate aqueduct
  
Estaremos utilizando o Visual Studio Code para editar nossos códigos junto com a extensão oficial do Dart. E para testar a aplicação, estaremos utilizando o PostMan.

## [Download Visual Studio Code - Mac, Linux, Windows](https://code.visualstudio.com/download?source=post%5Fpage-----bd3afad3ae1a--------------------------------)

### [Visual Studio Code is free and available on your favorite platform - Linux, macOS, and Windows. Download Visual Studio…](https://code.visualstudio.com/download?source=post%5Fpage-----bd3afad3ae1a--------------------------------)

[code.visualstudio.com](https://code.visualstudio.com/download?source=post%5Fpage-----bd3afad3ae1a--------------------------------)

  
## [Dart](https://marketplace.visualstudio.com/items?itemName=Dart-Code.dart-code&source=post%5Fpage-----bd3afad3ae1a--------------------------------)

### [Dart Code extends VS Code with support for the Dart programming language, and provides tools for effectively editing…](https://marketplace.visualstudio.com/items?itemName=Dart-Code.dart-code&source=post%5Fpage-----bd3afad3ae1a--------------------------------)

[marketplace.visualstudio.com](https://marketplace.visualstudio.com/items?itemName=Dart-Code.dart-code&source=post%5Fpage-----bd3afad3ae1a--------------------------------)

  
## [Download Postman App](https://www.getpostman.com/downloads/?source=post%5Fpage-----bd3afad3ae1a--------------------------------)

### [Be the first to experience new Postman features! Can't wait to see what Postman has in store for you? Be the first to…](https://www.getpostman.com/downloads/?source=post%5Fpage-----bd3afad3ae1a--------------------------------)

[www.getpostman.com](https://www.getpostman.com/downloads/?source=post%5Fpage-----bd3afad3ae1a--------------------------------)

  
## Inicio do projeto

Com as ferramentas necessárias já instaladas, abra um novo terminal em sua pasta de projetos e rode o seguinte comando para realizar a criação do projeto.

aqueduct create todo
Para abrir o projeto no visual studio code, navegue para dentro da pasta utilizando o comando

cd todo
e em seguida

code .
Dentro da pasta lib, existe um arquivo chamado channel.dart, esse arquivo é onde será declarado as nossa rotas e controllers.

  
O primeiro passo que iremos fazer será criar uma pasta dentro da pasta lib chamada models, onde será criado nossas classes modelos. O primeiro arquivo que criaremos será a classe de ToDo, no caso _to\_do.dart_.

  
Dentro desse arquivo, criaremos uma classe estendendo de uma outra classe chamada _Serializable_, isso é necessário para serializar o ToDo para JSON e vice versa.

  
Além das propriedades da classe, sendo elas id, name e done, também teremos dois métodos que fazem parte da classe herdada _Serializable_, sendo elas o asMap e readFromMap. No final, a classe ficará da seguinte maneira:

  
Em seguida, criaremos uma pasta dentro da pasta lib chamada _controllers_, onde iremos criar um arquivo chamado _to\_do\_controller.dart_.

  
Dentro desse arquivo, criaremos uma classe que irá ser estendida de _ResourceController,_ que é a classe padrão do aqueduct para a criação de métodos http dentro de um controller.

  
A controller com os principais métodos http e uma lista de ToDo’s mockados para fins de testes, ficaria assim:

  
Por fim, precisaremos incluir a controller de “todos” e qual será a sua rota no arquivo channel.dart.

  
Em seguida, abra o terminal do visual studio code utilizando o comando Ctrl + shift + \` e rode o comando “aqueduct serve” para rodar a aplicação.

Com o postman, você já poderá fazer requisições para a api:

GET:    localhost:8888/todo
GET:    localhost:8888/todo/1
POST:   localhost:8888/todo
PUT:    localhost:8888/todo
DELETE: localhost:8888/todo/1
  
Porém, você perceberá que os dados não são persistidos, por exemplo, ao fazer um método post ou delete, isto acontece por que o Aqueduct reseta o estado da aplicação para cada request, isso é melhor explicado no seguinte link da documentação oficial:

## [Multi-threading - Aqueduct](https://aqueduct.io/docs/application/threading/?source=post%5Fpage-----bd3afad3ae1a--------------------------------)

### [ One of the primary differentiators between Aqueduct and other server frameworks is its multi-threaded behavior. When…](https://aqueduct.io/docs/application/threading/?source=post%5Fpage-----bd3afad3ae1a--------------------------------)

[aqueduct.io](https://aqueduct.io/docs/application/threading/?source=post%5Fpage-----bd3afad3ae1a--------------------------------)

Alguns links interessantes:

## [The Builder Pattern in Java, and Dart Cascades](https://dev.to/jvarness/the-builder-pattern-in-java-and-dart-cascades-5l7?source=post%5Fpage-----bd3afad3ae1a--------------------------------)

### [Object construction is something that everyone will have to do in a language that has object-oriented paradigms…](https://dev.to/jvarness/the-builder-pattern-in-java-and-dart-cascades-5l7?source=post%5Fpage-----bd3afad3ae1a--------------------------------)

[dev.to](https://dev.to/jvarness/the-builder-pattern-in-java-and-dart-cascades-5l7?source=post%5Fpage-----bd3afad3ae1a--------------------------------)

  
## [1\. Getting Started](https://aqueduct.io/docs/tut/getting-started/?source=post%5Fpage-----bd3afad3ae1a--------------------------------#the-more-you-know-multi-threading-and-application-state)

### [ By the end of this tutorial, you will have created an Aqueduct application that serves fictional heroes from a…](https://aqueduct.io/docs/tut/getting-started/?source=post%5Fpage-----bd3afad3ae1a--------------------------------#the-more-you-know-multi-threading-and-application-state)

[aqueduct.io](https://aqueduct.io/docs/tut/getting-started/?source=post%5Fpage-----bd3afad3ae1a--------------------------------#the-more-you-know-multi-threading-and-application-state)

# **Conexão com banco de dados PostgreSQL**

Agora, iremos fazer algumas modificações para fazer acesso a um banco de dados Postgres em nossa API.

  
Primeiramente, é necessário uma instalação do Postgres na maquina de desenvolvimento, podendo ela ser feita direto na maquina ou utilizando o Docker.

Estarei utilizando neste tutorial, a imagem oficial do Postgres no dockerhub, para subir o serviço na minha maquina, utilizarei o seguinte comando no terminal:

docker run --name postgres -p 5432:5432 -e POSTGRES_PASSWORD=postgres -d postgres:9.6
  
Com o projeto aberto no Visual Studio Code, faremos algumas modificações na model de todo para ser feito o uso dela no banco de dados, a classe não precisará estender de Serializable pois a classe ManagedObject já estende de Serializable.

  
Em seguida, faremos algumas modificações no arquivo channel.dart
  
  
Aqui criamos uma variável context que guardará as informações de banco de dados e em seguida, iremos passar ela para o nosso controller de ToDo fazer as consultas de banco de dados. Precisaremos preparar o nosso controller para receber o contexto e fazer as operações de banco de dados, ficando assim:
  
  
Ajustamos o controller para receber o contexto de banco de dados e modicamos todos os métodos http para fazer consultas ao banco de dados utilizando a classe Query do Aqueduct. Mais informações sobre o ORM podem ser encontradas nos links:

## [Basic Queries — Aqueduct](https://aqueduct.io/docs/db/executing%5Fqueries/?source=post%5Fpage-----bd3afad3ae1a--------------------------------)

### [ To send commands to a database — whether to fetch, insert, delete or update objects — you will create, configure and…](https://aqueduct.io/docs/db/executing%5Fqueries/?source=post%5Fpage-----bd3afad3ae1a--------------------------------)

[aqueduct.io](https://aqueduct.io/docs/db/executing%5Fqueries/?source=post%5Fpage-----bd3afad3ae1a--------------------------------)

## [Advanced Queries — Aqueduct](https://aqueduct.io/docs/db/advanced%5Fqueries/?source=post%5Fpage-----bd3afad3ae1a--------------------------------)

### [ The rows from a table can be sorted and fetched in contiguous chunks. This sorting can occur on most properties. For…](https://aqueduct.io/docs/db/advanced%5Fqueries/?source=post%5Fpage-----bd3afad3ae1a--------------------------------)

[aqueduct.io](https://aqueduct.io/docs/db/advanced%5Fqueries/?source=post%5Fpage-----bd3afad3ae1a--------------------------------)

  
Nosso projeto já está quase pronto para funcionar com banco de dados, agora falta somente fazer a migração da model ToDo para o banco de dados, o primeiro passo é criar um arquivo na pasta raiz do projeto chamada database.yaml, que será utilizado pelo aqueduct para localizar o banco de dados e fazer as migrações, o conteúdo do arquivo será então:

  
Por fim, iremos criar a migração. Com o terminal do Visual Studio Code aberto, rode o seguinte comando para criar uma migração:

aqueduct db generate
Com esse comando, o Aqueduct detecta as models do projeto para criar a migração, as migrações ficam dentro do diretório migrations.

Para aplicar a migração, precisaremos rodar o comando:

aqueduct db upgrade
Aplicada a migração, você já poderá testar o projeto realizando requisições HTTP, você perceberá que os dados agora estão sendo persistidos.

![](/images/1*1EcLJoehsq7pmjj91bSmPg.png)

Exemplo de requisição POST

![](/images/1*ko_oy_7s09m5Q8zd2g9syw.png)

Dados persistidos no banco de dados

# **Autenticação JWT**

  
Como qualquer aplicação web, é necessário algum tipo de autenticação. Uma das maneiras mais comuns que se tem hoje em dia para realizar autenticação em API’s REST é utilizando um padrão chamado JWT, para mais informações, recomendo a leitura do seguinte artigo do Wellington Nascimento sobre o assunto:

## [Entendendo tokens JWT (Json Web Token)](https://medium.com/tableless/entendendo-tokens-jwt-json-web-token-413c6d1397f6?source=post%5Fpage-----bd3afad3ae1a--------------------------------)

### [Esse artigo apareceu primeiro em wellingtonjhn.com](https://medium.com/tableless/entendendo-tokens-jwt-json-web-token-413c6d1397f6?source=post%5Fpage-----bd3afad3ae1a--------------------------------)

[medium.com](https://medium.com/tableless/entendendo-tokens-jwt-json-web-token-413c6d1397f6?source=post%5Fpage-----bd3afad3ae1a--------------------------------)

  
Neste artigo, demonstrarei como aplicar esse padrão de autenticação na nossa API em Dart.

**Relacionamento one-to-many**

A primeira coisa que iremos fazer será criar um model para o usuário e fazer a relação com o ToDo. Dentro da pasta models, criaremos então uma classe user.dart:

  
Perceba que existe uma propriedade _ManagedSet_ que será responsável pela relacionamento one-to-many com o ToDo, mas precisaremos incluir o relacionamento também na classe de ToDo, portanto, adicione o atributo a seguir na classe todo.dart:

@Relate(#toDo)User user;
Por fim, precisaremos criar uma migration no banco de dados, para criar uma migration, utilizamos o comando

aqueduct db generate
Na migration criada, iremos fazer uma pequena alteração, pois ela incluiu a criação de uma coluna _password_ na tabela _\_user_, o que não será necessário pois gravaremos somente o hash da senha no banco de dados, então sinta-se a vontade para remover o seguinte trecho da migration:

SchemaColumn("password", ManagedPropertyType.string,isPrimaryKey: false,autoincrement: false,isIndexed: false,isNullable: false,isUnique: false),
Para aplicar a migration, utilize o comando:

aqueduct db upgrade
**Geração do token JWT**

Em seguida, importaremos uma dependência que será utilizada para gerar e descriptografar o token JWT. Então no arquivo pubspec.yaml, inclua a dependência:

jaguar_jwt: ^2.1.6
  
Em seguida, criaremos um diretório dentro de _lib_ chamado _utils_ e dentro desta pasta, criar um arquivo utils.dart que será utilizado para deixar funções comuns que serão utilizadas por todo o projeto. Dentro deste arquivo, teremos alguns atributos e métodos:

1. jwtKey — A chave privada para a criação dos tokens JWT.
2. generateJWT — Método que receberá um usuário e retornará uma string (o próprio token JWT).
3. generateSHA256Hash — Método que irá gerar o hash da senha do usuário.

O arquivo utils.dart ficará assim:

  
Agora, precisaremos criar dois novos controllers, o primeiro controller será utilizado para criar um novo usuário e gerar um hash da senha para o mesmo, ficando assim:

  
E por fim, um controller que será utilizado para validar o usuário e devolver um token autenticado para ele, chamaremos ele de session\_controller.dart.

  
**Middleware de autenticação**

  
No Aqueduct, todo controller é um middleware, você pode inclusive encadear controllers para se chegar em um resultado esperado, é isso que iremos fazer, antes da requisição chegar no ToDoController, precisaremos ter um middleware que verificará se o token JWT passado pelo header é realmente válido. Para fazer isso, criaremos uma pasta dentro de lib chamada middlewares, e dentro dela, uma classe jwt\_middleware.dart que terá o comportamento muito parecido com um controller, porém não irá retornar nada para o usuário, somente passar a requisição adiante para o próximo controller.

  
Neste middleware, vamos descriptografar o token, pegar o id do usuário no token e fazer algumas validações (ver se o usuário existe, verificar a data de expiração do token, etc) e por fim, passar o usuário do token para o próximo controller.

Para mais informações sobre como validar os dados do token:

## [jaguar\_jwt | Dart Package](https://pub.dev/packages/jaguar%5Fjwt?source=post%5Fpage-----bd3afad3ae1a--------------------------------)

### [JWT utilities for Dart and Jaguar.dart This library can be used to generate and process JSON Web Tokens (JWT). For more…](https://pub.dev/packages/jaguar%5Fjwt?source=post%5Fpage-----bd3afad3ae1a--------------------------------)

[pub.dev](https://pub.dev/packages/jaguar%5Fjwt?source=post%5Fpage-----bd3afad3ae1a--------------------------------)

Em seguida, precisaremos fazer algumas modificações na variável routes do arquivo channel.dart para incluir os novos controllers e o novo middleware de autenticação.

router.route("/todo/[:id]").link(() => JwtMiddleware(context)).link(() => ToDoController(context));router.route("/user/[:id]").link(() => UserController(context));router.route("/session/[:id]").link(() => SessionController(context));
Por fim, precisaremos fazer algumas modificações no ToDoController, pois agora o mesmo precisa filtrar os todo’s também pelo id do usuário, existem diversas maneiras de se fazer isso, mas para deixar simplificado, utilizaremos o id que foi obtido do token.

  
**Testes**

![](/images/1*rVL2n9z5uSC0Vo-ufFKbLg.png)

Criação de usuário

![](/images/1*VUB06K9BaZcJNonGfHvhrw.png)

Autenticação de usuário

![](/images/1*qOqasYbla8vIa1fU7Nl5fg.png)

Colocando o token no header da requisição

Agora você poderá criar, editar, listar e deletar os ToDo’s baseado no usuário logado.

![](/images/1*1CLn1ArVmPhGyS-AN47aNg.png)

Inclusão de _ToDo_

![](/images/1*TPPunKpj7PlmcfyOxWfEpg.png)

Listagem de _ToDo_

  
Com isso, terminamos o nosso backend em Dart, para finalizar, no próximo artigo demonstrarei como criar um frontend para consumir a API criada utilizando o framework AngularDart.

## [Criando uma aplicação web com Dart. Parte 2: AngularDart: Básico, roteamento e chamadas HTTP](https://medium.com/flutter-comunidade-br/criando-uma-aplicação-web-com-dart-parte-2-angulardart-básico-roteamento-e-chamadas-http-9a7e7187a477?source=post%5Fpage-----bd3afad3ae1a--------------------------------)

### [No artigo anterior, ensinei como criar uma aplicação backend utilizando a linguagem de programação Dart juntamente com…](https://medium.com/flutter-comunidade-br/criando-uma-aplicação-web-com-dart-parte-2-angulardart-básico-roteamento-e-chamadas-http-9a7e7187a477?source=post%5Fpage-----bd3afad3ae1a--------------------------------)

[medium.com](https://medium.com/flutter-comunidade-br/criando-uma-aplicação-web-com-dart-parte-2-angulardart-básico-roteamento-e-chamadas-http-9a7e7187a477?source=post%5Fpage-----bd3afad3ae1a--------------------------------)

  
O repositório do projeto pode ser encontrado no seguinte link do meu GitHub:

## [marcoagpegoraro/todo-app-aqueduct](https://github.com/marcoagpegoraro/todo-app-aqueduct?source=post%5Fpage-----bd3afad3ae1a--------------------------------)

### [It's necessary a postgres database in order for this application to work properly, modify channel.dart ant…](https://github.com/marcoagpegoraro/todo-app-aqueduct?source=post%5Fpage-----bd3afad3ae1a--------------------------------)

[github.com](https://github.com/marcoagpegoraro/todo-app-aqueduct?source=post%5Fpage-----bd3afad3ae1a--------------------------------)
  
  