# Welcome to SthlmJ DevNotes

## SELinux
Additional security layer that controls context of files processes and users. 
SELinux can be used to control processes, networks and execution contexts of processes and files.

```
Check current SELinux levels: 

Check SELinux status: 
$ getenforce

Full details: 
$ sestatus

Path: 
$ /etc/selinux/config

SE modules boolean: 
$ semodule -l PIPE less

List modules and boolean status: 
$ semanage boolean -l PIPE less

Gets current boolean status of a module: 
$ getsebool ftp_home_dir

Sets boolean status of a module: 
$ setsebool ftp_home_dir on

Gets current boolean status of a module again to verify changes: 
$ getsebool ftp_home_dir

Check SELinux status of the folder: 
$ls -lZ /var/gerrit/
Gives: system_u:object_r:user_home_t:s0
Which means: 1. user, 2. role, 3. type of file, 4. selinux level

Allow HTTP servers to connect to other backens: 
$ setsebool -P httpd_can_network_connect on

Allow HTTP servers to read files from user directory: 
$ setsebool -P httpd_enable_homedirs on

Allow HTTP servers to read files from user directory && veenee sandbox SELinux conf:
$ chcon -Rt httpd_sys_content_t /home/z-johnsnow/public
$ chcon -Rt svirt_sandbox_file_t /var/db 

Check SELinux status of the folder:
$ ls -lZ /home/z-johnsnow/public
```

https://medium.com/lucjuggery/docker-selinux-30-000-foot-view-30f6ef7f621


## Configuration Management 🔬

CM in this context is Software Development CM. This is not for IT configuration management with puppet chef ansibel!!!
CM is a human log/version control system. With goals to improve software development in a most effective way as possible. Like researcher in the laboratory that does experiments, a scientist must know what combinations of ingredients and recipes they already have tested so that they are not re-developing already tested combination. Fastest way to correct build configuration/ recipes for correct software version is key to faster product to market and lower cost of production. CM also works like link between PM and developers, preventing developers in different groups to do the same thing. CM needs to leverage all developers work so that they collaborates better and keeping track of who does what. So that developers are not doing the same things repetitively.
CM produces reports to the project for project progress. CM attempts to identify and track all relevant elements of the configuration of a software that are being developed, so that all possible errors and key ingredients (configuration) can be identified and possible solution to the problems found/Find bug root cause. CM then identifies the working version of software and reverts branch to the working one or most interesting finding one.

VCS - tools for CM to maintain multiple versions of a software system.</br>
CM Defect tracking system - Jenkins build logs, pull requests.

1. Identification - identifierar ändringar som händer utvecklingnivån/kodnivån.
2. Control - kontrollera ändringar i kodnivån.
3. Implement - implementera ändringar. Testköra ändringar ett antal gånger.
4. Kontrollera resultat ändring. Se till att projekt vet vad dom gör. Håll koll på vad utvecklarna har gjort.
5. Report results and lessons learned. 
6. If possible to do, make audit.

CM in this context, !CM for IT systems configurations: 
https://youtu.be/yTJyONlB_0o

## Keytool, OpenSSL, Root Certificates, Trusted Certificate Authorities (CA), Intermediate Certificates, Root Program 🔐 

Keytool and OpenSSL are both crypto key generator tools, but Keytool has additional feature of manipulating Java's preferred key storage format, the "KeyStore". </br></br>
Java prefers to work with keys and certs stored in KeyStore (TrustStore) = Use KeyStore when working with java apps/services like jenkins. OpenSSL does work with standard certificate formats(PEM/CER/CRT/PKCS/etc) but does not manipulate KeyStore files. </br></br>
Root Certificate - a root ssl certificate is a certificate signed by a trusted certificate authorities (CA). Anyone can generate a signing key and sign certs. But not considered valid unless it has been directly or indirectly signed by a trusted CA. </br></br>
A trusted CA is an entity/organization that has been entitled to verify that is effectively who it declares to be. In order for this model to work, all the participants on the game must agree on a set of CA which they trust. </br></br>
All operating systems and most web browsers ship 🚢 with a set of trusted CA. SSL is based on a chain of trust. When a device validates a certificate, it compares the certificate issuer with the set list of trusted CAs. </br></br>
If a match is not found, the client will then check to see if the certificate of the issuing CA was issued by a trusted CA, and so on (with intermediates certificates) until the end of the certificate chain. The top of the chain, the root certificate must be issued by Certificate Authorities.</br></br>
Root Program aka. Trusted Root is at the center of the trust model under public key infrastructure, and by extension SSL/TLS. Root Programs are root stories that are preloaded with root certificates and their public keys.
</br>
1. truststore = public key + details storage.</br>
Examples: certs stored in a browser, .crt, .cer
Summary: A truststore is used for what public keys to trust, like cert files.</br>
2. keystore = (public key + details) & (private key + details) storage.</br>
(Note: Storing the private key is why it requires a password, otherwise your security could be breached easily).</br>
Examples: .jks, cacerts file, p12 (can be either keystore or truststore)

Summary: A keystore is primarily used as a storage of your public and private keys so that you can perform HTTPS TLS encryption for others, but as shown by cacerts file used by JREs and later versions of JDKs, a keystore can be used as a truststore as well since it has all the details of the truststore.
</br>
So, all keystores are truststores, but no truststore is a keystore.
</br></br>
To test TLS/SSL connection you can either sign in to your service once ldaps is implemented. But you can also use SSLPoke if you want to check current session: </br>
https://matthewdavis111.com/java/poke-ssl-test-java-certs/
</br>
</br>
**Great Links** </br>
Link: </br>

https://gist.github.com/4ndrej/4547029 </br>
https://matthewdavis111.com/java/poke-ssl-test-java-certs/</br>
https://www.sslshopper.com/article-most-common-java-keytool-keystore-commands.html</br>
https://www.sslshopper.com/article-most-common-openssl-commands.html</br>
https://manuals.gfi.com/en/kerio/connect/content/server-configuration/ssl-certificates/adding-trusted-root-certificates-to-the-server-1605.html </br>
https://www.watchguard.com/help/docs/ssl/3/en-us/content/en-us/manage_system/active_directory_auth_w-ldap-ssl.html</br>
https://security.stackexchange.com/questions/98282/difference-between-openssl-and-keytool</br>
https://www.thesslstore.com/blog/root-certificates-intermediate/</br>
https://support.dnsimple.com/articles/what-is-ssl-root-certificate/</br>
https://www.top-password.com/blog/view-installed-certificates-in-windows-10-8-7/</br>
https://serverfault.com/questions/590870/how-to-view-all-ssl-certificates-in-a-bundle</br>
https://en.wikipedia.org/wiki/Root_certificate</br>
https://stackoverflow.com/questions/25156180/how-to-list-certificates-trusted-by-openssl</br></br>
**Generate, import trusted certificates** </br>
Code part:

```
List trusted CA:
$ keytool -list -v -keystore $JAVA_HOME/jre/lib/security/cacerts

Import new CA into Trusted Certs:
$ keytool -import -trustcacerts -file /path/to/ca/ca.pem -alias CA_ALIAS -keystore $JAVA_HOME/jre/lib/security/cacerts
$ keytool -import -alias myca -keystore /usr/lib/jvm/java-8-openjdk-amd64/jre/lib/security/cacerts -trustcacerts -file /server.ca

Inspect a certificate:
$ OpenSSL x509 -in se00-name00.cer -text -noout
$ echo | openssl s_client -showcerts -servername gnupg.org -connect nsa.gov:443 2>/dev/null | openssl x509 -inform pem -noout -text
```

Windows:
Win+R > certmgr.msc


## Puppet + Kubernetes + Chocolatey 🍫

With Kubernetes you can orchestrate your containers from one source.  Together with Puppet and Chocolatey, it enables you to manage windows nodes almost like it was a Linux machine considering that Chocolatey is a package/software management service for windows and that Puppet is a Configuration Management automation tools with YML.
</br>
</br>
**Great Links** </br>
Tutorials: https://learnk8s.io/blog/installing-docker-and-kubernetes-on-windows

**Chocolatey on Windows** </br>
PowerShell install Chocolatey:

```
PS> Set-ExecutionPolicy Bypass -Scope Process -Force
PS> iex ((New-Object System.Net.WebClient).
  DownloadString('https://chocolatey.org/install.ps1'))
```

## Girder

**Girder Explained**:
Installing third party plugin: 

Step 1: Installing plugins and rebuilding web
```
pip install girder-ldap
girder build
```

Step 2: Commit to docker image and restart Girder container
```
docker commit <CONTAINER-ID> girder/girder:myversion
docker restart girder
```

Admin Documentation link: https://girder.readthedocs.io/en/stable/installation.html

## Jenkins stuff

**Nightly Builds declarative pipeline** </br>
https://hopstorawpointers.blogspot.com/2018/12/nightly-build-steps-with-jenkins.html</br>

**Triggers** </br>
https://www.jenkins.io/doc/book/pipeline/syntax/#triggers

**Dockerized Jenkins**
Visit jenkins container at: http://localhost:8080/jenkins or similar

```
PS C:\> docker logs fcec621fc440
Running from: /usr/share/jenkins/jenkins.war
webroot: EnvVars.masterEnvVars.get("JENKINS_HOME")
2020-02-16 23:16:23.945+0000 [id=1]     INFO    org.eclipse.jetty.util.log.Log#initialized: Logging initialized @785ms to org.eclipse.jetty.util.log.JavaUtilLog
2020-02-16 23:16:24.078+0000 [id=1]     INFO    winstone.Logger#logInternal: Beginning extraction from war file
2020-02-16 23:16:25.499+0000 [id=1]     INFO    org.eclipse.jetty.server.Server#doStart: jetty-9.4.z-SNAPSHOT; built: 2019-05-02T00:04:53.875Z; git: e1bc35120a6617ee3df052294e433f3a25ce7097; jvm 1.8.0_242-b08
2020-02-16 23:16:25.810+0000 [id=1]     INFO    o.e.j.w.StandardDescriptorProcessor#visitServlet: NO JSP Support for /jenkins, did not find org.eclipse.jetty.jsp.JettyJspServlet
2020-02-16 23:16:25.864+0000 [id=1]     INFO    o.e.j.s.s.DefaultSessionIdManager#doStart: DefaultSessionIdManager workerName=node0
2020-02-16 23:16:25.864+0000 [id=1]     INFO    o.e.j.s.s.DefaultSessionIdManager#doStart: No SessionScavenger set, using defaults
2020-02-16 23:16:25.874+0000 [id=1]     INFO    o.e.j.server.session.HouseKeeper#startScavenging: node0 Scavenging every 660000ms
2020-02-16 23:16:26.131+0000 [id=1]     INFO    hudson.WebAppMain#contextInitialized: Jenkins home directory: /var/jenkins_home found at: EnvVars.masterEnvVars.get("JENKINS_HOME")
2020-02-16 23:16:26.277+0000 [id=1]     INFO    o.e.j.s.handler.ContextHandler#doStart: Started w.@26be6ca7{Jenkins v2.204.2,/jenkins,file:///var/jenkins_home/war/,AVAILABLE}{/var/jenkins_home/war}
2020-02-16 23:16:26.318+0000 [id=1]     INFO    o.e.j.server.AbstractConnector#doStart: Started ServerConnector@19932c16{HTTP/1.1,[http/1.1]}{0.0.0.0:8080}
2020-02-16 23:16:26.318+0000 [id=1]     INFO    org.eclipse.jetty.server.Server#doStart: Started @3158ms
2020-02-16 23:16:26.319+0000 [id=20]    INFO    winstone.Logger#logInternal: Winstone Servlet Engine v4.0 running: controlPort=disabled
2020-02-16 23:16:27.919+0000 [id=27]    INFO    jenkins.InitReactorRunner$1#onAttained: Started initialization
2020-02-16 23:16:27.948+0000 [id=27]    INFO    jenkins.InitReactorRunner$1#onAttained: Listed all plugins
2020-02-16 23:16:28.863+0000 [id=27]    INFO    jenkins.InitReactorRunner$1#onAttained: Prepared all plugins
2020-02-16 23:16:28.873+0000 [id=26]    INFO    jenkins.InitReactorRunner$1#onAttained: Started all plugins
2020-02-16 23:16:28.887+0000 [id=26]    INFO    jenkins.InitReactorRunner$1#onAttained: Augmented all extensions
2020-02-16 23:16:29.598+0000 [id=27]    INFO    jenkins.InitReactorRunner$1#onAttained: Loaded all jobs
2020-02-16 23:16:29.634+0000 [id=41]    INFO    hudson.model.AsyncPeriodicWork#lambda$doRun$0: Started Download metadata
2020-02-16 23:16:29.683+0000 [id=41]    INFO    hudson.util.Retrier#start: Attempt #1 to do the action check updates server
2020-02-16 23:16:30.678+0000 [id=28]    INFO    o.s.c.s.AbstractApplicationContext#prepareRefresh: Refreshing org.springframework.web.context.support.StaticWebApplicationContext@1a3cb80c: display name [Root WebApplicationContext]; startup date [Sun Feb 16 23:16:30 UTC 2020]; root of context hierarchy
2020-02-16 23:16:30.679+0000 [id=28]    INFO    o.s.c.s.AbstractApplicationContext#obtainFreshBeanFactory: Bean factory for application context [org.springframework.web.context.support.StaticWebApplicationContext@1a3cb80c]: org.springframework.beans.factory.support.DefaultListableBeanFactory@61c63c12
2020-02-16 23:16:30.686+0000 [id=28]    INFO    o.s.b.f.s.DefaultListableBeanFactory#preInstantiateSingletons: Pre-instantiating singletons in org.springframework.beans.factory.support.DefaultListableBeanFactory@61c63c12: defining beans [authenticationManager]; root of factory hierarchy
2020-02-16 23:16:30.837+0000 [id=28]    INFO    o.s.c.s.AbstractApplicationContext#prepareRefresh: Refreshing org.springframework.web.context.support.StaticWebApplicationContext@412b2401: display name [Root WebApplicationContext]; startup date [Sun Feb 16 23:16:30 UTC 2020]; root of context hierarchy
2020-02-16 23:16:30.838+0000 [id=28]    INFO    o.s.c.s.AbstractApplicationContext#obtainFreshBeanFactory: Bean factory for application context [org.springframework.web.context.support.StaticWebApplicationContext@412b2401]: org.springframework.beans.factory.support.DefaultListableBeanFactory@3d653ef
2020-02-16 23:16:30.838+0000 [id=28]    INFO    o.s.b.f.s.DefaultListableBeanFactory#preInstantiateSingletons: Pre-instantiating singletons in org.springframework.beans.factory.support.DefaultListableBeanFactory@3d653ef: defining beans [filter,legacy]; root of factory hierarchy
2020-02-16 23:16:31.110+0000 [id=28]    INFO    jenkins.install.SetupWizard#init:

*************************************************************
*************************************************************
*************************************************************

Jenkins initial setup is required. An admin user has been created and a password generated.
Please use the following password to proceed to installation:

74e7dcc7dbaf4534a2b38dd

This may also be found at: /var/jenkins_home/secrets/initialAdminPassword

*************************************************************
*************************************************************
*************************************************************
```


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
https://jenkins.io/doc/

### Jenkins Master - Agent Setup:
Link: https://plugins.jenkins.io/swarm </br>
Swarm plugin is the easiest way to utilize master-agent configurations. 


Example:
https://wiki.jenkins.io/display/JENKINS/Step+by+step+guide+to+set+up+master+and+agent+machines+on+Windows


## Binary Repository Manager - Artifactory stuff
Jenkins deploy to artifactory:
```
def server = Artifactory.server '-123456789@1234567890123456'

/*json of uploadSpec, called by 'Deploy' stage*/
def uploadSpec = """{
  "files": [
    {
      "pattern": "*.h",
      "target": "builds/my-project/"
    }
  ]
}"""


node('sandbox')
{
        stage ('Archive')
        {
            archiveArtifacts artifacts: '**/*.h'
			server.upload spec: uploadSpec, failNoOp: true
        }
}

```
Artifactory REST API documentation: https://www.jfrog.com/confluence/display/JFROG/Artifactory+REST+API

## GitLab stuff
Pulling GitLab CE in docker: 
`docker pull store/gitlab/gitlab-ce:11.10.4-ce.0`

Running GitLab CE in docker on port 8083: 
`docker run -d -p 443:443 -p 8083:80 -p 23:22 gitlabben`

GitLab Readme.md:
https://docs.gitlab.com/omnibus/docker/README.html

## Docker stuff
Differences between dockerfile and docker-compose: Docker-compose.yml files are used for defining and running multi-container Docker applications together, whereas Dockerfiles are simple text files that contain the commands to assemble an image that will be used to deploy containers. </br>

Best practice: is to define your own Dockerfile to create the image you desire. Then write the docker.compose.yml to define how you will run the container. This allows you to save history of how the container was run for later on when you want to update your container application to a newer version or adding some environment variables.

Docker proxy: https://docs.docker.com/network/proxy/#configure-the-docker-client

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
  privileged: true
  user: root
```
This Compose file defines two services: web and redis.

Web service
The web service uses an image that’s built from the Dockerfile in the current directory. It then binds the container and the host machine to the exposed port, 5000. This example service uses the default port for the Flask web server, 5000.

Redis service
The redis service uses a public Redis image pulled from the Docker Hub registry.

privileged: true
When running in selinux

user: root
sometimes jenkins inside the container needs to have access to certain folders.

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

See full command of running/stopped container in Docker:
Use runlike from git repo: https://github.com/lavie/runlike

`pip install runlike` Install Runlike
`docker ps -a -q` Find the container ID
`runlike CONTAINERID` Extract complete docker run command

```
## Linux commands 
Write to a file when there are no vi vim nano: 
```
$ cat > resolv.conf
search comapny.int
nameserver 11.22.33.44
nameserver 11.22.33.55
```

Press Ctrl+D to exit and save

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
