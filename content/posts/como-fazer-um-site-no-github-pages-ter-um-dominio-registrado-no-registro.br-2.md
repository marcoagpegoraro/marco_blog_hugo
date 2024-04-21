+++
  author = "Marco Pegoraro"
  title = "Como fazer um site no Github pages ter um dominio registrado no registro.br"
  date = "2023-07-30 00:00:00"
  description = "Configurando tanto o registro.br e o Github pages"
  tags = ["DNS","Github Pages","Registro.br"] 
  header_image = "/images/1*w1-0p7xElUKPEw_ILNJvZA.png"
+++
  
  
![](/images/1*w1-0p7xElUKPEw_ILNJvZA.png)

# Introdução

Se você caiu neste artigo, certamente tem uma página no Github pages e gostaria de alterar a URL ".github.io" para uma URL ".br", vamos combinar que fica bem mais profissional ter somente o nome da sua marca ou o seu nome no site, ao invés de ter o nome + github, esse artigo veio para te ajudar.

O registro.br é o lugar que gerencia os dominios ".br", então qualquer site que você ver com esse dominio, pode ter certeza que tem o registro.br por trás.

# A principal vantagem de contratar um dominio no registro.br

Quando você contrata um dominio no registro.br, você esta solicitando diretamente para o orgão responsável por dominios no Brasil, resultando assim em um custo menor comparado a contratar com serviços de hospedagens de site, pois eles vão fazer o serviço de contratar no registro.br por você, claro, por um preço.

Pode até ser que faça sentido para quem quer gerenciar tudo na mesma plataforma, digamos, um sistema PHP com MySQL e dominio em alguma hospedagem de site ficaria de fato mais fácil gerenciar se tudo estiver no mesmo lugar, porém para nós que já temos o site hospedado no Github pages, é bem mais vantajoso ir direto ao registro.br.

# Como registrar um dominio?

Primeiramente, você precisará entrar no registro.br e criar uma conta, é um processo bem simples e rapido. Em seguida, clique em registrar logo acima

![](/images/1*Uqa6YvhvCQF6iZZtuyvV9w.png)

Nesta tela, você deverá digitar o dominio que deseja e o site te mostrará se está disponível ou não, ao validar que o dominio desejado está disponível, aparecerá a opção de registrar.

![](/images/1*7ax0hw6ffF0TVtL9AuxU7Q.png)

Na tela de registrar, você terá que preencher algumas informações pessoais e concluir o pagamento, realizando o pagamento, seu dominio estará disponível em poucas horas, no meu caso não demorou nem uma hora e o dominio já estava publicado.

# Apontando o dominio para o Github pages

Para validar se seu dominio está valido, logue na sua conta do registro.br e clique em "painel" lá encima, após isso, você será redirecionado para os seus dominios, seu dominio deve estar com o status "**Publicado"**, após validar, clique no seu dominio.

Lá embaixo, você verá uma seção DNS, clique nela, se tiver uma opção "modo avançado", alguma coisa assim, ative-a, em seguida, clique em configurar zona DNS, aparecerá uma tela assim, porém a sua lista estará vazia:

![](/images/1*tiz5qFELwF5nvnOAzEcKNw.png)

Agora é só clicar em nova entrada e adicionar igual está no meu, exceto que você vai substituir conforme a imagem.

Feito isso, salve as alterações e vamos agora la pro Github configurar ele.

# Configurando o Github pages

No repositório do seu Github pages, clique em "Settings"

![](/images/1*LwVUF9Zk3KLPcNYrGIRVnA.png)

Em seguida, clique em "Pages"

![](/images/1*kmBQohuMeEQtoiJ_l0vvSg.png)

Agora salve essa configuração, com isso, o Github criará um arquivo CNAME na branch de deploy do seu projeto com o endereço do seu site, nunca apague este arquivo pois o Github não saberá como encontrar seu site.

![](/images/1*df1lZZJ9Efx8JClSf2CVTw.png)

Estou utilizando React em outra branch, e utilizo uma lib chamada gh-pages para realizar o deploy, ela gera o build e joga os arquivos na branch master, e com isso, ficava toda vez sobrescrevendo esse arquivo, fazendo meu site parar de funcionar, demorou um certo tempo pra eu perceber que era esse o problema. Então caso esse seja seu caso, coloque o arquivo CNAME com o endereço do site na pasta “public” do seu projeto, para que o build sempre tenha esse arquivo na pasta raiz.

E com isso, finalizamos esse tutorial, espero que tenha te ajudado, até a proxima!