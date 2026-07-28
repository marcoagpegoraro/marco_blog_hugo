+++
  author = "Marco Pegoraro"
  title = "Como aprendi a linguagem de programação Go e utilizei a mesma para programar meu blog pessoal"
  date = "2023-10-27 21:44:06.531000000"
  description = "Golang + Fiber + Gorm + AWS S3 + Django Templates + FlatifyCSS + Railway"
  tags = ["AWS","Fiber","FlatifyCSS","Go","GoLang","GORM","Railway","S3"] 
  header_image = "/images/2365d858-d162-4736-89d4-6d2e7eeab154.webp"
+++
  
Em 2007, engenheiros da Google perceberam que a base de código, principalmente escrita em C++, estava complexa demais e isso estava atrasando entregas, foi então visto que seria necessário a criação de uma nova linguagem de programação. Então Robert Griesemer, Rob Pike, e Ken Thompson - também criador da linguagem B - se reuniram e, com suas habilidades, criaram a linguagem Go, com o propósito de ser uma linguagem simples como Python, porém robusta e performática como C.

Embora eu já tenha brincado com algumas linguagens no passado, como PHP, Python, JavaScript e Dart, fazia um bom tempo que não aprendia uma nova linguagem de programação, cansado então de ver só Java no dia a dia, resolvi escolher uma para aprender.

Uma das opções foi a linguagem Rust, criada pela Mozilla, porém percebi que pouquíssimas pessoas estavam utilizando atualmente, e as que utilizavam, geralmente era para programação para embedded systems e blockchain, duas areas que desconheço. Mesmo sendo uma linguagem muito interessante, difícil no começo porém recompensadora conforme você vai aprendendo.

Resolvi então pegar o que as pessoas consideram a linguagem "rival" do Rust, a linguagem Go, percebi que o mesmo é bem mais utilizado em serviços web comparado ao Rust. Algumas empresas brasileiras já estão migrando suas bases de código para o Go, incluindo Magazine Luiza, Mercado Livre e Globo.com

![](/images/2365d858-d162-4736-89d4-6d2e7eeab154.webp)

# Como aprender uma linguagem nova?

Acredito que quando você domina uma linguagem de programação, no meu caso, Java, qualquer outra linguagem de programação é fácil de aprender e ser minimamente produtivo em questão de algumas poucas semanas.

Então ao invés de ficar procurando cursos para comprar, eu simplesmente fui por um caminho no qual muitos iniciantes na area da programação não fazem e não entendo o por que, que é simplesmente ler a documentação da linguagem, não é necessário pagar 20 reais em um curso da Udemy para aprender uma linguagem nova, eu digo que 100% dos casos, a documentação da linguagem/framework e algum eventual tutorial no Youtube já é o suficiente.

No caso do Go, existe um site bem legal chamado "A Tour of Go" (https://go.dev/tour/welcome/1), no qual você vai aprendendo e fazendo uma coisa de cada vez, pra quem já sabe qualquer uma linguagem de programação, é a maneira mais rápida e fácil de aprender Go.

# Começando o blog

Depois de aprender o básico da linguagem, como por exemplo: **types, stdin, stdout, if, else, elseif, for, structs, interfaces, goroutines**; foi a hora de tentar aplicar esse conhecimento em um projeto pessoal. Já estava querendo programar meu blog pessoal, mas não queria utilizar Next.js, pois há poucos meses utilizei React para criar meu site portfolio utilizando Github actions, e estava querendo utilizar alguma tecnologia nova, ai que veio a ideia de utilizar Go para isso.

O primeiro requisito que defini, é que seria uma aplicação MVC, ou seja, todo o processamento do HTML seria exclusivamente na parte do backend, utilizando templates.

Pesquisando, eu encontrei algumas opções que poderia utilizar, uma delas seria utilizar a própria stdlib do Go, porém optei por utilizar um framework web já pronto, com isso, facilitaria e aceleraria o desenvolvimento do projeto, o framework escolhido foi então o Fiber, pois ele utiliza como base o fasthttp, a engine HTTP mais rápida para Go, e também por ser bem similar ao framework Express do JavaScript, porém ainda não descarto a opção de migrar o projeto para o net/http.

A primeira coisa que fiz foi criar uma nova pasta no meu GOPATH chamada de marco\_blog, em seguida, defini algumas pastas para melhorar a organização do projeto, como por exemplo, controllers, models, services, routes, views, helpers, etc. Cada uma com sua própria responsabilidade, por exemplo, dentro dos controllers, não posso fazer nada além de obter as variáveis necessárias para a request dentro dos query params, body, etc, com esses valores, ai posso chamar o service para realizar o processamento da request.

# Templates

O Fiber oferece diversas opções de engines de templates HTML, dentre as opções, optei por utilizar o Django, pois é o mesmo utilizado pelo framework de mesmo nome no mundo Python. depois de realizar alguns testes de renderização de variáveis, loops e filtros, vi que seria uma opção bem sólida a se escolher, mas depois de terminar o projeto, vejo que talvez seria melhor escolher o próprio engine nativo do Go, pois percebi que alguns filtros funcionavam, já outros não, como por exemplo, o filtro de formatação de data.

# Hot Reload

Para rodar um projeto em go em modo de auto reload, encontrei duas alternativas, a primeira e mais conhecida é utilizar o air (https://github.com/cosmtrek/air), mas acabei por optando por utilizar um chamado CompileDaemon (https://github.com/githubnemo/CompileDaemon) pelo simples fato de que achei ela primeiro, mas as duas opções funcionam de formas bem semelhantes. Para recarregar o projeto ao alterar os arquivos HTML, precisei modificar o comando para observar esses arquivos também, o comando que rodo no terminal ficou assim então:

CompileDaemon --command="./marco_blog" -include=Makefile -include="*.django" -include="*.css" -include="*.js"
  
# Banco de dados

GORM é o ORM mais utilizado pela comunidade Go, possui diversas opções de queries, migrações e models, devido a isso, optei por utilizar ele para o projeto, foi bastante útil principalmente na query de posts da página inicial, onde existem diversos filtros dependendo da pesquisa.

Comecei o desenvolvimento utilizando Postgres rodando em um container docker local, mas sabia que não poderia usar ele para sempre, então comecei a pesquisar o banco de dados que usaria em produção, a primeira opção foi ver se existia alguma opção boa e barata na AWS, porém cotando os preços no RDS, estava muito fora do que queria gastar, o Aurora Serveless v2 parecia uma boa opção, porém o gasto mínimo de 0.5AGUs era muito alto para a proposta do site. Pensei então em migrar para DynamoDB, mas vi que precisaria fazer bastante alteração na minha aplicação, e pra ser sincero, nunca gostei muito do DynamoDB. Foi então conversando com um amigo que descobri o Planetscale (https://planetscale.com) que é uma plataforma que oferece um MySQL gratuito para hobbies, então no ORM, a única coisa seria trocar o drive de Postgres para MySQL, foi uma migração bem straight forward.

# CSS

Comecei o desenvolvimento utilizando bootstrap, estava ficando bem legal, porém queria tentar algo diferente, foi ai que pesquisando alguns design systems de empresas como Airbnb, IBM, Microsoft e Google, resolvi utilizar um que se baseia no design system do Duolingo. Porém ainda quero aprender TailwindCSS para utilizar no site e deixar com uma aparência mais única.

![](/images/20ff4c03-f72e-4361-9c6d-d15345d41897.webp)

# CRUD de posts

A primeira coisa que fiz, e que naturalmente imagino que todos começam por isso, foi fazer o crud da model principal do projeto, no caso os posts do blog, deixei o index do site como sendo a lista de posts, uma rota com o id do post para visualizar, e outra com o id do post porém para editar, utilizei esse cheat sheet para a padronização das rotas (https://devhints.io/rails-routes).

Decidi que o post deveria ter no mínimo esses campos: titulo, subtítulo, idioma, tags e body, as tags eu programei o Javascript manualmente para quando eu apertasse enter, ele salvava essa tag em um array e em seguida, renderizasse as tags contidas dentro desse array em formato de badges, ao realizar o post, ele então enviaria as tags separadas por virgula, sendo assim bem fácil de obter as tags na parte do backend.

O corpo do post foi complicado, pois não bastava só uma textarea, deveria ser um editor de textos com recursos mínimos necessários para a formatação do corpo, como por exemplo, colocar algo em negrito, itálico, titulo, lista, imagens, etc. Esses dias vi o Diego da Rocketseat recriando a interface do notion, e no vídeo, ele utiliza um editor pronto chamado Tiptap, como já uso o notion para anotações do dia a dia, gostei bastante e resolvi utilizar no projeto, mas percebi que no final, ele exporta um JSON com informações sobre cada linha do post, qual formatação, fonte, etc. Percebi que seria muito trabalhoso trabalhar com esse tipo de formato, então decidi procurar uma alternativa simples, que gerasse direto um HTML, foi ai que encontrei o Quill, outro editor opensource muito fácil de utilizar e bem customizável, decidi usar ele na aplicação.

Depois de fazer alguns testes de inserção e listagem de posts, vi que o resultado ficou muito próximo do que eu queria, só precisando de algumas tags CSS para formatação da visualização do post e logo já tinha uma versão alpha funcionando do blog, porém ainda estava longe de terminar.

# AWS S3

Resolvi fazer um teste diferente, o que aconteceria se eu fizesse o upload de uma imagem através do editor Quill? Fazendo o teste, vi que funcionou de primeira, opa, que maravilha! fechou essa parte do crud... certo?

Errado, acabei percebendo que o quill converte a imagem para uma tag HTML base64, com todos os bytes da imagem, sem tirar nem por, imagine uma imagem com múltiplos MBs em cada linha de uma tabela no banco de dados? imaginou? agora imagine também o usuário entrando na pagina principal do blog e toda vez ter que baixar essas imagens dos posts do zero, um caos total.

Ai percebi que precisaria realizar o upload dessas imagens em algum lugar, a primeira coisa que veio na cabeça foi utilizar o AWS S3 para isso, é bem fácil de configurar, bastando somente criar um usuário com permissão de escrita no bucket, e também criar um bucket com acesso publico para a internet.

Feito isso, agora precisei escrever um código no meu service la do Go para realizar o upload dessas imagens base64 para o bucket, e fazer o replace pelo link da imagem la no bucket publico do S3, foi muito bom pois assim consegui colocar em pratica a manipulação de slices utilizando Go.

![](/images/9a5cbb19-df6d-4b72-a17e-8437ebbdc2ea.webp)

# Cache

Para não sobrecarregar o banco de dados com request que raramente mudariam, somente quando eu fizesse uma atualização ou inserção de post, utilizei uma biblioteca chamada go-cache, no qual serve para guardar dados em memória e, com isso, obter esses dados caso precise muito mais rápido que realizar uma query no banco de dados. A proposta é muito semelhante a um Redis ou Memcached, porém muito mais simples pois tudo está em memória, não existe um serviço externo de cache, para o projeto que estou desenvolvendo, é mais do que suficiente.Claro que não poderia deixar o cache crescer infinitamente, então criei um middleware que, a cada requisição, remove os caches invalidados e caso o numero de itens no cache supere 15, ele limpa o cache inteiro, prevenindo assim o uso excessivo de memória da aplicação.

# ![](/images/9b2349b4-99ce-4d1a-9618-d2e4a87b490e.webp)

# Goroutines

As Goroutines são threads virtuais gerenciadas pelo próprio Go para que seja possível realizar múltiplas tarefas ao mesmo tempo, ou seja, ao invés de pesquisar pelos posts no banco de dados e depois pesquisar pelas tags de uma maneira síncrona, podemos utilizar as Goroutines para realizar essa tarefa assíncrona, como se fosse a api CompletableFuture no Java ou a de Promise no JavaScript.

Para criar uma Goroutine, basta colocar a palavra chave "go" antes de uma chamada de função, com isso, a função já roda na sua própria thread virtual, sem bloquear a thread principal, para uma explicação pratica, veja: https://go.dev/tour/concurrency/1

Caso seja necessário obter o valor de retorno da Goroutine, podemos utilizar os channels, são variáveis que criamos com o intuito de passar para dentro de uma ou mais Goroutines e então colocar valores nela com o intuito de obter depois da execução da Goroutine. No exemplo a seguir, mostro como utilizei Goroutines + Channels para rodar assincronamente três funções e obter o valor de cada uma delas:

![](/images/26e97979-793e-4660-abed-098d3186fa88.webp)

# Autenticação

Com essa questão do upload de imagens resolvido, agora seria necessário colocar algum tipo de autenticação no site para que somente eu conseguisse publicar postagens, para isso, utilizei meu velho conhecido token JWT, porém como estamos falando de uma aplicação MVC e não de uma app API Rest, o token JWT era guardado nos cookies 🍪 do navegador, fazendo isso, conseguimos prevenir um tipo bem conhecido de ataque chamado XSS (https://www.imperva.com/learn/application-security/cross-site-scripting-xss-attacks), no qual um hacker consegue injetar código JavaScript no site com o intuito de obter o token do usuário, como o token está guardado nos cookies 🍪, nenhum JavaScript consegue ter acesso a ele.

Como somente eu irei acessar o blog, achei desnecessário criar uma tabela "users" contendo somente meus dados, então resolvi guardar minhas informações de login e senha direto nas variáveis do ambiente do container, sendo assim bem seguro pois para ter acesso a esse tipo de informação, o hacker teria que obter acesso a onde o site esta hospedado, e se ele tem acesso a isso, o sistema inteiro já esta comprometido. A senha também está guardada em formato hash gerado pelo bcrypt, poderia ter utilizado também uma seed mas acho que não seria necessário, bom, espero que não.

Com o token guardado nos cookies 🍪, criei um middleware de autenticação para validar se o token é válido ou não, e com isso, se for válido ou não, eu guardo dentro de uma variavel booleana chamada "is\_auth" dentro dos locals (https://docs.gofiber.io/api/ctx/#locals), e com isso, consigo validar quais rotas necessitam ou não de autenticação.

![](/images/e1102a5a-a26c-4734-8b49-89b2952bfd98.webp)

# Deploy

Para fazer o deploy, queria algo simples e barato, pensei em utilizar o AWS Elastic Beanstalk, mas achei muito complicado de configurar, além de ter que configurar o code commit para funcionar o deploy automático ao realizar o push em uma branch no git, ai decidir usar o Heroku, porém achei muito caro, o plano eco custa 5$ por mes e a aplicação fica caindo se não existe atividade. Encontrei então uma alternativa muito boa chamada Railway, é igual um Heroku porém mais barato, e tem um free trial bem generoso de 5 dólares. foi só questão de dar permissão ao meu repositório git e o deploy aconteceu automaticamente utilizando docker, dentro da plataforma, posso configurar ainda as variáveis de ambiente do container, a rota de health check, comando de build/run, etc. Achei bem completo e recomendo bastante.

![](/images/bc257c3e-aed1-4b85-95f8-8478a440ebc7.webp)

# Vale a pena aprender Go?

SIM! Go com certeza se tornou uma das minhas linguagens favoritas depois desse projeto, a sua simplicidade, junto com sua performance tornam ela extremamente competitiva, principalmente no mundo corporativo, atualmente dominado pelo Java e pelo C#. Além do suporte que a linguagem tem com a AWS e Kafka, fazendo-a assim uma excelente escolha para empresas que buscam performance sem a necessidade de uma linguagem muito complexa como o Rust.

Agora creio que já tenho conhecimento o suficiente em Go para encarar qualquer projeto, até consegui utilizar uma goroutine para procurar as tags enquanto a busca por posts é realizada na página inicial.

Agora preciso escolher qual linguagem vou aprender no futuro, estão criando uma linguagem chamada V (https://vlang.io), que mistura elementos do Go e do Rust, estou com vontade de aprender, me parece ser bem simples, quem sabe não converto meu site para usar V no futuro?

Acho que isso é tudo pessoal, é um código que fiz em uma stack que não sou habituado e nunca utilizei em projetos reais, então certamente terá vários pontos a melhorar, caso tenha alguma dica, não se esqueça de dizer nos comentários 😉

O código do projeto se encontra no seguinte repositório GIT:

<https://github.com/marcoagpegoraro/marco%5Fblog>