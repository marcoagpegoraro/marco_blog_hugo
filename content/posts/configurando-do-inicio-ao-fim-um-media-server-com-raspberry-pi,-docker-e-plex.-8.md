+++
  author = "Marco Pegoraro"
  title = "Configurando do inicio ao fim um media server com Raspberry PI, Docker e Plex."
  date = "2022-01-04 00:00:00"
  description = ""
  tags = ["","DLNA","Docker","Linux","Plex","Raspberry Pi","Ubuntu"] 
+++
  
# ![](https://miro.medium.com/v2/resize:fit:1400/1*GsqI4vM5akhcVLgsq_8Fyw.jpeg)Introdução

Há tempos que tinha um Raspberry PI 3 aqui comigo porém nunca cheguei a utilizar ele para nada além de alguns testes para ver o funcionamento, pois bem, recentemente comprei uma smart TV, e nela, eu conseguia assistir meus filmes, series e animes pelo meu HD externo, foi ai que pensei na ideia, e se fosse possível utilizar meu Raspberry como um _media server_ e acessar meus vídeos diretamente pela TV utilizando minha rede local?

# Começo do projeto

Foi ai que comecei meu projeto para fazer meu pequeno servidor de media, o Raspberry eu já tinha, só faltava comprar o armazenamento dele e a fonte, optei por um cartão micro SDHC 1 de 32GB que já é mais do que suficiente para instalar o que eu preciso e uma fonte básica micro USB, lembrando que a fonte para o Raspberry deve ser de 5 Volts (V) e 3 Amperes (A). Feito isso, baixei o programa [Raspberry PI Imager](https://www.raspberrypi.com/software/) para gravar a imagem Linux no meu cartão SD, optei por instalar o Ubuntu Server na versão 20.04 LTS mas você poderia instalar a distro de sua preferência para esse projeto.

![](https://miro.medium.com/v2/resize:fit:1380/1*B5bpnaQIwewf35hu6vu2UQ.png)

O Raspberry Pi Imager é um programa que facilita muito instalar um sistema operacional em um cartão de memória

> Eu já tinha um Raspberry PI 3, mas se você for começar este projeto do zero, recomendo fortemente e investir mais um pouco em um Raspberry PI 4, além do ganho de performance, a versão 4 possui a entrada USB 3.0, o que faz o servidor de arquivos ter um ganho de velocidade muito maior que a do USB 2.0 que meu Raspberry PI 3 possui.

# Configuração do sistema operacional

Depois da distro instalada, o próximo passo foi colocar o cartão no slot do PI e conectar a fonte, ao ligar, ele vai pedir para realizar o login, no caso do Ubuntu, é "ubuntu" e senha "ubuntu", depois será necessário definir uma nova senha. A primeira coisa que eu gosto de fazer é deixar um IP fixo para meu Raspberry, para ser mais fácil acessar ele remotamente no futuro. Para isso, vou linkar as fontes dos materiais que achei na internet.

Como configurar um endereço de IP fixo no Ubuntu:

[https://linuxize.com/post/how-to-configure-static-ip-address-on-ubuntu-18-04](https://linuxize.com/post/how-to-configure-static-ip-address-on-ubuntu-18-04/)/

> Opcional (porem essencial): **SSH**

> Depois de configurar um IP estático para a maquina, o ideal é que você comece a acessar via SSH da sua maquina de uso normal, a praticidade é ordens de grandeza maior do que ficar com o Raspberry plugado a um monitor, teclado e mouse. Para utilizar o SSH, não tive que fazer nada pois já veio habilitado o SSH no meu Ubuntu, mas você pode seguir este tutorial para verificar se o seu está funcionando também:

> <https://linuxize.com/post/how-to-enable-ssh-on-ubuntu-20-04/>

Em seguida, como estou utilizando WIFI, eu precisei configurar mais uma parte para o Raspberry conseguir acessar a internet, mas eu recomendo fortemente que você utilize conexão via cabo, tanto pela performance quanto pela facilidade, eu só não utilizei pois não tenho um cabo Ethernet aqui comigo no momento (acontece):

<https://linuxconfig.org/ubuntu-20-04-connect-to-wifi-from-command-line>

Com a minha internet funcionando e com o IP fixo já configurado, o ultimo passo do setup do sistema operacional vai ser montar o drive toda vez que o PI ligar, para isso, precisei baixar alguns pacotes para ler o _filesystem_ exFAT do meu HD externo (se possível, utilize outro sistema de arquivos mais confiável, para mais detalhes [clique aqui](https://askubuntu.com/questions/319512/mount-exfat-drive-on-ubuntu-not-as-root)):

sudo apt-get install exfat-utils exfat-fuse
2023/10/20 23:02:51 stdout: 
Depois disso, editei meu arquivo /etc/fstab e coloquei a seguinte linha nele para montar meu HD toda vez que acontecer o boot do sistema:

/dev/sda1       /media/hd2tb    exfat   rw,async,umask=0 0 0
2023/10/20 23:02:51 stdout: 
# Instalação dos softwares

O próximo passo foi instalar o Docker para que possamos configurar o container do Plex Media Server, que no caso, será o programa responsável por fazer o streaming para a televisão:

## [How to Install Docker On Ubuntu 18.04 {2021 Tutorial}](https://phoenixnap.com/kb/how-to-install-docker-on-ubuntu-18-04?source=post%5Fpage-----3c9b84a9dda5--------------------------------)

### [Docker is an increasingly popular software package that creates a container for application development. Developing in…](https://phoenixnap.com/kb/how-to-install-docker-on-ubuntu-18-04?source=post%5Fpage-----3c9b84a9dda5--------------------------------)

[phoenixnap.com](https://phoenixnap.com/kb/how-to-install-docker-on-ubuntu-18-04?source=post%5Fpage-----3c9b84a9dda5--------------------------------)

  
Com o docker instalado na maquina, será necessário criar o container do Plex, pra isso, existe uma imagem já pronta feita pelo [LinuxServer](https://www.linuxserver.io/):

<https://hub.docker.com/r/linuxserver/plex>

Eu criei o container utilizando o comando "docker run", porém você também pode optar por utilizar o Dockerfile, fica ao seu critério, os únicos valores que eu alterei foram estes:

-v /path/to/library:/config -v /path/to/tvseries:/tv -v /path/to/movies:/movies 
2023/10/20 23:02:51 stdout: 
Coloquei o caminho no qual está meus vídeos. No caso "/media/hd2tb/Videos".

Com o container do Plex funcionando, você já poderá acessar o servidor do Plex via navegador, é só digitar o endereço IP do seu Raspberry PI, na porta 32400, no meu caso, ficou "<http://192.168.0.100:32400/web/index.html#!/>"

Depois disso, você só precisará fazer as configurações que o Plex necessita, apontar onde está sua pasta de vídeos e seus vídeos já estarão disponíveis para seus clientes Plex.

![](https://miro.medium.com/v2/resize:fit:1400/1*t9h0CGyK6XrUckan6VjsMg.jpeg)

Assistindo Akira diretamente da minha televisão utilizando o Plex.

# Extra - configurando um servidor Samba para gerenciamento dos arquivos:

Agora com seus vídeos conectados no Raspberry, o ideal seria que nós conseguíssemos editar os arquivos a vontade, certo? para isso, existem diversas maneiras, mas a minha favorita é compartilhar o HD externo na rede local para qualquer computador com acesso consiga acessar e fazer modificações diretamente no drive, para isso, vamos utilizar o Samba para compartilhar nosso drive na rede.

Existe um tutorial bem fácil para instalar e configurar o Samba na própria documentação do Ubuntu:

<https://ubuntu.com/tutorials/install-and-configure-samba>

Só que ao invés de criar uma pasta nova, você irá compartilhar seu drive, no meu caso, compartilhei o caminho "/media/hd2tb", depois de configurar o caminho e os usuários samba, só nos resta testar utilizando outro computador na rede, utilizei um computador macOS, mas poderia ser qualquer sistema operacional

![](https://miro.medium.com/v2/resize:fit:1400/1*iNDbly8zpgjdfEluPrvM_w.png)

Podemos ver que agora nós conseguimos acessar e editar ao nosso gosto nossa pasta de vídeos do Plex de uma maneira fácil e intuitiva.

E com isso, termino meu tutorial, uma das vantagens de usar o Docker é a facilidade de subir containers sem atrapalhar os demais containers que já estão em funcionamento, no futuro planejo subir um container com o [Pi-hole](https://pi-hole.net/) para bloquear os anúncios na minha rede interna, e quem sabe até um [Rancher](https://rancher.com/) para monitorar meus containers, mas isso fica para outro tutorial, se gostaram, não deixem de bater palmas para o artigo e compartilhem com quem você acha que esse artigo seria útil, fico por aqui, até mais!

## Miscellaneous:

Na primeira tentativa ao realizar este projeto, eu fiz não utilizando o Docker nem o Plex, para configurar o servidor de media, eu utilizei um programa chamado MiniDLNA, no qual tem um funcionamento bem semelhante porem muito mais simples e direto, fica ai esta alternativa para quem quiser instalar também, o protocolo DLNA é reconhecido pela maioria das TVs atuais, tanto que a minha reconheceu na primeira tentativa, vou estar deixando um tutorial muito interessante que encontrei no Youtube sobre como instalar o programa:

  
Referencias:

<https://askubuntu.com/questions/319512/mount-exfat-drive-on-ubuntu-not-as-root>