# Trickshots
###Joel Bandi's blog and bestiary.

I use this [blog](https://joelbandi.github.io/trickshots) as a way of documenting my journey through the wildlands of software programming, 
as a way of reminding myself and others on how to effectively learn use masterfully crafted programming Trickshots.
Trickshots that I've learnt during this journey


This blog uses [Hexo](https://hexo.io/), a static site generator with over _10,000_ stars on github. You can check its rankings in [Staticgen](https://www.staticgen.com/)

__To obtain a local copy of this blog, follow directions below:__



1. Make sure you have [Node](https://nodejs.org/en/) and [Git](https://git-scm.com/) installed
2. Follow instruction in [Hexo](https://hexo.io/) to install hexo-cli using ```npm install hexo-cli -g```
3. Clone a local copy using ```git clone https://github.com/joelbandi/trickshots.git```
4. Run ```npm install``` to download local dependencies
5. Use ```hexo generate``` to generate the static pages. Use the optional ```-w``` flag to watch source for updates
6. Use ```hexo server -p 5000``` to serve up the static files. The ```-p``` option specifies the port bindings
7. Visit [http://localhost:5000/](http://localhost:5000/) to access the blog


__After making required changes if you wish to deploy the blog and push the code:__

1. Update the repo field in the ```/_config.yml``` file to wherever you want to deploy
2. Make sure the repo has a branch called ```gh-pages```
2. _optional_ : You can also add heroku to your deploy option if need arises
3. Run ```hexo deploy -g``` to publish it to the gh-pages branch of the specified repo



__Some useful Hexo [commands](https://hexo.io/docs/commands.html) for quick reference:__

1. Use ```hexo new post <title>``` to create a new post in the blog.
2. Use ```hexo new page <title>``` to create a new page in your blog.


__Useful tip:__

There are customized scripts in package.json file for ease of use.
The following scripts work provided you have initialized the repository with proper git settings.

1. Use ```npm run master-push``` to push the master source copy into the 'master' branch of this repo.
2. Use ```npm run blog-deploy``` to deploy the blog on GitHub pages.
3. Use ```npm run full-sync``` to perform both actions listed below.
