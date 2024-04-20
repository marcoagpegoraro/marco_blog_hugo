+++
  author = "Marco Pegoraro"
  title = "Git: Aumentando sua produtividade utilizando alias nos comandos"
  date = "2024-01-24 01:32:29.516000000"
  description = "Tempo é dinheiro!"
  tags = ["Bash","Git","Terminal"] 
+++
  
Hoje estava, como de costume, trabalhando em uma feature no qual eu fazia recorrentes commits e pushs, até que percebi o tanto de tempo gasto na minha vida só para digitar os mesmos comandos todas as vezes, ai que pensei se não existe uma maneira de deixar os comandos mais simples, fui pesquisar e encontrei que existe o comando "alias" no bash para criar um link de um comando para outro, por exemplo, podemos criar uma alias para o comando git push ser executado se rodarmos o comando "gp" (git push) da seguinte maneira:

alias gp="git push"
Fiz o teste e deu certo! Com certeza se tivesse feito isso antes, economizaria preciosos segundos na minha vida.

Para deixar esse comando salvo para não precisar sempre criar o alias, podemos incluir esse comando direto no arquivo .bashrc (caso você esteja utilizando o interpretador de comandos Bash), no meu caso, como utilizo o zsh, preciso alterar o .zshrc.

Também criei um alias para outro comando que utilizo muito, que é o "git add .":

alias ga="git add ."
Para esses comando simples, o alias já é bastante útil, porém existem casos que queremos passar um argumento para o comando git, como por exemplo, o nome de um commit ou o nome da branch remote, para esses casos, podemos criar uma função bash:

gc () 
{
  git commit -m $1
}
Agora com o simples comando "gc "Alterado função que busca usuário"", consigo realizar um commit sem ter que digitar toda vez "git commit -m", legal né?

Outro comando que de vez em quando utilizo é quando eu faço um commit sem querer e tenho que desfazer ele, para isso, criei um alias para voltar um commit e manter as alterações que fiz:

alias gr="git reset --soft HEAD~1"
Por fim, criei outra função que geralmente utilizo quando estou subindo uma branch nova e preciso informar a origem :

gpo () 
{
  git push -u origin $1
}
Como gostaria de ter pensado nessa ideia antes 😅

Se quiser utilizar esses atalhos no seu workflow, copie as seguintes linhas abaixo, cole no seu .bashrc e economize tempo você também:

alias ll="ls -la"
alias ga="git add ."
alias gp="git push"
alias gr="git reset --soft HEAD~1"
gc ()
{
  git commit -m $1
}
gpo ()
{
  git push -u origin $1
}
![](https://marco-blog-post-images.s3.sa-east-1.amazonaws.com/ea7dc9ec-d008-4624-bb0c-764f32ddad2c.jpg)

Referências:

https://linuxize.com/post/how-to-create-bash-aliases

Acessado em 23 de Janeiro de 2024