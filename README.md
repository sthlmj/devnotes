# Welcome to SthlmJ DevNotes

## Girder

**Girder Explained**:
Girder is a data management platform. https://blog.kitware.com/girder-applications-an-inside-look/
Official documentation for the old 2.4.0 version. https://buildmedia.readthedocs.org/media/pdf/girder/v2.4.0/girder.pdf

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
Differences between dockerfile and docker-compose: Docker-compose.yml files are used for defining and running multi-container Docker applications, whereas Dockerfiles are simple text files that contain the commands to assemble an image that will be used to deploy containers.

### Example with Dockerfile and Docker Compose: 
Link: https://docs.docker.com/compose/gettingstarted/

#### Step1: Setup
Define the application dependencies.

##### 1. Create a directory for the project:
```
$ mkdir composetest
$ cd composetest
```

2. Create a file called app.py in your project directory and paste this in:
```
import time

import redis
from flask import Flask

app = Flask(__name__)
cache = redis.Redis(host='redis', port=6379)


def get_hit_count():
    retries = 5
    while True:
        try:
            return cache.incr('hits')
        except redis.exceptions.ConnectionError as exc:
            if retries == 0:
                raise exc
            retries -= 1
            time.sleep(0.5)


@app.route('/')
def hello():
    count = get_hit_count()
    return 'Hello World! I have been seen {} times.\n'.format(count)
```

In this example, redis is the hostname of the redis container on the application’s network. We use the default port for Redis, 6379.

Handling transient errors

Note the way the get_hit_count function is written. This basic retry loop lets us attempt our request multiple times if the redis service is not available. This is useful at startup while the application comes online, but also makes our application more resilient if the Redis service needs to be restarted anytime during the app’s lifetime. In a cluster, this also helps handling momentary connection drops between nodes.

3. Create another file called requirements.txt in your project directory and paste this in:
```
flask
redis
```

#### Step2: Create a Dockerfile
In this step, you write a Dockerfile that builds a Docker image. The image contains all the dependencies the Python application requires, including Python itself.

In your project directory, create a file named Dockerfile and paste the following:
```
FROM python:3.7-alpine
WORKDIR /code
ENV FLASK_APP app.py
ENV FLASK_RUN_HOST 0.0.0.0
RUN apk add --no-cache gcc musl-dev linux-headers
COPY requirements.txt requirements.txt
RUN pip install -r requirements.txt
COPY . .
CMD ["flask", "run"]
```
This tells Docker to:

Build an image starting with the Python 3.7 image.
Set the working directory to /code.
Set environment variables used by the flask command.
Install gcc so Python packages such as MarkupSafe and SQLAlchemy can compile speedups.
Copy requirements.txt and install the Python dependencies.
Copy the current directory . in the project to the workdir . in the image.
Set the default command for the container to flask run.
For more information on how to write Dockerfiles, see the Docker user guide and the Dockerfile reference.

#### Step 3: Define services in a Compose file
Create a file called docker-compose.yml in your project directory and paste the following:
```
version: '3'
services:
  web:
    build: .
    ports:
      - "5000:5000"
  redis:
    image: "redis:alpine"
```
This Compose file defines two services: web and redis.

Web service
The web service uses an image that’s built from the Dockerfile in the current directory. It then binds the container and the host machine to the exposed port, 5000. This example service uses the default port for the Flask web server, 5000.

Redis service
The redis service uses a public Redis image pulled from the Docker Hub registry.

#### Step 4: Build and run your app with Compose
1. From your project directory, start up your application by running docker-compose up.
```
$ docker-compose up
Creating network "composetest_default" with the default driver
Creating composetest_web_1 ...
Creating composetest_redis_1 ...
Creating composetest_web_1
Creating composetest_redis_1 ... done
Attaching to composetest_web_1, composetest_redis_1
web_1    |  * Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)
redis_1  | 1:C 17 Aug 22:11:10.480 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
redis_1  | 1:C 17 Aug 22:11:10.480 # Redis version=4.0.1, bits=64, commit=00000000, modified=0, pid=1, just started
redis_1  | 1:C 17 Aug 22:11:10.480 # Warning: no config file specified, using the default config. In order to specify a config file use redis-server /path/to/redis.conf
web_1    |  * Restarting with stat
redis_1  | 1:M 17 Aug 22:11:10.483 * Running mode=standalone, port=6379.
redis_1  | 1:M 17 Aug 22:11:10.483 # WARNING: The TCP backlog setting of 511 cannot be enforced because /proc/sys/net/core/somaxconn is set to the lower value of 128.
web_1    |  * Debugger is active!
redis_1  | 1:M 17 Aug 22:11:10.483 # Server initialized
redis_1  | 1:M 17 Aug 22:11:10.483 # WARNING you have Transparent Huge Pages (THP) support enabled in your kernel. This will create latency and memory usage issues with Redis. To fix this issue run the command 'echo never > /sys/kernel/mm/transparent_hugepage/enabled' as root, and add it to your /etc/rc.local in order to retain the setting after a reboot. Redis must be restarted after THP is disabled.
web_1    |  * Debugger PIN: 330-787-903
redis_1  | 1:M 17 Aug 22:11:10.483 * Ready to accept connections
```
Compose pulls a Redis image, builds an image for your code, and starts the services you defined. In this case, the code is statically copied into the image at build time.

2. Enter http://localhost:5000/ in a browser to see the application running.

If you’re using Docker natively on Linux, Docker Desktop for Mac, or Docker Desktop for Windows, then the web app should now be listening on port 5000 on your Docker daemon host. Point your web browser to http://localhost:5000 to find the Hello World message. If this doesn’t resolve, you can also try http://127.0.0.1:5000.

If you’re using Docker Machine on a Mac or Windows, use docker-machine ip MACHINE_VM to get the IP address of your Docker host. Then, open http://MACHINE_VM_IP:5000 in a browser.

You should see a message in your browser saying:

Hello World! I have been seen 1 times.

3. Refresh the page.

The number should increment.

Hello World! I have been seen 2 times.


################ Continue

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
3. Make sure you are in the same directory as `docker-compose.yml` and run `docker-compose up -d` to start GitLab

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
