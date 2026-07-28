1. Change the current directory to /Users/marco/music/go/marco_blog_hugo.
2. Create or install a theme:
   - Create a new theme with the command "hugo new theme <THEMENAME>"
   - Install a theme from https://themes.gohugo.io/
3. Edit hugo.toml, setting the "theme" property to the theme name.
4. Create new content with the command "hugo new content <SECTIONNAME>/<FILENAME>.<FORMAT>".
5. Start the embedded web server with the command "hugo server --buildDrafts".




before running the hugo server command, make sure to include the submodule risotto:
```git submodule update --init --recursive```
