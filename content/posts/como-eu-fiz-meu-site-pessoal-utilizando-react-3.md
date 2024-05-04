+++
  author = "Marco Pegoraro"
  title = "Como eu fiz meu site pessoal utilizando React"
  date = "2023-07-30 00:00:00"
  description = "Utilizando Github Pages, Material Ui Design, animações e i18n"
  tags = ["Github Pages","i18n","Material UI","React"] 
  header_image = "/images/1aIBlqgTuJH5IhAd5rWAe_A.png"
+++
  
# ![](/images/1aIBlqgTuJH5IhAd5rWAe_A.png)Introdução

Com a facilidade das redes sociais, fica muito facil termos como criar um perfil no qual podemos inserir quaisquer informaçoes sobre nós mesmos, por exemplo, no Instagram existe a opção de criar _highliths_ com momentos importantes da nossa vida agrupadamente, como uma viagem ao exterior ou com as primeiras fotos de nossos filhos.

No ambito profissional, temos o Linkedin, sendo possível colocar diversas informações sobre nossa carreira, como empregos, projetos, certificados, projetos pessoais, etc. As possibilidades de interatividade crescem exponencialmente, fica muito mais fácil encontrar pessoas que trabalham na mesma empresa que você ou com a mesma area de atuação.

Porém nem sempre foi assim, antes do advento das redes sociais, pessoas que quisessem mostrar seu trabalho na internet, sendo elas fisicas ou juridicas, precisavam de um endereço na WWW, isso passava muito mais credibilidade para o cliente, hoje em dia isso se perdeu, porém ainda pode ser um fator decisivo para conquistar o cliente.

Se imagine como um cliente que, ao decidir contratar um serviço, encontra duas empresas na qual as duas oferecem serviços semelhantes, tem o mesmo tempo de mercado e preços iguais, porém uma tem um site profissional com tudo o que a empresa pode oferecer e outra só tem redes sociais, pode ser que isso não seja um fator decisivo, mas a empresa que possui o site com certeza passaria mais credibilidade.

# Vamos começar!

Há três anos atras, fiz meu primeiro site pessoal utilizando Angular 9, porém ele estava bem primitivo, eu sabia que podia melhorar, então optei por utilizar a plataforma do Github chamada Pages, é um serviço gratuito que toda pessoa com uma conta Github pode usar, com ele você consegue criar um repositório com o seu "username" e terminando com o final ".github.io" e o arquivo index.html que estiver nele, o github vai subir automaticamente para a web no endereço do repositorio que você criou, esse link explica com mais detalhes: [https://pages.github.com](https://pages.github.com/)

No meu caso, criei um repositório chamado _marcoagpegoraro.github.io_, Com isso, você já conseguiria programar o site da maneira que você desejar, podendo inclusive colocar avontate arquivos .css e .js para seu index.html consumir, mas para quem quer demonstrar que conhece algum tipo de framework frontend, como por exemplo, React, Vue, Angular, etc, fica interessante programar o site em algum framework e publicar o código na branch main do repositorio para o código ser servido automaticamente, foi isso que eu fiz, no meu caso, eu tenho experiencia com o framework Angular, porém decidi programar meu site utilizando React, poís é a tecnologia mais utilizada atualmente no frontend.

# Como programar utilizando o React e exportar o build para o Github Pages?

Ai que fica interessante, o Github Pages leva em conta o que está na branch main, porém qualquer outra branch é ignorada pelo mesmo, então você poderia criar uma outra branch para o código da sua aplicação, por exemplo, uma branch chamada "develop" ou "react", e quando quiser realizar um deploy para o site em produção, fazer o build da aplicação, como por exemplo, utilizando o comando "npm run build — prod", e pegar o conteudo da pasta "build" gerada e colocar este código na branch main, porém isso seria muito trabalhoso.

Mas e se tivesse algum comando que faça isso automaticamente? Procurando eu ví que existia já um pacote chamado [gh-pages](https://www.npmjs.com/package/gh-pages) que faz exatamente isso, facilitando demais o processo de deploy, então instalei o pacote na minha aplicação e coloquei o seguinte script no meu package.json

> “deploy”: “gh-pages -d build -b main”,

Com isso, sempre que eu quiser realizar um deploy, é só eu rodar o comando "npm run deploy" que o pacote já faz o deploy pra mim, muito fácil e prático.

# Como programei o meu site?

Todo o código do meu site está disponível neste repositório:

[https://github.com/marcoagpegoraro/marcoagpegoraro.github.io](https://github.com/marcoagpegoraro/marcoagpegoraro.github.io/blob/develop/package.json)

Primeiramente, criei um novo projeto React na branch develop, importei a lib do [gh-pages](https://www.npmjs.com/package/gh-pages) e fiz um teste pra saber se funcionou o deploy automaticamente, feito isso, comecei o desenvolvimento do site.

A primeira coisa que fiz foi pesquisar algum framework CSS para utilizar na aplicação, pensei em utilizar [Bootstrap](https://react-bootstrap.netlify.app/), acho uma biblioteca incrível que agiliza demais o desenvolvimento, principalmente para sistemas internos de empresas, mas acho que peca muito no visual, o que não é algo que queremos em um site pessoal, pensando nisso, pesquisei e ví que tinha uma chamada [Material UI React (MUI)](https://mui.com/), vi que os componentes utilizam como base o material design da google, e que diversas empresas utilizam, então optei por usar ele.

# Páginas

## Apresentação

Optei por deixar o site bem simples e direto, sem roteamento, somente utilizando scroll para baixo, então com isso em mente, começei o desenvolvimento do app. Criei primeiramente uma pasta chamada Pages, na qual teria todas as "páginas" do site, ou seja, a cada 100 [view height](https://www.sitepoint.com/css-viewport-units-quick-start/), seria uma página nova.

A primeira página do site contem somente uma [AppBar](https://mui.com/material-ui/api/app-bar/) com links para minhas redes sociais, posteriormente, coloquei um input [select](https://mui.com/material-ui/react-select/) com as linguagens disponíveis no site e um botão para alterar o tema do site.

![](/images/1FPdVSzeQwLSJk6w6menf-A.png)

É bom sempre adotar as melhores práticas de componentização, por exemplo, os icones de redes sociais eu utilizo tanto na AppBar quanto no Footer do site, e como está componentizado, só preciso importar o mesmo componente nessas partes do código.

Para deixar centralizado, utilizei as propriedades de [flex](https://css-tricks.com/snippets/css/a-guide-to-flexbox/) do CSS, e alinhei todo o conteudo no centro, é uma habilidade primordial aprender como funciona pra quem quer trabalhar como desenvolvedor frontend, basicamente coloquei o display da página como flex e pedi para alinhar os itens ao centro:

.presentation-page {
    display: flex;
    align-items: center;
    flex-direction: column;
    height: 100vh;
    flex-wrap: wrap;
    animation: fadeIn 3s;
}
## Sobre mim

Em seguida, temos a página na qual escrevi um texto com um mini resumo da minha vída, separei em três paragrafos principais, caso queira se basear, o primeiro paragrafo é uma pequena introdução sobre mim e onde trabalho atualmente. O segundo paragrafo é algo mais tecnico, para demonstrar quais habilidades eu sei e quais que já trabalhei e, por tanto, tenho afinidade. Por fim, escrevi um ultimo paragrafo para "quebrar o gelo" e falar sobre meus hobbies e coisas da minha vida pessoal, afinal, i'm only human, after all.

![](/images/1ih26j9TcviKBIEjWn6IJfg.png)

## Experiencia profissional

Aqui eu pensei em deixar minha experiencia profissional, bem semelhante a aquela seção que temos no perfil do linkedin, porém queria deixar com um visual criativo que remetesse a algo que gosto.

![](/images/176ansUJaCSvsVCDIL7YkkQ.png)

Por ai sempre vejo os tutoriais utilizarem a janela do macOS como frame para códigos, com aqueles três botões: vermelho, amarelo e verde, decidi fazer o mesmo mas achei que ficou muito genérico, então tive a ideia de programar o estilo da janela do Windows XP, já que gosto bastante de tecnologias antigas, foi o primeiro sistema operacional que tive, porém iria dar bastante trabalho, então optei por fazer a janela do Windows 98, então com um pouco de CSS, ficou pronto do jeito que eu queria, dentro da janela, decidi deixar o visual do material design mesmo para facilitar a navegação e leitura de quem estiver lendo, imagine aqueles [chips](https://mui.com/material-ui/react-chip/) com o visual do Windows 98, todo quadrado e sem icones, não iria ficar tão legal, né?

## Portifolio (Projetos)

Aqui eu dediquei uma area para projetos e coisas legais que já fiz ou conquistei, como por exemplo, certificados, projetos, apresentações e artigos.

![](/images/1xdEDLcT2rPXVFr-Svm90Gw.png)

No futuro gostaria de separar os projetos em uma página especifica, o problema é que não tenho muitos projetos rs, poís a maioria dos códigos que já escrevi na vida foram códigos para a empresa onde trabalho e empresas que já trabalhei, e códigos de empresas são das empresas (e permanecerá assim), então ainda preciso dedicar alguns finais de semana para fazer [POCs](https://www.techtarget.com/searchcio/definition/proof-of-concept-POC) sobre tecnologias ou frameworks nas quais tenho interesse em aprender, e, em seguida, fazer uma página só para esses projetos.

Decidi utilizar [cards](https://mui.com/material-ui/react-card/) para cada projeto, fica uma visualização bem fácil, ainda mais com imagens.

## Contato

Para finalizar, fiz uma página de contato bem simples, com o titulo ao lado esquerdo e um campo de texto a direita com dois botões de mensagem, um para envio de mensagem por Whatsapp e outro para Email, é bem mais fácil do que parece fazer esses botões funcionarem, o do Whatsapp é só criar um [link customizado](https://faq.whatsapp.com/5913398998672934) com o número que deseja mandar a mensagem e o conteudo e o navegador já abre direto em uma conversa comigo e a mensagem digitada, o do e-mail é só fazer um link [mail:to](https://www.simplilearn.com/tutorials/html-tutorial/html-mailto).

Abaixo temos o footer do site, deixei um texto sobre as tecnologias utilizadas no site e os links das mesmas, também deixei o link do repositório para caso alguem queira ver.

![](/images/1ENjI9a25gvZNXo-2x17oBg.png)

A direita deixei os mesmos links de redes sociais que tem na app bar, graças a componentização, foi só importar o mesmo componente nas duas partes do site.

# Suporte a telas de celular

Depois que programei o site, foi necessário corrigir alguns problemas quando o site era apresentado em uma tela de celular, para isso utilizei [media queries](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS%5Fmedia%5Fqueries/Using%5Fmedia%5Fqueries) para alterar o tamanho de algumas partes do site, ou até mesmo removelas por completo,

Na app bar, quando o tamanho da tela é menor que um tamanho especifico, eu troco de lugar o componente de icones de redes sociais com os botões de troca de idioma e dark mode, para a visualização ficar mais agradavel.

![](/images/18gsUJWF4XV9wzr16X_kxkQ.png)

Na tela de apresentação, eu ocultei completamente minha foto apresentando pois não haveria espaço para ela, então eu coloquei um media query exatamente no ponto na qual a imagem quebraria para a linha de baixo e faço ela ter um display: none.

Uma página que precisei alterar bastante o design foi a de experiências profissionais, não teria espaço para uma sidebar ao lado, então eu ocultei ela por completo, então tive a ideia de substituir ela por uma ["File menu bar"](https://en.wikipedia.org/wiki/File%5Fmenu) logo abaixo da ["title bar"](https://learn.microsoft.com/en-us/windows/apps/develop/title-bar?tabs=wasdk), com um select para a pessoa escolher qual informação deseja visualizar, ficou bem fácil a adaptação, já que os componentes do conteudo se ajustaram automaticamente.

![](/images/1EKStUfbzaVxWUhfwV_OWpw.png)

As demais páginas não precisaram de nenhum ajuste muito especifico, somente ajustar o tamanho quando a tela diminuia foi o suficiente para caber tudo na mesma tela.

![](/images/1khLS8NKmcNakJGO1racVNA.png)

# Animações

Gostaria de deixar o site mais agradavel aos olhos, para isso, utilizei também algumas animações CSS, na página inicial, utilizei uma simples animação de [fade in](https://blog.hubspot.com/website/css-fade-in), já deixou o site com uma cara bem profissional.

![](/images/1SzDxG8t3NePwhoyryBkfVg.gif)

Para as demais páginas (exceto o footer), utilizei uma classe do Javascript chamada [Intersection Observer](https://www.youtube.com/watch?v=T33NN%5FpPeNI), com ele, é possível saber se o usuario está visualizando ou não determinado elemento HTML, e com isso, realizar alguma logica desejada, é muito utilizado pra fazer um scroll infinito, como o do Facebook por exemplo, ou em animações quando o usuário passa por uma parte do site, igual acontece no meu caso.

Para o React, existe uma implementação chamada [react-intersection-observer](https://www.npmjs.com/package/react-intersection-observer), utilizei ela e o código ficou assim:

import { useInView } from 'react-intersection-observer';
export const Page = () => {
  const { ref, inView, entry } = useInView({})
  const [isAnimatedAlready, setIsAnimatedAlready] = useState(false)
  if(inView && !isAnimatedAlready)
    setIsAnimatedAlready(true)
  
  return <div ref={ref} className={`about-me-page 
${isAnimatedAlready ? 'animation-in' : 'animation-out'}`}>
<!--content-->
</div>
Precisei utilizar uma variavel isAnimatedAlready pra gravar se o usuário já passou pela página para ela ser animada só na primeira vez, caso eu quisesse animar todas as vezes, seria necessário só remover essa variavel.

![](/images/141dPGvM6f746nh_Bx9fnXQ.gif)

# Suporte a outros idiomas

Gostaria que o site estivesse disponível em outras linguas alem do portugues também, para isso, utilizei a biblioteca i18n, na qual possúi suporte a diversas linguas, para isso, utilizei como base o artigo Como adicionar internacionalização (i18n) no React do [Lucas Pinheiro](https://medium.com/@lucas%5Fpinheiro), basicamente é necessário criar um arquivo para cada idioma que deseja adicionar, no meu caso, en-US.js, pt-BR.js e it-IT.js, e dentro deles, exportar um objeto com chave sendo o que o HTML vai procurar e o valor que seria o texto da linguagem selecionada.

//Ingles (en-US.js) 
presentation: {
  title: `Fullstack Software Developer | AWS | Java | JavaScript`
},
//Portugues (pt-BR.js) 
presentation: {
  title: `Desenvolvedor de software Fullstack | AWS | Java | JavaScript`
},
//Italiano (it-IT.js)
presentation: {
  title: `Sviluppatore Software Fullstack | AWS | Java | JavaScript`
},
Para selecionar a linguagem, criei na app bar um select de linguagens, quando alterado, ele pede para o i18n alterar qual arquivo utilizar para o idioma do site, o legal é que fica salvo no local storage qual foi a linguagem escolhida, fica mais ou menos assim a implementação:

import { useTranslation } from 'react-i18next'

export const SelectLanguageInput = () => {
  const { i18n } = useTranslation()
  const [webSiteLanguage, setWebSiteLanguage] = React.useState(i18n.language)

  const handleLanguageChange = (event: SelectChangeEvent) => {
    setWebSiteLanguage(event.target.value)
    i18n.changeLanguage(event.target.value)
  }

  return <FormControl size="small">
      <InputLabel id="Language">Language</InputLabel>
      <Select
        labelId="Language"
        id="Language"
        value={webSiteLanguage}
        label="Language"
        onChange={handleLanguageChange}
      >
        <MenuItem value={'en-US'}>🇺🇸 English</MenuItem>
        <MenuItem value={'pt-BR'}>🇧🇷 Português</MenuItem>
        <MenuItem value={'it-IT'}>🇮🇹 Italiano</MenuItem>
      </Select>
    </FormControl>
}
Para mais informações, recomendo bastante o artigo do Lucas Pinheiro indicado logo acima.

# Tema claro e escuro

Para finalizar, o MUI possui a possiblidade de implantar [tema escuro](https://mui.com/material-ui/customization/dark-mode/), o famoso Dark Theme que programadores tanto amam (e uns nem tanto), pra isso, foi só seguir a documentação do framwork que rapidamente já estava com algo funcional no site, é necessário verificar qual tema o usuário prefere, light ou dark, gravar o estado dessa variavel e colocar todos os componentes do site sendo filhos do _ThemeProvider_, ficou assim:


import React from 'react';
import { NavBar } from './components/NavBar';
import { AppBar, Box, Button, Card, CardContent, CardMedia, CssBaseline, Fab, useTheme, ThemeProvider, Toolbar, Typography, createTheme, useMediaQuery } from '@mui/material';
import { themes } from './config/themes';

const ColorModeContext = React.createContext({ toggleColorMode: () => { } });

function App() {
  const prefersDarkMode = useMediaQuery('(prefers-color-scheme: dark)');

  const [mode, setMode] = React.useState<'light' | 'dark'>(prefersDarkMode ? 'dark' : 'light',);
  const colorMode = React.useMemo(
    () => ({
      toggleColorMode: () => {
        setMode((prevMode) => (prevMode === 'light' ? 'dark' : 'light'));
      },
    }),
    [],
  );

  const theme = React.useMemo(
    () => createTheme({ palette: {
          mode,
          ...(themes(mode)),
        },
      }),
    [mode],
  );

  return (
    <ColorModeContext.Provider value={colorMode}>
      <ThemeProvider theme={theme}>
        <CssBaseline />
        <div className="app">
          <NavBar colorModeContext={ColorModeContext} />
          <div className='app-content'>
            <!-- content -->
          </div>
          <Footer />
        </div>
      </ThemeProvider>
    </ColorModeContext.Provider>
  );
}

export default App;
# Dominio customizado

E foi assim que fiz meu site, pra finalizar, contratei o serviço de dominio do registro.br, para o site ficar com .com.br ao invés de .github.io, caso tenha interesse, leia este outro artigo que escrevi sobre o assunto:

## [Como fazer um site no Github pages ter um dominio registrado no registro.br](https://medium.com/@tete5423/como-fazer-um-site-no-github-pages-ter-um-dominio-registrado-no-registro-br-872482317901?source=post%5Fpage-----88c12bca47d0--------------------------------)

### [Configurando tanto o registro.br quanto o Github pages](https://medium.com/@tete5423/como-fazer-um-site-no-github-pages-ter-um-dominio-registrado-no-registro-br-872482317901?source=post%5Fpage-----88c12bca47d0--------------------------------)

[medium.com](https://medium.com/@tete5423/como-fazer-um-site-no-github-pages-ter-um-dominio-registrado-no-registro-br-872482317901?source=post%5Fpage-----88c12bca47d0--------------------------------)

  
# Resultado final

Você pode verificar tanto o código do projeto quanto o resultado nesses links a seguir:

[https://www.marcoagpegoraro.com.br](https://marcoagpegoraro.com.br/)

<https://github.com/marcoagpegoraro/marcoagpegoraro.github.io>

  
É isso pessoal, até a proxima!

  
Da pra fazer com qualquer framework frontend!