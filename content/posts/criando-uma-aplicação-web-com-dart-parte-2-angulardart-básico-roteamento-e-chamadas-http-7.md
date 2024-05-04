+++
  author = "Marco Pegoraro"
  title = "Criando uma aplicação web com Dart. Parte 2: AngularDart: Básico, roteamento e chamadas HTTP"
  date = "2019-10-27 00:00:00"
  description = ""
  tags = [""] 
  header_image = "/images/1x9n1DT5QKLTZa9sFxNxtkg.png"
+++
  
![](/images/1x9n1DT5QKLTZa9sFxNxtkg.png)

No artigo anterior, ensinei como criar uma aplicação backend utilizando a linguagem de programação Dart juntamente com o framework Aqueduct, o artigo pode ser encontrado no seguinte link:

## [Criando uma aplicação web com Dart. Parte 1: Aqueduct: Básico, conexão com banco e autenticação JWT](https://medium.com/flutter-comunidade-br/criando-uma-aplicação-web-com-dart-parte-1-aqueduct-básico-conexão-com-banco-e-autenticação-jwt-bd3afad3ae1a?source=post%5Fpage-----9a7e7187a477--------------------------------)

### [O Dart é uma linguagem de programação criada dentro da Google em 2011 com o objetivo de substituir o javascript como…](https://medium.com/flutter-comunidade-br/criando-uma-aplicação-web-com-dart-parte-1-aqueduct-básico-conexão-com-banco-e-autenticação-jwt-bd3afad3ae1a?source=post%5Fpage-----9a7e7187a477--------------------------------)

[medium.com](https://medium.com/flutter-comunidade-br/criando-uma-aplicação-web-com-dart-parte-1-aqueduct-básico-conexão-com-banco-e-autenticação-jwt-bd3afad3ae1a?source=post%5Fpage-----9a7e7187a477--------------------------------)

  
Para consumir a api criada, utilizaremos um framework frontend chamado AngularDart, que é uma variação do conhecido framework Angular, só que, ao invés de utilizar o TypeScript, é utilizado o Dart. O resultado da aplicação pode ser vista no GIF a seguir:

![](/images/1mOvWe2rRYUJAqmTiFcSDWw.gif)

Demonstração da aplicação

Aviso: para este tutorial, é recomendado ter um conhecimento prévio sobre frameworks single page application, não entrarei em muitos detalhes de como é o funcionamento de algumas features abordadas.

# Setup

Para começar a aplicação, precisaremos clonar o projeto startup do próprio AngularDart, que pode ser encontrado no link a seguir:

## [angular-examples/quickstart](https://github.com/angular-examples/quickstart/tree/d9e768b947f2787cc45eedbfb0d97530e6b5dd07?source=post%5Fpage-----9a7e7187a477--------------------------------)

### [Welcome to the example app used in the Setup for Development page of Dart for the web. You can run a hosted copy of…](https://github.com/angular-examples/quickstart/tree/d9e768b947f2787cc45eedbfb0d97530e6b5dd07?source=post%5Fpage-----9a7e7187a477--------------------------------)

[github.com](https://github.com/angular-examples/quickstart/tree/d9e768b947f2787cc45eedbfb0d97530e6b5dd07?source=post%5Fpage-----9a7e7187a477--------------------------------)

Precisaremos também, instalar o WebDev, que será utilizado para servir a aplicação, a ferramenta poderá ser instalada com o comando:

pub global activate webdev 2.5.3
Para subir uma aplicação AngularDart em modo de desenvolvedor, utilizamos o seguinte comando dentro da pasta do projeto:

webdev serve
No desenvolvimento deste tutorial, encontrei um problema ao rodar a aplicação dizendo que meu compilador estava com bugs, talvez não aconteça com você, mas para resolver isso, foi necessário somente o seguinte comando no terminal para limpar o cache da aplicação:

pub run build_runner clean
No arquivo pubspec.yaml, podemos observar que estamos utilizando a versão 6.0.0-alpha, que é a versão mais recente do framework no momento que este artigo foi escrito. Para darmos continuação ao desenvolvimento, iremos adicionar algumas dependências, o arquivo então ficará assim:

  
1. angular\_router — Dependência do angular utilizado para roteamento.
2. bulma\_scss — Framework CSS que utilizaremos no projeto.
3. font\_awesome — Biblioteca de ícones.
4. http — Biblioteca para realização de requisições HTTP.
5. angular\_forms — Dependência do angular utilizado para formulários.
6. sass\_builder — Utilizado para uso de arquivos scss.

# **Estilos**

  
Agora, iremos apagar o conteúdo do arquivo styles.css e substituir com o seguinte conteúdo para deixar a página com uma fonte mais agradável e ocupando 100% da página, não precisaremos de mais nenhum estilo pois estaremos utilizando o framework CSS [Bulma](https://bulma.io/).

  
Para adicionar o Bulma no projeto, criaremos um arquivo _app\_component.scss_ dentro da pasta lib com as importações necessárias, o arquivo ficará assim:

  
Em seguida, precisaremos importar o arquivo scss no _app\_component.dart_, para fazer isso, adicionaremos a seguinte propriedade no _@component_:

styleUrls: [    'app_component.css',],
O arquivo scss será convertido para css automaticamente pela dependência _sass\_builder_.

# **Roteamento**

Para começar a implementação do Angular router, precisaremos injetar o router no app Angular, para fazer isso, edite o arquivo main.dart para ficar da seguinte maneira:

  
Dentro de lib, criaremos uma pasta chamada components, no qual colocaremos todos os componentes da aplicação. Nossa aplicação terá 4 componentes principais, um componente de barra de navegação que ficará sempre visível a aplicação, um componente de login, outro de cadastro e por fim, um dashboard para listar os ToDo’s.

  
Então dentro da pasta components, crie as pastas navbar, login, signup e dashboard e dentro de cada pasta, crie dois arquivos, um com extensão dart e outro html com o mesmo nome da pasta, a estrutura do projeto ficará assim:

lib
│
└───components
│    │
│    └───dashboard
│    │   │   dashboard.dart
│    │   │   dashboard.html
│    │
│    └───login
│    │   │   login.dart
│    │   │   login.html
│    │
│    └───navbar
│    │   │   navbar.dart
│    │   │   navbar.html    
│    └───signup
│    │   │   signup.dart
│    │   │   signup.html
│
│   app_component.dart
│   app_component.scss
Dentro de cada componente, criaremos um código base só para podermos ver o roteamento funcionando.
  
  
Dentro de cada html, você pode colocar um texto somente para diferenciar cada componente caso desejar.

Em seguida, criaremos um arquivo _route\_paths.dart_ dentro da pasta _lib_, este arquivo será onde declararemos nossas rotas:

  
Por fim, criaremos um arquivo chamado _routes.dart_ dentro da pasta _lib_ que será o arquivo que relacionará as rotas com os componentes do sistema:
  
  
Definido as rotas, precisaremos editar o app\_component.dart para que o sistema reconheça as rotas criadas e dependendo da url, mostrar o componente desejado, para isso, importaremos as classes de roteamento recém criadas e criaremos um arquivo app\_component.html no qual terá a tag router-outlet, que será responsável pela renderização dos componentes dependendo da rota.

app\_component.dart:

  
app\_component.html:

  
E com isso, o roteamento básico da página já estará funcionando, com o comando _webdev serve_ rodando, você já poderá navegar entre as páginas:

http://localhost:8080/#/login
http://localhost:8080/#/signup
http://localhost:8080/#/dashboard
# **Navbar**

Antes de criar a navbar, precisaremos de uma variável global para a nossa aplicação para verificar se o usuário está ou não autenticado. Também criaremos uma variável chamada baseUrl na qual será utilizada para auxiliar nas requisições http, para isso, criaremos uma pasta utils dentro de lib, e dentro desta pasta, criaremos um arquivo utils.dart:

  
A navbar do sistema ainda será um componente, porém ele ficará fixo na página independente da rota, para fazer isso, importaremos o componente diretamente no app component junto com a classe Utils:

  
Logo a seguir, colocaremos o navbar no html do app:

  
Com isso, já será possível observar que o navbar se mantem presente em todas as rotas do sistema

# **Finalizando o navbar**

  
Utilizaremos o seguinte html no componente navbar, todo o processo pode ser visto na documentação do bulma. Na linha 11, temos um método necessário para lidar com o menu quando é uma tela de celular, iremos implementar este método no Dart. Os botões da barra de navegação serão diferentes caso o usuário esteja logado ou não, por isso, implementamos na linha 22 e 28 dois IFs para fazer esta verificação, um para se o usuário esta logado e outro caso não esteja.

  
navbar.dart:

  
Feito isso, já é para o navbar da aplicação estar funcionando.

![](/images/1FEiTlKLTIe2O7Qb7wEivAg.gif)

# **Componente de Signup**

Para o design da tela de Sign Up, utilizaremos um card, e dentro dele, colocaremos os campos necessários para o cadastro e um botão para cadastro do usuário, também criaremos um modal de confirmação de cadastro.
  
  
# **Componente de Login**

O design da tela de login será bem semelhante, dentro de um card, colocaremos dois campos para o usuário digitar os dados de login e um botão para realizar o login, também colocaremos um modal para informar o usuário caso o login informado não exista na base de dados:
  
  
# **Componente Dashboard**

  
Para o componente de dashboard, estaremos utilizando dois cards, um para os botões de ação (adicionar e atualizar), e outro para a listagem de tarefas, perceba que cada tarefa terá seu próprio botão de exclusão e edição. Estaremos utilizando também um modal que será usado tanto para adicionar tarefas quanto para editar tarefas, para fazer isso, o método que chama o modal pode ou não receber uma tarefa como argumento, caso não receba, o modal será utilizado para adição, caso receba, será utilizado para editar a tarefa recebida.

  
Assim que a página for carregada (ngOnInit), será chamado um método getTodos, no qual será responsável de carregar as tarefas da nossa API Rest.
  
  
E com isso, finalizamos nosso frontend feito em AngularDart, espero ter gostado deste tutorial, o repositório do projeto final, tanto a api rest quanto o frontend poderão ser encontrados nos seguintes links:

## [marcoagpegoraro/todo-app-aqueduct](https://github.com/marcoagpegoraro/todo-app-aqueduct?source=post%5Fpage-----9a7e7187a477--------------------------------)

### [It's necessary a postgres database in order for this application to work properly, modify channel.dart ant…](https://github.com/marcoagpegoraro/todo-app-aqueduct?source=post%5Fpage-----9a7e7187a477--------------------------------)

[github.com](https://github.com/marcoagpegoraro/todo-app-aqueduct?source=post%5Fpage-----9a7e7187a477--------------------------------)

  
## [marcoagpegoraro/todo-app-angular-dart](https://github.com/marcoagpegoraro/todo-app-angular-dart?source=post%5Fpage-----9a7e7187a477--------------------------------)

### [Contribute to marcoagpegoraro/todo-app-angular-dart development by creating an account on GitHub.](https://github.com/marcoagpegoraro/todo-app-angular-dart?source=post%5Fpage-----9a7e7187a477--------------------------------)

[github.com](https://github.com/marcoagpegoraro/todo-app-angular-dart?source=post%5Fpage-----9a7e7187a477--------------------------------)
  
  