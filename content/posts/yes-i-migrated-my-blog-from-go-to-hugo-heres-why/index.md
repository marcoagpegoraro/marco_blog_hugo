---
  author: Marco Pegoraro
  title: Yes, i migrated my blog coded in Go to Hugo, here's why
  date: 2024-04-28 13:30:00
  description: ""
  tags: ["Go","Hugo"] 
  header_image: https://gohugo.io/images/hugo-logo-wide.svg
---
## Introduction

A while ago i wrote a [post explaining how i created my blog using Golang](https://marcoagpegoraro.com.br/posts/como-aprendi-a-linguagem-de-programa%C3%A7%C3%A3o-go-e-utilizei-a-mesma-para-programar-meu-blog-pessoal-15), and how incredible was the experience, but now, six months latter, i migrated my blog to [Hugo](https://gohugo.io/), an opensource framework coded in Go to create static sites.

Not that i was unsatisfied with my blog project in Go, in fact, it had a lot of things that i would like to improve overtime, for example, configure GitHub Actions to automatically deploy in AWS, or a VPC. The possibility to have multiple users, change the default template engine from django to [templ](https://templ.guide/), start using [htmx](https://htmx.org/), etc.

But then i realized that this would not be the the best approach to the problem that i was trying to solve, i just need a simple static page blog, i don't need a database, an instance running, a logging service, i don't need any of that. 

And on top of that, the [railway](https://railway.app/) free tier was ending, it started with 5 dollars and ended with 1.97, not bad for a service that was running non stop for 6 months.
![railway freetier showing 1.97USD left](./railway-freetier.png)
And the nail on the coffin was the fact that [Planet Scale](https://planetscale.com/) was ending the [free tier plan](https://dev.to/lukeecart/planet-scale-is-removing-free-tier-2f17), which is where my database was hosted.

## Sunsetting my old blog

I even thought about migrating the database to DynamoDB, but i think it was time to search for another solution all together, because this blog is a side project, i don't make any money with it, i don't plan to put ads in the website, at least for now, so i searched alternatives.

One of then was migrate my blog to Jekyll, but i didn't like the installation process, and the themes required to download a folder, instead of putting the themes folder in your project.

Then i found Hugo, a fast static site generator build with Go, the installation was super simple and the documentation is very complete and straightforward, and the community is very active, i had the necessity of customized a feel aspects of my blog and every question that i have, the community already answered in the [official forum](https://discourse.gohugo.io).

I choose the [risotto theme](https://github.com/joeroe/risotto), is a minimalist theme inspired by terminals, i really like the aesthetics of this theme.

## Migrating the posts from SQL to Markdown

So there was one problem left, i needed to migrate every post from my old mySQL database. For those who don't know, hugo uses markdown files for pages, so i needed to download the content of the old database and then convert to markdown file.

Planet scale graciously put an button to wake the free tier database for a limited period of time for people make the database backup, so i did just that and did the backup using DBeaver, it has an option to save the results of a query in multiple file formats, such as json, csv and "insert query".

I then downloaded all my posts table and my tags table in the json format. All the posts was using raw html, so i needed to convert html to markdown, and then save the results in a file, i then wrote the following javascript script to do just that: 

```js
const fs = require('node:fs');
const { NodeHtmlMarkdown, NodeHtmlMarkdownOptions } = require('node-html-markdown')

const nhm = new NodeHtmlMarkdown();

fs.readFile('./posts.json', 'utf8', (err, data) => {
  if (err) {
    console.error(err);
    return;
  }

  const dataJson = JSON.parse(data)
  const posts = dataJson.posts

  for(let i = 0; i < posts.length; i++){    
    if (posts[i].is_draft == 1) continue;
    let content = nhm.translate(/* html */ posts[i].body)

    content = generateHeader(posts[i]) + content

    try {
      const postname=posts[i].title.toLowerCase().replaceAll(" ", "-")
      const filename = `./posts/${postname}-${posts[i].id}.md`
        fs.writeFileSync(filename, content);
        console.log("converted post: " + filename)
      } catch (err) {
      console.error(err);
    }    
  }
});

function generateHeader(post){
  return `+++
  author = "Marco Pegoraro"
  title = "${post.title}"
  date = "${post.publicated_at ?? post.created_at}"
  description = "${post.subtitle}"
  tags = ${getTags(post)} 
+++
  
`
}

function getTags(post){
  const data = fs.readFileSync('./posts_tags.json',
    { encoding: 'utf8', flag: 'r' });
    let tagPostList = []
    
    const dataJson = JSON.parse(data)
    const tags = dataJson.tags
    for(let i = 0; i < tags.length; i++){
      if (tags.tag_name == "") continue;
      if(post.id == tags[i].post_id){
        tagPostList.push(tags[i].tag_name)
      }     
    }
    return JSON.stringify(tagPostList)
}
```

As you can see, with this script i use most of the columns of the database, such as post.title, post.publicated_at, post.body, etc. 

First, i open the file that contains all the posts in the database, then i parse to JSON and i iterate through the post array, inside the loop, i check if is a draft (is draft = 1, is not draft = 0), if it's not, i begin to convert the file content from html to markdown using a node library called [node-html-markdown](https://www.npmjs.com/package/node-html-markdown).

Then i use the generateHeader function to insert the header of the markdown file with information related to the post, such as creation time, title, tags and description.

Inside the generateHeader function, i call another function called generateTags, it is a function that open the file that have all the tags and then compare the post id in the two tables to fill the tags field.

And that's how i converted all my files to Markdown.

## Removing the images from S3

Now there was only one thing left to do, removing all my images hosted at Amazon S3 and store inside the repository, i also wrote a script to do that, but i was so tired that i manually opened all my posts and right clicked in the images and saved each one of then. Probably was the fastest option.

Then i created an "images" folder inside my repository and stored all my photos inside it, then i opened the "Search" tab in vscode to replace all the "s3.sa-east-1.amazonaws.com" links to "/images/", then it worked perfectly, now all my images are stored locally in the repository instead of a third party server.

## Using GitHub Actions and GitHub Pages

To replace railway, i used GitHub Actions, it is a free service provided by github to run a pipeline when a commit happens in a repository branch, i followed the official documentation in the Hugo blog(https://gohugo.io/hosting-and-deployment/hosting-on-github/). Then this pipeline publishes the static site generated by hugo in GitHub Pages, another GitHub service that offers free hosting of static pages, in fact, my portfolio already uses GitHub Pages.

Then i configured my DNS (https://registro.br) to point to the GH Pages address instead of the railway one.

## Conclusion

And that's how i migrated my blog to Hugo, you can see the code of my blog in the following repository: https://github.com/marcoagpegoraro/marco_blog_hugo

And since you are here, from now on, i will write only posts in english instead of portuguese, because it's a language that anyone in the world can understand. As you can probably tell, my english is not perfect, but i think that the more i write things in english, the more i get better at it.

Until next time!