# Welcome to SthlmJ DevNotes

## Jenkins stuff

**Jenkins Configuration AS Code**:
With Jenkins Configuration AS Code [JCASC](https://github.com/jenkinsci/configuration-as-code-plugin/blob/master/README.md) plugin you can configure Jenkins master by code instead of clicking through the GUI. This helps if you needs to do a lot of configuraitons at once. 

Jenkins [Configuration AS Code example:](https://www.praqma.com/stories/start-jenkins-config-as-code/)
Jenkins Configuraiton as Code another [example](https://codebabel.com/jcasc-stateless-ci/)

Yet another [example](https://jenkins.io/blog/2018/08/23/speaker-blog-casc-part-1/)

Code example within the path:
`/usr/jenkinsconfigurationascode.yml`

$ cat jenkinsconfigurationascode.yml 
```
jenkins:
  systemMessage: "Jenkan managed by configuration as Code//Joe"

  securityRealm:
    ldap:
      configurations:
        - server: ldap.acme.com
          rootDN: dc=acme,dc=fr
          managerPasswordSecret: ${LDAP_PASSWORD}
      cache:
        size: 100
        ttl: 10
      userIdStrategy: CaseSensitive
      groupIdStrategy: CaseSensitive
```

![Jenkins system wide message](https://github.com/sthlmj/devnotes/blob/master/Screenshot%202019-09-08%20at%2018.31.09.png)
![Jenkins system wide message2](https://github.com/sthlmj/devnotes/blob/master/Screenshot%202019-09-08%20at%2018.37.54.png)


### Jenkinsfile Pipeline as code:
Example: 
TODOOOOOOOOTODOOOOOOOOTODOOOOOOOO
TODOOOOOOOOTODOOOOOOOOTODOOOOOOOO

### Jenkins Master - Agent Setup:
TODOOOOOOOOTODOOOOOOOOTODOOOOOOOO
TODOOOOOOOOTODOOOOOOOOTODOOOOOOOO

Example:
https://wiki.jenkins.io/display/JENKINS/Step+by+step+guide+to+set+up+master+and+agent+machines+on+Windows


## Binary Repository Manager - Artifactory stuff
TODOOOOOOOOTODOOOOOOOOTODOOOOOOOO
TODOOOOOOOOTODOOOOOOOOTODOOOOOOOO

Terminology - What is an artifact: https://devops.stackexchange.com/questions/1898/what-is-an-artifactory

## GitLab stuff
Pulling GitLab CE in docker: 
`docker pull store/gitlab/gitlab-ce:11.10.4-ce.0`

Running GitLab CE in docker on port 8083: 
`docker run -d -p 443:443 -p 8083:80 -p 23:22 gitlabben`

GitLab Readme.md:
https://docs.gitlab.com/omnibus/docker/README.html

## Docker stuff

### Docker compose steps - Example of installing GitLab using docker-compose
With [Docker compose](https://docs.docker.com/compose/) you can easily configure, install, and upgrade your Docker-based GitLab installation.
1. Install docker compose: https://docs.docker.com/compose/install/
2. Create a docker-compose.yml file (or download an example):

```
web:
  image: 'gitlab/gitlab-ce:latest'
  restart: always
  hostname: 'gitlab.example.com'
  environment:
    GITLAB_OMNIBUS_CONFIG: |
      external_url 'https://gitlab.example.com'
      # Add any other gitlab.rb configuration here, each on its own line
  ports:
    - '80:80'
    - '443:443'
    - '22:22'
  volumes:
    - '/srv/gitlab/config:/etc/gitlab'
    - '/srv/gitlab/logs:/var/log/gitlab'
    - '/srv/gitlab/data:/var/opt/gitlab'
```
3. Make sure you are in the same directory as `docker-compose.yml` and `run docker-compose up -d` to start GitLab

Read [“Pre-configure Docker container”](https://docs.gitlab.com/omnibus/docker/README.html#pre-configure-docker-container) to see how the `GITLAB_OMNIBUS_CONFIG` variable works.

Below is another `docker-compose.yml` example with GitLab running on a custom HTTP and SSH port. Notice how the `GITLAB_OMNIBUS_CONFIG` variables match the `ports` section:
```
web:
  image: 'gitlab/gitlab-ce:latest'
  restart: always
  hostname: 'gitlab.example.com'
  environment:
    GITLAB_OMNIBUS_CONFIG: |
      external_url 'http://gitlab.example.com:8929'
      gitlab_rails['gitlab_shell_ssh_port'] = 2224
  ports:
    - '8929:8929'
    - '2224:22'
  volumes:
    - '/srv/gitlab/config:/etc/gitlab'
    - '/srv/gitlab/logs:/var/log/gitlab'
    - '/srv/gitlab/data:/var/opt/gitlab'
```
This is the same as using `--publish 8929:8929 --publish 2224:22`.


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

Explore a running **docker container as root**:
`docker exec -it -u root 18083ce7261b /bin/bash`

As root, **change date** by executing this command inside container: 
`dpkg-reconfigure tzdata`

Explore a running **docker container**:
`docker exec -it name-of-container /bin/bash`

The equivalent for this in docker-compose would be:
`docker-compose exec web bash`
(web is the name-of-service in this case and it has tty by default.)

Once you are inside do:
`ls -lsa`

or any other bash command like:
`cd ..`

Copy files **from container to host**:
`docker cp <containerId>:/file/path/within/container /host/path/target`

Copy file example **from container MyJenkins to host**:
`docker cp MyJenkins:/var/jenkins_home/secrets/initialAdminPassword /host/path/target`

copy file **from host to docker container**:
`docker cp foo.txt mycontainer:/foo.txt`

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

###############################################################################
![Image of Yaktocat](https://octodex.github.com/images/yaktocat.png)
[Mastering Markdown](https://guides.github.com/features/mastering-markdown/)
