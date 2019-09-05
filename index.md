# Welcome to SthlmJ DevNotes

## Docker stuff
```markdown 

Pulls **jenkins LTS**:
`docker pull jenkins/jenkins:lts`

Runs jenkins on docker on mapped port 8082 **volume attached and java env**:
`docker run --name MyJenkins -p 8082:8080 -p 50000:50000 -v /Users/joe/Desktop/Jenkins_Home:/var/jenkins_home --env JAVA_OPTS=-Dhudson.footerURL=http://mycompany.com jenkins/jenkins:lts`

Runs jenkins on docker with **java environment**: 
`docker run --name MyJenkins -p 8082:8080 -p 50000:50000 --env JAVA_OPTS=-Dhudson.footerURL=http://mycompany.com jenkins/jenkins:lts`

Runs jenkins on docker on mapped port 8082 **volume attached**:
`docker run --name MyJenkins -p 8082:8080 -p 50000:50000 -v /Users/joe/Desktop/Jenkins_Home:/var/jenkins_home jenkins/jenkins:lts`

Runs jenkins on docker on mapped port 8082 **_no_ volume attached**:
`docker run -p 8082:8080 -p 50000:50000 jenkins/jenkins:lts`

**Jenkins docker timezone**, add the following to run command: 
`-v /etc/timezone:/etc/timezone -v /etc/localtime:/etc/localtime`

Explore a running **docker container**:
`docker exec -it name-of-container bash`

The equivalent for this in docker-compose would be:
`docker-compose exec web bash`
(web is the name-of-service in this case and it has tty by default.)

Once you are inside do:
`ls -lsa`

or any other bash command like:
`cd ..`

------------------------------------------------------------------------------
This command should let you explore **a docker image**:
`docker run --rm -it --entrypoint=/bin/bash name-of-image`

once inside do:
`ls -lsa`

or any other bash command like:
`cd ..`
The -it stands for interactive... and tty.

This command should let you **inspect a running docker container or image**:
`docker inspect name-of-container-or-image`

You might want to do this and find out if there is any bash or sh in there. Look for entrypoint or cmd in the json return.

see docker exec documentation
https://docs.docker.com/engine/reference/commandline/exec/#usage

see docker-compose exec documentation
https://docs.docker.com/compose/reference/exec/

see docker inspect documentation
https://docs.docker.com/engine/reference/commandline/inspect/
####------------------------------------------------------------------------------

`docker version` ~ docker version
`docker info` ~ info of all images and containers 
`docker run` ~ run a container
`docker ps` ~ see current running containers
`docker ps -a` ~ see past run containers
`docker images` ~ list all images
`docker tag efb740c5821f artifactory` ~ tag a long image name using image id into short name _artifactory_

see short introduction to tags
https://www.freecodecamp.org/news/an-introduction-to-docker-tags-9b5395636c2a/

Docker Hub is the default public registry

Images ~ Stopped containers
Containers ~ Running Images

`docker rmi` ~ remove an image
`docker rm` ~ also removes an image
`docker run -d --name web -p 80:8080 nigelpoulton/pluralsight-docker-ci` 
~ 
`-d` = telling daemon start container in detached mode = throw in background and don't latch in terminal output. 
`--name web` = our container name is **web**
`-p` = map network port. web server listening to port 8080. we assign port 80 on docker host to port 8080 inside container. 
`nigelpoulton/pluralsight-docker-ci` = telling which image to use. 

Interactive container with shell: 
`docker run -it --name temp ubuntu:latest /bin/bash`

`ctrl p + q` ~ exit the ubuntu

`docker stop $(docker ps -aq)` ~ running docker stop as an argument.
`docker rm $(docker ps -aq)` ~ remove all container with docker ps -aq as argument.
`docker rmi $(docker images -q)` ~ doing the same for all the images.
```


## Kanban vs. Scrum 
Scrum and Kanban are both iterative work systems that rely on process flows and aim to reduce waste. However; there are a few main differences between the two:

**Roles & Responsibilities:** 
- **Kanban** There are no pre-defined roles for a team. Although there may still be a Project Manager, the team is encouraged to collaborate and chip in when any one person becomes overwhelmed.	
- **Scrum** Each team member has a predefined role, where the Scrum master dictates timelines, Product owner defines goals and objectives and team members execute the work.

**Due Dates / Delivery Timelines:**	
- **Kanban** Products and processes are delivered continuously on an as-needed basis (with due dates determined by the business as needed).	
- **Scrum** Deliverables are determined by sprints, or set periods of time in which a set of work must be completed and ready for review.

**Delegation & Prioritization:** 
- **Kanban** Uses a “pull system,” or a systematic workflow that allows team members to only “pull” new tasks once the previous task is complete.	
- **Scrum** Also uses a “pull system” however an entire batch is pulled for each iteration.

**Modifications / Changes:**	
- **Kanban** Allows for changes to be made to a project mid-stream, allowing for iterations and continuous improvement prior to the completion of a project.	
- **Scrum** Changes during the sprint are strongly discouraged.

**Measurement of Productivity:**
- **Kanban** Measures production using “cycle time,” or the amount of time it takes to complete one full piece of a project from beginning to end.	
- **Scrum** Measures production using velocity through sprints. Each sprint is laid out back-to-back and/or concurrently so that each additional sprint relies on the success of the one before it.

**Best Applications**	
- **Kanban:** Best for projects with widely-varying priorities.	
- **Scrum:** Best for teams with stable priorities that may not change as much over time.





########################################################################################################

# How to use Jekyll Github Pages theme: 
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

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/sthlmj/devnotes/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file..

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
