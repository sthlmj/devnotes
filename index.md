# Welcome to SthlmJ DevNotes

## Docker stuff
docker version ~ docker version
docker info ~ info of all images and containers 
docker run ~ run a container
docker ps ~ see current running containers
docker ps -a ~ see past run containers
docker images ~ list all images

Docker Hub is the default public registry

Images ~ Stopped containers
Containers ~ Running Images

docker rmi ~ remove an image
docker rm ~ also removes an image

 docker run -d --name web -p 80:8080 nigelpoulton/pluralsight-docker-ci 
~ 
-d telling daemon start container in detached mode = throw in background and don't latch in terminal output. 
--name web = our container name is web
-p = map network port. web server listening to port 8080. we assign port 80 on docker host to port 8080 inside container. 
nigelpoulton/pluralsight-docker-ci  = telling which image to use. 

docker ps

Interactive container with shell: 
docker run -it --name temp ubuntu:latest /bin/bash

ctrl p + q ~ exit the ubuntu

docker stop $(docker ps -aq) ~ running docker stop as an argument.
docker rm $(docker ps -aq) ~ remove all container with docker ps -aq as argument.
docker rmi $(docker images -q) ~ doing the same for all the images.





You can use the [editor on GitHub](https://github.com/sthlmj/devnotes/edit/master/index.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/sthlmj/devnotes/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
