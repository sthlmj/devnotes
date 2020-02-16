</br># Welcome to SthlmJ DevNotes


## Keytool, OpenSSL, Root Certificates, Trusted Certificate Authorities (CA), Intermediate Certificates, Root Program 🔐 

Keytool and OpenSSL are both crypto key generator tools, but Keytool has additional feature of manipulating Java's preferred key storage format, the "KeyStore". </br></br>
Java prefers to work with keys and certa stored in KeyStore (TrustStore) = Use KeyStore when working with java apps. OpenSSL does work with standard formats(PEM/CER/CRT/PKCS/etc) but does not manipulate KeyStore files. </br></br>
Root Certificate - a root ssl certificate is a certificate signed by a trusted certificate authorities (CA). Anyone can generate a signing key and sign certs. But not considered valid unless it has been directly or indirectly signed by a trusted CA. </br></br>
A trusted CA is an entity/organization that has been entitled to verify that is effectively who it declares to be. In order for this model to work, all the participants on the game must agree on a set of CA which they trust. </br></br>
All operating systems and most web browsers ship 🚢 with a set of trusted CA. SSL is based on a chain of trust. When a device validates a certificate, it compares the certificate issuer with the set list of trusted CAs. </br></br>
If a match is not found, the client will then check to see if the certificate of the issuing CA was issued by a trusted CA, and so on (with intermediates certificates) until the end of the certificate chain. The top of the chain, the root certificate must be issued by Certificate Authorities.</br></br>
Root Program aka. Trusted Root is at the center of the trust model under public key infrastructure, and by extension SSL/TLS. Root Programs are root stories that are preloaded with root certificates and their public keys.
</br>
</br>
**Great Links** </br>
Link: </br>
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

Inspect a certificate:
$ OpenSSL x509 -in se00-name00.cer -text -noout
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

## GERRITCODEREVIEW

**Great Links** </br>
Documentation: https://gerrit-documentation.storage.googleapis.com/Documentation/2.16.8/index.html </br>
Plugins: https://gerrit-documentation.storage.googleapis.com/Documentation/2.16.8/config-plugins.html </br>
Tutorial: https://www.mediawiki.org/wiki/Gerrit/Tutorial#Set_Up_SSH_Keys_in_Gerrit </br>
Gerrit in production: https://github.com/GerritCodeReview/docker-gerrit </br>
</br>
**Gerrit plug-n-play in docker** </br>
Quickly gets gerrit up n running to test it out: 
```
docker run -ti -p 8080:8080 -p 29418:29418 gerritcodereview/gerrit
```

**Gerrit ssh key n ssh to Gerrit** </br>
Step 1: ssh keygen
```
ssh-keygen -t rsa -C "iam.external@wellwellvell.com"
```

Step 2: get ssh up and running, add private key to agent and check gerrit version through ssh daemon
```
eval `ssh-agent`
ssh-add .ssh/id_rsa
ssh -p 29418 admin@localhost gerrit version
```

**Gerrit plugins** </br>
Possible to install plugins via plugins manager. Just click install and plugin will be downloaded and installed in the plugins directory. Suitable candidate is messageoftheday plugin.

List all plugins: 
```
ssh -p 29418 admin@localhost gerrit plugin ls
```

List all plugins - output:
```
Name                           Version    Status   File
-------------------------------------------------------------------------------
avatars-gravatar               381fc84a89 ENABLED  avatars-gravatar.jar
codemirror-editor              v2.16.8    ENABLED  codemirror-editor.jar
commit-message-length-validator v2.16.8    ENABLED  commit-message-length-validator.jar
delete-project                 v2.16-116-g6787290183 ENABLED  delete-project.jar
download-commands              v2.16.8    ENABLED  download-commands.jar
events-log                     v2.13-284-g7c88d05a87 ENABLED  events-log.jar
find-owners                    a11833e109 ENABLED  find-owners.jar
gitiles                        2c91cbc413 ENABLED  gitiles.jar
healthcheck                    0dd5329ea2 ENABLED  healthcheck.jar
heartbeat                      0371a82791 ENABLED  heartbeat.jar
hooks                          v2.16.8    ENABLED  hooks.jar
messageoftheday                3f65e8bfc6 ENABLED  messageoftheday.jar
plugin-manager                 v2.15.3-6-g613e25e1b7 ENABLED  plugin-manager.jar
replication                    v2.16.8    ENABLED  replication.jar
reviewnotes                    v2.16.8    ENABLED  reviewnotes.jar
singleusergroup                v2.16.8    ENABLED  singleusergroup.jar
uploadvalidator                f7b1c59e78 ENABLED  uploadvalidator.jar
```

Reload plugin after installing - messageoftheday:
```
ssh -p 29418 admin@localhost gerrit plugin reload messageoftheday
```

Install heartbeat.jar plugin: </br>
```
ssh -p 29418 admin@localhost gerrit plugin install https://gerrit-ci.gerritforge.com/view/Plugins-stable-2.16/job/plugin-heartbeat-bazel-stable-2.16/lastSuccessfulBuild/artifact/bazel-bin/plugins/heartbeat/heartbeat.jar
```

Install heartbeat.jar - tail log - output: </br>
```
[2020-01-29 23:04:22,852] [SSH gerrit plugin install https://gerrit-ci.gerritforge.com/view/Plugins-stable-2.16/job/plugin-heartbeat-bazel-stable-2.16/lastSuccessfulBuild/artifact/bazel-bin/plugins/heartbeat/heartbeat.jar (admin)] INFO  com.google.gerrit.server.plugins.PluginLoader : Installed plugin heartbeat
[2020-01-29 23:05:15,910] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin heartbeat
[2020-01-29 23:05:16,025] [PluginScanner] INFO  com.ericsson.gerrit.plugins.heartbeat.HeartbeatDaemon : Initialized to send heartbeat event every 15000 milliseconds
[2020-01-29 23:05:16,043] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin heartbeat, version 0371a82791
[2020-01-29 23:05:16,043] [PluginScanner] INFO  com.ericsson.gerrit.plugins.heartbeat.HeartbeatDaemon : Stopped sending heartbeat event
[2020-01-29 23:05:16,045] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin heartbeat, version 0371a82791
[2020-01-29 23:06:16,624] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.CleanupHandle : Cleaned plugin plugin_heartbeat_200129_2304_303494816642575780.jar
```

Install healthcheck.jar plugin: </br>
```
ssh -p 29418 admin@localhost gerrit plugin install https://gerrit-ci.gerritforge.com/view/Plugins-stable-2.16/job/plugin-healthcheck-bazel-stable-2.16/lastSuccessfulBuild/artifact/bazel-bin/plugins/healthcheck/healthcheck.jar
```

Install healthcheck.jar - tail log - output: </br>
```
[2020-01-29 23:00:28,240] [SSH gerrit plugin install https://gerrit-ci.gerritforge.com/view/Plugins-stable-2.16/job/plugin-healthcheck-bazel-stable-2.16/lastSuccessfulBuild/artifact/bazel-bin/plugins/healthcheck/healthcheck.jar (admin)] INFO  com.google.gerrit.server.plugins.PluginLoader : Installed plugin healthcheck
[2020-01-29 23:01:15,958] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin healthcheck
Jan 29, 2020 11:01:16 PM com.google.inject.servlet.GuiceFilter setPipeline
WARNING: Multiple Servlet injectors detected. This is a warning indicating that you have more than one GuiceFilter running in your web application. If this is deliberate, you may safely ignore this message. If this is NOT deliberate however, your application may not work as expected.
[2020-01-29 23:01:16,129] [PluginScanner] INFO  com.google.gerrit.server.config.PluginConfigFactory : No /var/gerrit/etc/healthcheck.config; assuming defaults
[2020-01-29 23:01:16,140] [PluginScanner] WARN  com.google.gerrit.server.plugins.PluginLoader : Cannot load plugin healthcheck
java.lang.IllegalArgumentException: A metric named plugins/healthcheck/reviewdb/latest_latency already exists
	at com.codahale.metrics.MetricRegistry.register(MetricRegistry.java:97)
	at com.google.gerrit.metrics.dropwizard.CallbackMetricImpl0.register(CallbackMetricImpl0.java:72)
	at com.google.gerrit.metrics.dropwizard.DropWizardMetricMaker.newTrigger(DropWizardMetricMaker.java:304)
	at com.google.gerrit.server.plugins.PluginMetricMaker.newTrigger(PluginMetricMaker.java:161)
	at com.google.gerrit.metrics.MetricMaker.newTrigger(MetricMaker.java:139)
	at com.googlesource.gerrit.plugins.healthcheck.HealthCheckMetrics.start(HealthCheckMetrics.java:80)
	at com.google.gerrit.lifecycle.LifecycleManager.start(LifecycleManager.java:95)
	at com.google.gerrit.server.plugins.ServerPlugin.startPlugin(ServerPlugin.java:246)
	at com.google.gerrit.server.plugins.ServerPlugin.start(ServerPlugin.java:175)
	at com.google.gerrit.server.plugins.PluginLoader.runPlugin(PluginLoader.java:495)
	at com.google.gerrit.server.plugins.PluginLoader.rescan(PluginLoader.java:423)
	at com.google.gerrit.server.plugins.PluginScannerThread.run(PluginScannerThread.java:42)
```

Install its-jira.jar plugin: </br>
```
ssh -p 29418 admin@localhost gerrit plugin install -n its-jira.jar https://gerrit-ci.gerritforge.com/view/Plugins-stable-2.16/job/plugin-its-jira-bazel-stable-2.16/lastSuccessfulBuild/artifact/bazel-bin/plugins/its-jira/its-jira.jar
```

Install its-jira plugin - tail log: </br>
```
docker logs 1b7ee3d03e4a
```

Install its-jira plugin - tail log - output: </br>
```
com.google.gerrit.server.plugins.PluginInstallException: No valid Jira server configuration was found for project 'All-Projects'
.Missing one or more configuration values: url: null, username: null, password: null
	at com.google.gerrit.server.plugins.PluginLoader.runPlugin(PluginLoader.java:516)
	at com.google.gerrit.server.plugins.PluginLoader.installPluginFromStream(PluginLoader.java:190)
	at com.google.gerrit.sshd.commands.PluginInstallCommand.doRun(PluginInstallCommand.java:85)
	at com.google.gerrit.sshd.commands.PluginAdminSshCommand.run(PluginAdminSshCommand.java:34)
	at com.google.gerrit.sshd.SshCommand$1.run(SshCommand.java:44)
	at com.google.gerrit.sshd.BaseCommand$TaskThunk.run(BaseCommand.java:463)
	at com.google.gerrit.server.logging.LoggingContextAwareRunnable.run(LoggingContextAwareRunnable.java:83)
	at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:511)
	at java.util.concurrent.FutureTask.run(FutureTask.java:266)
	at java.util.concurrent.ScheduledThreadPoolExecutor$ScheduledFutureTask.access$201(ScheduledThreadPoolExecutor.java:180)
	at java.util.concurrent.ScheduledThreadPoolExecutor$ScheduledFutureTask.run(ScheduledThreadPoolExecutor.java:293)
	at com.google.gerrit.server.git.WorkQueue$Task.run(WorkQueue.java:646)
	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1149)
	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:624)
	at java.lang.Thread.run(Thread.java:748)
Caused by: java.lang.RuntimeException: No valid Jira server configuration was found for project 'All-Projects'
.Missing one or more configuration values: url: null, username: null, password: null
	at com.googlesource.gerrit.plugins.its.jira.JiraItsServer.getFacade(JiraItsServer.java:60)
	at com.googlesource.gerrit.plugins.its.jira.JiraItsStartupHealthcheck.start(JiraItsStartupHealthcheck.java:43)
	at com.google.gerrit.lifecycle.LifecycleManager.start(LifecycleManager.java:95)
	at com.google.gerrit.server.plugins.ServerPlugin.startPlugin(ServerPlugin.java:246)
	at com.google.gerrit.server.plugins.ServerPlugin.start(ServerPlugin.java:175)
	at com.google.gerrit.server.plugins.PluginLoader.runPlugin(PluginLoader.java:495)
	... 14 more
fatal: Plugin failed to install. Cause: No valid Jira server configuration was found for project 'All-Projects'
.Missing one or more configuration values: url: null, username: null, password: null


[2020-01-29 22:47:48,409] [SSH gerrit plugin install -n its-jira.jar https://gerrit-ci.gerritforge.com/view/Plugins-stable-2.16/job/plugin-its-jira-bazel-stable-2.16/lastSuccessfulBuild/artifact/bazel-bin/plugins/its-jira/its-jira.jar (admin)] WARN  com.google.gerrit.server.plugins.PluginLoader : Plugin provides its own name: <its-jira>, use it instead of the input name: <its-jira.jar>
[2020-01-29 22:47:48,597] [SSH gerrit plugin install -n its-jira.jar https://gerrit-ci.gerritforge.com/view/Plugins-stable-2.16/job/plugin-its-jira-bazel-stable-2.16/lastSuccessfulBuild/artifact/bazel-bin/plugins/its-jira/its-jira.jar (admin)] INFO  com.googlesource.gerrit.plugins.its.jira.JiraModule : JIRA is configured as ITS
```


## ELK-B

**Elastic Kibana -Beats**
https://rehansaeed.com/tag/docker-compose/ </br>
https://logz.io/blog/metricbeat-tutorial/ </br>
https://itnext.io/deploy-elk-stack-in-docker-to-monitor-containers-c647d7e2bfcd </br>
https://qbox.io/blog/monitoring-docker-containers-with-metricbeat-elasticsearch-and-kibana </br>

**Installing ELK-B:**

Step 1: Install, configure and test that Elasticsearch is running
```
yum install elasticsearch
or
docker run elasticsearch
```

Config file: 
```
/etc/elasticsearch/elasticsearch.yml
```

Config file change params: 
```
cluster.name: Netmon-Cluster
node.name: se00-mon01
network.host: localhost
```

Memory HEAP: 
```
sysctl -w vm.max_map_count=262144
```

Test that Elasticsearch is running: 
```
curl http://localhost:9200
```
Verify: ``` name, cluster-name, version, tagline "you know for search" ```


Step 2: Install, configure and test that Logstash is running on 192.168.0.14:5043
```
yum install logstash
or 
docker run logstash
```

logstash is installed in: 
```/usr/share/logstash```

Test logstash installation that it's sending message to elasticsearch on localhost or (192.168.0.12): 
By inputing to log: ``` /usr/share/logstash/bin/logstash -e 'input { stdin { } } output { elasticsearch { hosts => ["192.168.0.12:9200"] } }' ```

Type some test message on the terminal

Verify message was received on elasticsearch: ```GET http://192.168.0.12:9200/logstash-*/_search```
Verify GET message index was from ```logstash, timestamp, message```

Step 3: Install, configure and test kibana
```
yum install kibana
or 
docker run kibana
```

Config file: 
```/etc/kibana/kibana.yml```

Config file change params: 
```
server.host: "192.168.0.15"
server.name: "my-kibana"
elasticsearch.url: "http://192.168.0.12:9200"
```

Kibana confirm index pattern: 
Goto kibana: http://192.168.0.15:5601 and confirm that index pattern is: ```logstash-*```

Kibana select confirm time-field name: @timestamp

Kibana click ```Discover``` tab and see that you've got the log from Step 2

Step 4: Configure logstash for beats
On logstash server, add new logpattern:
```/etc/logstash/conf.d/beats.conf

input{
    beats{
        port => "5043"
    }
}

output{
    elasticsearch {
        hosts => ["192.168.0.12:9200"]
        index => "%{[@metadata][beat]}-%{+YYYY.MM.dd}"
        document_type => "%{[@metadata][type]}"
    }
}

```

Step 5: Creating winlogbeat dashboard </br>
On kibana, click index pattern, click add new, winlogbeat index name pattern: ```winlogbeat-*``` time-field should be: ```@timestamp``` </br>

Vizualize winlogbeat Dashboard: </br>
Select Line Chart, choose ```winlogbeat-*``` index pattern. ```Y-Axis: Count``` and ```X-Axis: Date Histogram``` and @timestamp, custom label Events. Click on play button. Add per server on same chart, select Split Chart, sub-aggregate: Terms, Fields: computer_name, order-by: metric: count, custom-label: Hosts. Fix split lines. Click on play button again. Save Vizualization as Windows Server Events.</br>

Create dashboard and add vizualization: </br>
Click Dashboard, add Windows Server Events. Adjust the dashboard and save as: Windows Server: Events</br>

Fix so that it shows Error events: </br>
Click Discovery, on search box: level: "Error". Save search as: Windows Events: Errors. </br>

Click vizualization, click bar-chart. Select saved search: Windows Events: Errors
Customize chart: Y-Axis: Count and X-Axis: Date Histogram, @timestamp, split bars by: Terms, Field: Computer_name, Order By: metric: count. 
Save vizualization: Windows Events: Errors</br>

Click Dashboard, add Windows Events: Errors to dashboard. Set refresh intervalls on 10 sec. Save Dashboard.</br>

Step 6: Install metricbeat and configure metricbeat for windows</br>
Download metricbeat for windows. 
Open and configure ```metricbeat.yml``` file, check metricbeat configs for cpu, ram hdd, tags and fields. Change output from default elasticsearch to logstash.

Install metricbeat template:
```Invoke-WebRequest -uri http://192.168.0.12:9200/_template/metricbeat -Method PUT -infile .\metricbeat.template.json```
Verify StatusCode: 200, OK, Acknowledge. So that we know our template is correctly installed.

Install and run metricbeat service: 
```.\install-service-metricbeat.ps1```
```start-service metricbeat```

Step 7: Add new index pattern in kibana</br>
Click management, add new: ```metricbeat-*``` and select ```@timestamp``` Click save. </br>
Verify that metricsbeat are streaming in in the Discovery tab.
Visualize RAM consumption with a line chart in visualize. pick ```metricbeat-*``` and set ```Y-Axis: Average```, ```system.memory.used.pct``` on X-Axis: select Date Histogram. Add sub bucket to set on each servers. Split lines. Field: beat.hostname. Save visualization: Server Ram Usage. </br>

Visualize CPU, create new area chart, metricbeat...... to be continued. </br>

Visualize HDD, create new area chart, metricbeat...... to be continued. </br>

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
