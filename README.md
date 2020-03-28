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

**The docker-compose way - Installing ELK-B:**</br>
Step 1: Write some docker-compose.yml that consists for EKB</br>
Step 2: Deploy containers</br>
Step 3: check logs</br>
</br>
When it works outside the prem: 
![MetricBeats in Kibana](https://github.com/sthlmj/devnotes/blob/master/Annotation%202020-02-16%20230825.png)
</br>

ElasticSearch Successful logs:
```
PS C:\> docker logs cc41723db121
OpenJDK 64-Bit Server VM warning: Option UseConcMarkSweepGC was deprecated in version 9.0 and will likely be removed in a future release.
[2020-02-16T21:52:20,784][INFO ][o.e.e.NodeEnvironment    ] [2z2SmG3] using [1] data paths, mounts [[/ (overlay)]], net usable_space [52.1gb], net total_space [58.8gb], types [overlay]
[2020-02-16T21:52:20,787][INFO ][o.e.e.NodeEnvironment    ] [2z2SmG3] heap size [495.3mb], compressed ordinary object pointers [true]
[2020-02-16T21:52:20,788][INFO ][o.e.n.Node               ] [2z2SmG3] node name derived from node ID [2z2SmG3BS7qbJcJRhNJ-Lw]; set [node.name] to override
[2020-02-16T21:52:20,789][INFO ][o.e.n.Node               ] [2z2SmG3] version[6.8.6], pid[1], build[default/docker/3d9f765/2019-12-13T17:11:52.013738Z], OS[Linux/4.19.76-linuxkit/amd64], JVM[AdoptOpenJDK/OpenJDK 64-Bit Server VM/13.0.1/13.0.1+9]
[2020-02-16T21:52:20,789][INFO ][o.e.n.Node               ] [2z2SmG3] JVM arguments [-Xms1g, -Xmx1g, -XX:+UseConcMarkSweepGC, -XX:CMSInitiatingOccupancyFraction=75, -XX:+UseCMSInitiatingOccupancyOnly, -Des.networkaddress.cache.ttl=60, -Des.networkaddress.cache.negative.ttl=10, -XX:+AlwaysPreTouch, -Xss1m, -Djava.awt.headless=true, -Dfile.encoding=UTF-8, -Djna.nosys=true, -XX:-OmitStackTraceInFastThrow, -Dio.netty.noUnsafe=true, -Dio.netty.noKeySetOptimization=true, -Dio.netty.recycler.maxCapacityPerThread=0, -Dlog4j.shutdownHookEnabled=false, -Dlog4j2.disable.jmx=true, -Djava.io.tmpdir=/tmp/elasticsearch-17160688704184476641, -XX:+HeapDumpOnOutOfMemoryError, -XX:HeapDumpPath=data, -XX:ErrorFile=logs/hs_err_pid%p.log, -Xlog:gc*,gc+age=trace,safepoint:file=logs/gc.log:utctime,pid,tags:filecount=32,filesize=64m, -Djava.locale.providers=COMPAT, -XX:UseAVX=2, -Des.cgroups.hierarchy.override=/, -Xms512m, -Xmx512m, -Des.path.home=/usr/share/elasticsearch, -Des.path.conf=/usr/share/elasticsearch/config, -Des.distribution.flavor=default, -Des.distribution.type=docker]
[2020-02-16T21:52:23,476][INFO ][o.e.p.PluginsService     ] [2z2SmG3] loaded module [aggs-matrix-stats]
[2020-02-16T21:52:23,477][INFO ][o.e.p.PluginsService     ] [2z2SmG3] loaded module [analysis-common]
[2020-02-16T21:52:23,477][INFO ][o.e.p.PluginsService     ] [2z2SmG3] loaded module [ingest-common]
[2020-02-16T21:52:23,477][INFO ][o.e.p.PluginsService     ] [2z2SmG3] loaded module [ingest-geoip]
[2020-02-16T21:52:23,477][INFO ][o.e.p.PluginsService     ] [2z2SmG3] loaded module [ingest-user-agent]
[2020-02-16T21:52:23,477][INFO ][o.e.p.PluginsService     ] [2z2SmG3] loaded module [lang-expression]
[2020-02-16T21:52:23,478][INFO ][o.e.p.PluginsService     ] [2z2SmG3] loaded module [lang-mustache]
[2020-02-16T21:52:23,478][INFO ][o.e.p.PluginsService     ] [2z2SmG3] loaded module [lang-painless]
[2020-02-16T21:52:23,478][INFO ][o.e.p.PluginsService     ] [2z2SmG3] loaded module [mapper-extras]
[2020-02-16T21:52:23,478][INFO ][o.e.p.PluginsService     ] [2z2SmG3] loaded module [parent-join]
[2020-02-16T21:52:23,478][INFO ][o.e.p.PluginsService     ] [2z2SmG3] loaded module [percolator]
[2020-02-16T21:52:23,478][INFO ][o.e.p.PluginsService     ] [2z2SmG3] loaded module [rank-eval]
[2020-02-16T21:52:23,478][INFO ][o.e.p.PluginsService     ] [2z2SmG3] loaded module [reindex]
[2020-02-16T21:52:23,478][INFO ][o.e.p.PluginsService     ] [2z2SmG3] loaded module [repository-url]
[2020-02-16T21:52:23,478][INFO ][o.e.p.PluginsService     ] [2z2SmG3] loaded module [transport-netty4]
[2020-02-16T21:52:23,478][INFO ][o.e.p.PluginsService     ] [2z2SmG3] loaded module [tribe]
[2020-02-16T21:52:23,479][INFO ][o.e.p.PluginsService     ] [2z2SmG3] loaded module [x-pack-ccr]
[2020-02-16T21:52:23,479][INFO ][o.e.p.PluginsService     ] [2z2SmG3] loaded module [x-pack-core]
[2020-02-16T21:52:23,479][INFO ][o.e.p.PluginsService     ] [2z2SmG3] loaded module [x-pack-deprecation]
[2020-02-16T21:52:23,479][INFO ][o.e.p.PluginsService     ] [2z2SmG3] loaded module [x-pack-graph]
[2020-02-16T21:52:23,479][INFO ][o.e.p.PluginsService     ] [2z2SmG3] loaded module [x-pack-ilm]
[2020-02-16T21:52:23,479][INFO ][o.e.p.PluginsService     ] [2z2SmG3] loaded module [x-pack-logstash]
[2020-02-16T21:52:23,479][INFO ][o.e.p.PluginsService     ] [2z2SmG3] loaded module [x-pack-ml]
[2020-02-16T21:52:23,479][INFO ][o.e.p.PluginsService     ] [2z2SmG3] loaded module [x-pack-monitoring]
[2020-02-16T21:52:23,479][INFO ][o.e.p.PluginsService     ] [2z2SmG3] loaded module [x-pack-rollup]
[2020-02-16T21:52:23,479][INFO ][o.e.p.PluginsService     ] [2z2SmG3] loaded module [x-pack-security]
[2020-02-16T21:52:23,481][INFO ][o.e.p.PluginsService     ] [2z2SmG3] loaded module [x-pack-sql]
[2020-02-16T21:52:23,481][INFO ][o.e.p.PluginsService     ] [2z2SmG3] loaded module [x-pack-upgrade]
[2020-02-16T21:52:23,481][INFO ][o.e.p.PluginsService     ] [2z2SmG3] loaded module [x-pack-watcher]
[2020-02-16T21:52:23,482][INFO ][o.e.p.PluginsService     ] [2z2SmG3] no plugins loaded
[2020-02-16T21:52:30,089][INFO ][o.e.d.DiscoveryModule    ] [2z2SmG3] using discovery type [single-node] and host providers [settings]
[2020-02-16T21:52:31,468][INFO ][o.e.n.Node               ] [2z2SmG3] initialized
[2020-02-16T21:52:31,475][INFO ][o.e.n.Node               ] [2z2SmG3] starting ...
[2020-02-16T21:52:31,842][INFO ][o.e.t.TransportService   ] [2z2SmG3] publish_address {172.19.0.2:9300}, bound_addresses {0.0.0.0:9300}
[2020-02-16T21:52:32,041][INFO ][o.e.h.n.Netty4HttpServerTransport] [2z2SmG3] publish_address {172.19.0.2:9200}, bound_addresses {0.0.0.0:9200}
[2020-02-16T21:52:32,041][INFO ][o.e.n.Node               ] [2z2SmG3] started
[2020-02-16T21:52:32,184][INFO ][o.e.g.GatewayService     ] [2z2SmG3] recovered [0] indices into cluster_state
[2020-02-16T21:52:32,376][INFO ][o.e.l.LicenseService     ] [2z2SmG3] license [21bd01d5-d239-4ffe-a0ee-3525f98bf031] mode [basic] - valid
[2020-02-16T21:52:41,498][INFO ][o.e.c.m.MetaDataIndexTemplateService] [2z2SmG3] adding template [.management-beats] for index patterns [.management-beats]
[2020-02-16T21:52:43,244][INFO ][o.e.c.m.MetaDataIndexTemplateService] [2z2SmG3] adding template [.kibana_task_manager] for index patterns [.kibana_task_manager]
[2020-02-16T21:52:43,324][INFO ][o.e.c.m.MetaDataCreateIndexService] [2z2SmG3] [.kibana_task_manager] creating index, cause [auto(bulk api)], templates [.kibana_task_manager], shards [1]/[1], mappings [_doc]
[2020-02-16T21:52:43,345][INFO ][o.e.c.r.a.AllocationService] [2z2SmG3] updating number_of_replicas to [0] for indices [.kibana_task_manager]
[2020-02-16T21:52:43,655][WARN ][r.suppressed             ] [2z2SmG3] path: /.kibana_task_manager/_doc/_search, params: {ignore_unavailable=true, index=.kibana_task_manager, type=_doc}
org.elasticsearch.action.search.SearchPhaseExecutionException: all shards failed
        at org.elasticsearch.action.search.AbstractSearchAsyncAction.onPhaseFailure(AbstractSearchAsyncAction.java:296) [elasticsearch-6.8.6.jar:6.8.6]
        at org.elasticsearch.action.search.AbstractSearchAsyncAction.executeNextPhase(AbstractSearchAsyncAction.java:133) [elasticsearch-6.8.6.jar:6.8.6]
        at org.elasticsearch.action.search.AbstractSearchAsyncAction.onPhaseDone(AbstractSearchAsyncAction.java:259) [elasticsearch-6.8.6.jar:6.8.6]
        at org.elasticsearch.action.search.InitialSearchPhase.onShardFailure(InitialSearchPhase.java:100) [elasticsearch-6.8.6.jar:6.8.6]
        at org.elasticsearch.action.search.InitialSearchPhase.access$100(InitialSearchPhase.java:48) [elasticsearch-6.8.6.jar:6.8.6]
        at org.elasticsearch.action.search.InitialSearchPhase$2.lambda$onFailure$1(InitialSearchPhase.java:220) [elasticsearch-6.8.6.jar:6.8.6]
        at org.elasticsearch.action.search.InitialSearchPhase.maybeFork(InitialSearchPhase.java:174) [elasticsearch-6.8.6.jar:6.8.6]
        at org.elasticsearch.action.search.InitialSearchPhase.access$000(InitialSearchPhase.java:48) [elasticsearch-6.8.6.jar:6.8.6]
        at org.elasticsearch.action.search.InitialSearchPhase$2.onFailure(InitialSearchPhase.java:220) [elasticsearch-6.8.6.jar:6.8.6]
        at org.elasticsearch.action.search.SearchExecutionStatsCollector.onFailure(SearchExecutionStatsCollector.java:73) [elasticsearch-6.8.6.jar:6.8.6]
        at org.elasticsearch.action.ActionListenerResponseHandler.handleException(ActionListenerResponseHandler.java:59) [elasticsearch-6.8.6.jar:6.8.6]
        at org.elasticsearch.action.search.SearchTransportService$ConnectionCountingHandler.handleException(SearchTransportService.java:463) [elasticsearch-6.8.6.jar:6.8.6]
        at org.elasticsearch.transport.TransportService$ContextRestoreResponseHandler.handleException(TransportService.java:1114) [elasticsearch-6.8.6.jar:6.8.6]
        at org.elasticsearch.transport.TransportService$DirectResponseChannel.processException(TransportService.java:1226) [elasticsearch-6.8.6.jar:6.8.6]
        at org.elasticsearch.transport.TransportService$DirectResponseChannel.sendResponse(TransportService.java:1200) [elasticsearch-6.8.6.jar:6.8.6]
        at org.elasticsearch.transport.TaskTransportChannel.sendResponse(TaskTransportChannel.java:60) [elasticsearch-6.8.6.jar:6.8.6]
        at org.elasticsearch.action.support.ChannelActionListener.onFailure(ChannelActionListener.java:56) [elasticsearch-6.8.6.jar:6.8.6]
        at org.elasticsearch.search.SearchService$2.onFailure(SearchService.java:367) [elasticsearch-6.8.6.jar:6.8.6]
        at org.elasticsearch.search.SearchService$2.onResponse(SearchService.java:361) [elasticsearch-6.8.6.jar:6.8.6]
        at org.elasticsearch.search.SearchService$2.onResponse(SearchService.java:355) [elasticsearch-6.8.6.jar:6.8.6]
        at org.elasticsearch.search.SearchService$4.doRun(SearchService.java:1107) [elasticsearch-6.8.6.jar:6.8.6]
        at org.elasticsearch.common.util.concurrent.AbstractRunnable.run(AbstractRunnable.java:37) [elasticsearch-6.8.6.jar:6.8.6]
        at org.elasticsearch.common.util.concurrent.TimedRunnable.doRun(TimedRunnable.java:41) [elasticsearch-6.8.6.jar:6.8.6]
        at org.elasticsearch.common.util.concurrent.ThreadContext$ContextPreservingAbstractRunnable.doRun(ThreadContext.java:751) [elasticsearch-6.8.6.jar:6.8.6]
        at org.elasticsearch.common.util.concurrent.AbstractRunnable.run(AbstractRunnable.java:37) [elasticsearch-6.8.6.jar:6.8.6]
        at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1128) [?:?]
        at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:628) [?:?]
        at java.lang.Thread.run(Thread.java:830) [?:?]
[2020-02-16T21:52:43,655][WARN ][r.suppressed             ] [2z2SmG3] path: /.kibana_task_manager/_doc/_search, params: {ignore_unavailable=true, index=.kibana_task_manager, type=_doc}
org.elasticsearch.action.search.SearchPhaseExecutionException: all shards failed
        at org.elasticsearch.action.search.AbstractSearchAsyncAction.onPhaseFailure(AbstractSearchAsyncAction.java:296) [elasticsearch-6.8.6.jar:6.8.6]
        at org.elasticsearch.action.search.AbstractSearchAsyncAction.executeNextPhase(AbstractSearchAsyncAction.java:133) [elasticsearch-6.8.6.jar:6.8.6]
        at org.elasticsearch.action.search.AbstractSearchAsyncAction.onPhaseDone(AbstractSearchAsyncAction.java:259) [elasticsearch-6.8.6.jar:6.8.6]
        at org.elasticsearch.action.search.InitialSearchPhase.onShardFailure(InitialSearchPhase.java:100) [elasticsearch-6.8.6.jar:6.8.6]
        at org.elasticsearch.action.search.InitialSearchPhase.access$100(InitialSearchPhase.java:48) [elasticsearch-6.8.6.jar:6.8.6]
        at org.elasticsearch.action.search.InitialSearchPhase$2.lambda$onFailure$1(InitialSearchPhase.java:220) [elasticsearch-6.8.6.jar:6.8.6]
        at org.elasticsearch.action.search.InitialSearchPhase.maybeFork(InitialSearchPhase.java:174) [elasticsearch-6.8.6.jar:6.8.6]
        at org.elasticsearch.action.search.InitialSearchPhase.access$000(InitialSearchPhase.java:48) [elasticsearch-6.8.6.jar:6.8.6]
        at org.elasticsearch.action.search.InitialSearchPhase$2.onFailure(InitialSearchPhase.java:220) [elasticsearch-6.8.6.jar:6.8.6]
        at org.elasticsearch.action.search.SearchExecutionStatsCollector.onFailure(SearchExecutionStatsCollector.java:73) [elasticsearch-6.8.6.jar:6.8.6]
        at org.elasticsearch.action.ActionListenerResponseHandler.handleException(ActionListenerResponseHandler.java:59) [elasticsearch-6.8.6.jar:6.8.6]
        at org.elasticsearch.action.search.SearchTransportService$ConnectionCountingHandler.handleException(SearchTransportService.java:463) [elasticsearch-6.8.6.jar:6.8.6]
        at org.elasticsearch.transport.TransportService$ContextRestoreResponseHandler.handleException(TransportService.java:1114) [elasticsearch-6.8.6.jar:6.8.6]
        at org.elasticsearch.transport.TransportService$DirectResponseChannel.processException(TransportService.java:1226) [elasticsearch-6.8.6.jar:6.8.6]
        at org.elasticsearch.transport.TransportService$DirectResponseChannel.sendResponse(TransportService.java:1200) [elasticsearch-6.8.6.jar:6.8.6]
        at org.elasticsearch.transport.TaskTransportChannel.sendResponse(TaskTransportChannel.java:60) [elasticsearch-6.8.6.jar:6.8.6]
        at org.elasticsearch.action.support.ChannelActionListener.onFailure(ChannelActionListener.java:56) [elasticsearch-6.8.6.jar:6.8.6]
        at org.elasticsearch.search.SearchService$2.onFailure(SearchService.java:367) [elasticsearch-6.8.6.jar:6.8.6]
        at org.elasticsearch.search.SearchService$2.onResponse(SearchService.java:361) [elasticsearch-6.8.6.jar:6.8.6]
        at org.elasticsearch.search.SearchService$2.onResponse(SearchService.java:355) [elasticsearch-6.8.6.jar:6.8.6]
        at org.elasticsearch.search.SearchService$4.doRun(SearchService.java:1107) [elasticsearch-6.8.6.jar:6.8.6]
        at org.elasticsearch.common.util.concurrent.AbstractRunnable.run(AbstractRunnable.java:37) [elasticsearch-6.8.6.jar:6.8.6]
        at org.elasticsearch.common.util.concurrent.TimedRunnable.doRun(TimedRunnable.java:41) [elasticsearch-6.8.6.jar:6.8.6]
        at org.elasticsearch.common.util.concurrent.ThreadContext$ContextPreservingAbstractRunnable.doRun(ThreadContext.java:751) [elasticsearch-6.8.6.jar:6.8.6]
        at org.elasticsearch.common.util.concurrent.AbstractRunnable.run(AbstractRunnable.java:37) [elasticsearch-6.8.6.jar:6.8.6]
        at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1128) [?:?]
        at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:628) [?:?]
        at java.lang.Thread.run(Thread.java:830) [?:?]
[2020-02-16T21:52:43,781][INFO ][o.e.c.r.a.AllocationService] [2z2SmG3] Cluster health status changed from [YELLOW] to [GREEN] (reason: [shards started [[.kibana_task_manager][0]] ...]).
[2020-02-16T21:52:44,043][INFO ][o.e.c.m.MetaDataCreateIndexService] [2z2SmG3] [.kibana_1] creating index, cause [api], templates [], shards [1]/[1], mappings [doc]
[2020-02-16T21:52:44,046][INFO ][o.e.c.r.a.AllocationService] [2z2SmG3] updating number_of_replicas to [0] for indices [.kibana_1]
[2020-02-16T21:52:44,170][INFO ][o.e.c.r.a.AllocationService] [2z2SmG3] Cluster health status changed from [YELLOW] to [GREEN] (reason: [shards started [[.kibana_1][0]] ...]).
[2020-02-16T21:52:44,264][INFO ][o.e.c.m.MetaDataIndexTemplateService] [2z2SmG3] adding template [kibana_index_template:.kibana] for index patterns [.kibana]
[2020-02-16T21:52:44,308][INFO ][o.e.c.m.MetaDataIndexTemplateService] [2z2SmG3] adding template [kibana_index_template:.kibana] for index patterns [.kibana]
[2020-02-16T21:52:44,325][INFO ][o.e.c.m.MetaDataIndexTemplateService] [2z2SmG3] adding template [kibana_index_template:.kibana] for index patterns [.kibana]
[2020-02-16T21:52:44,441][INFO ][o.e.c.m.MetaDataIndexTemplateService] [2z2SmG3] adding template [kibana_index_template:.kibana] for index patterns [.kibana]
[2020-02-16T21:52:50,926][INFO ][o.e.c.m.MetaDataIndexTemplateService] [2z2SmG3] adding template [kibana_index_template:.kibana] for index patterns [.kibana]
[2020-02-16T21:52:50,997][INFO ][o.e.c.m.MetaDataMappingService] [2z2SmG3] [.kibana_1/fEaJMjsTTZK3HRd3tx1kcQ] update_mapping [doc]
[2020-02-16T21:52:52,228][INFO ][o.e.c.m.MetaDataIndexTemplateService] [2z2SmG3] adding template [kibana_index_template:.kibana] for index patterns [.kibana]
[2020-02-16T21:52:52,265][INFO ][o.e.c.m.MetaDataMappingService] [2z2SmG3] [.kibana_1/fEaJMjsTTZK3HRd3tx1kcQ] update_mapping [doc]
[2020-02-16T21:52:53,242][INFO ][o.e.c.m.MetaDataIndexTemplateService] [2z2SmG3] adding template [kibana_index_template:.kibana] for index patterns [.kibana]
[2020-02-16T21:52:54,266][INFO ][o.e.c.m.MetaDataIndexTemplateService] [2z2SmG3] adding template [kibana_index_template:.kibana] for index patterns [.kibana]
[2020-02-16T21:52:55,309][INFO ][o.e.c.m.MetaDataIndexTemplateService] [2z2SmG3] adding template [kibana_index_template:.kibana] for index patterns [.kibana]
[2020-02-16T21:52:56,318][INFO ][o.e.c.m.MetaDataIndexTemplateService] [2z2SmG3] adding template [kibana_index_template:.kibana] for index patterns [.kibana]
[2020-02-16T21:52:57,328][INFO ][o.e.c.m.MetaDataIndexTemplateService] [2z2SmG3] adding template [kibana_index_template:.kibana] for index patterns [.kibana]
[2020-02-16T21:52:58,341][INFO ][o.e.c.m.MetaDataIndexTemplateService] [2z2SmG3] adding template [kibana_index_template:.kibana] for index patterns [.kibana]
[2020-02-16T21:52:58,848][INFO ][o.e.c.m.MetaDataIndexTemplateService] [2z2SmG3] adding template [kibana_index_template:.kibana] for index patterns [.kibana]
[2020-02-16T21:52:59,356][INFO ][o.e.c.m.MetaDataIndexTemplateService] [2z2SmG3] adding template [kibana_index_template:.kibana] for index patterns [.kibana]
[2020-02-16T21:53:00,379][INFO ][o.e.c.m.MetaDataIndexTemplateService] [2z2SmG3] adding template [kibana_index_template:.kibana] for index patterns [.kibana]
[2020-02-16T21:53:01,405][INFO ][o.e.c.m.MetaDataIndexTemplateService] [2z2SmG3] adding template [kibana_index_template:.kibana] for index patterns [.kibana]
[2020-02-16T21:53:02,406][INFO ][o.e.c.m.MetaDataIndexTemplateService] [2z2SmG3] adding template [kibana_index_template:.kibana] for index patterns [.kibana]
[2020-02-16T21:53:03,464][INFO ][o.e.c.m.MetaDataIndexTemplateService] [2z2SmG3] adding template [kibana_index_template:.kibana] for index patterns [.kibana]
[2020-02-16T21:53:04,466][INFO ][o.e.c.m.MetaDataIndexTemplateService] [2z2SmG3] adding template [kibana_index_template:.kibana] for index patterns [.kibana]
[2020-02-16T21:53:05,477][INFO ][o.e.c.m.MetaDataIndexTemplateService] [2z2SmG3] adding template [kibana_index_template:.kibana] for index patterns [.kibana]
[2020-02-16T21:53:06,497][INFO ][o.e.c.m.MetaDataIndexTemplateService] [2z2SmG3] adding template [kibana_index_template:.kibana] for index patterns [.kibana]
[2020-02-16T21:53:07,518][INFO ][o.e.c.m.MetaDataIndexTemplateService] [2z2SmG3] adding template [kibana_index_template:.kibana] for index patterns [.kibana]
[2020-02-16T21:53:08,548][INFO ][o.e.c.m.MetaDataIndexTemplateService] [2z2SmG3] adding template [kibana_index_template:.kibana] for index patterns [.kibana]
[2020-02-16T21:53:09,561][INFO ][o.e.c.m.MetaDataIndexTemplateService] [2z2SmG3] adding template [kibana_index_template:.kibana] for index patterns [.kibana]
[2020-02-16T21:53:10,580][INFO ][o.e.c.m.MetaDataIndexTemplateService] [2z2SmG3] adding template [kibana_index_template:.kibana] for index patterns [.kibana]
[2020-02-16T21:53:11,592][INFO ][o.e.c.m.MetaDataIndexTemplateService] [2z2SmG3] adding template [kibana_index_template:.kibana] for index patterns [.kibana]
[2020-02-16T21:53:12,610][INFO ][o.e.c.m.MetaDataIndexTemplateService] [2z2SmG3] adding template [kibana_index_template:.kibana] for index patterns [.kibana]
[2020-02-16T21:53:13,622][INFO ][o.e.c.m.MetaDataIndexTemplateService] [2z2SmG3] adding template [kibana_index_template:.kibana] for index patterns [.kibana]
[2020-02-16T21:53:14,633][INFO ][o.e.c.m.MetaDataIndexTemplateService] [2z2SmG3] adding template [kibana_index_template:.kibana] for index patterns [.kibana]
[2020-02-16T21:53:15,645][INFO ][o.e.c.m.MetaDataIndexTemplateService] [2z2SmG3] adding template [kibana_index_template:.kibana] for index patterns [.kibana]
[2020-02-16T21:53:16,653][INFO ][o.e.c.m.MetaDataIndexTemplateService] [2z2SmG3] adding template [kibana_index_template:.kibana] for index patterns [.kibana]
[2020-02-16T21:53:18,753][WARN ][o.e.d.r.a.a.i.RestGetIndexTemplateAction] [2z2SmG3] [types removal] The parameter include_type_name should be explicitly specified in get template requests to prepare for 7.0. In 7.0 include_type_name will default to 'false', which means responses will omit the type name in mapping definitions.
[2020-02-16T21:53:18,865][WARN ][o.e.d.r.a.a.i.RestPutIndexTemplateAction] [2z2SmG3] [types removal] The parameter include_type_name should be explicitly specified in put template requests to prepare for 7.0. In 7.0 include_type_name will default to 'false', and requests are expected to omit the type name in mapping definitions.
[2020-02-16T21:53:18,952][INFO ][o.e.c.m.MetaDataIndexTemplateService] [2z2SmG3] adding template [metricbeat-6.8.6] for index patterns [metricbeat-6.8.6-*]
[2020-02-16T21:53:18,982][WARN ][o.e.d.c.m.MetaDataCreateIndexService] [2z2SmG3] the default number of shards will change from [5] to [1] in 7.0.0; if you wish to continue using the default of [5] shards, you must manage this on the create index request or with an index template
[2020-02-16T21:53:19,014][INFO ][o.e.c.m.MetaDataCreateIndexService] [2z2SmG3] [metricbeat-6.8.6-2020.02.16] creating index, cause [auto(bulk api)], templates [metricbeat-6.8.6], shards [5]/[1], mappings [doc]
```


Kibana Successful logs: 
```
PS C:\> docker logs d99aa24858df
{"type":"log","@timestamp":"2020-02-16T21:52:39Z","tags":["warning","elasticsearch","config","deprecation"],"pid":1,"message":"Config key \"url\" is deprecated. It has been replaced with \"hosts\""}
{"type":"log","@timestamp":"2020-02-16T21:52:39Z","tags":["status","plugin:kibana@6.8.6","info"],"pid":1,"state":"green","message":"Status changed from uninitialized to green - Ready","prevState":"uninitialized","prevMsg":"uninitialized"}
{"type":"log","@timestamp":"2020-02-16T21:52:40Z","tags":["status","plugin:elasticsearch@6.8.6","info"],"pid":1,"state":"yellow","message":"Status changed from uninitialized to yellow - Waiting for Elasticsearch","prevState":"uninitialized","prevMsg":"uninitialized"}
{"type":"log","@timestamp":"2020-02-16T21:52:40Z","tags":["status","plugin:xpack_main@6.8.6","info"],"pid":1,"state":"yellow","message":"Status changed from uninitialized to yellow - Waiting for Elasticsearch","prevState":"uninitialized","prevMsg":"uninitialized"}
{"type":"log","@timestamp":"2020-02-16T21:52:40Z","tags":["status","plugin:graph@6.8.6","info"],"pid":1,"state":"yellow","message":"Status changed from uninitialized to yellow - Waiting for Elasticsearch","prevState":"uninitialized","prevMsg":"uninitialized"}
{"type":"log","@timestamp":"2020-02-16T21:52:40Z","tags":["status","plugin:monitoring@6.8.6","info"],"pid":1,"state":"green","message":"Status changed from uninitialized to green - Ready","prevState":"uninitialized","prevMsg":"uninitialized"}
{"type":"log","@timestamp":"2020-02-16T21:52:40Z","tags":["status","plugin:spaces@6.8.6","info"],"pid":1,"state":"yellow","message":"Status changed from uninitialized to yellow - Waiting for Elasticsearch","prevState":"uninitialized","prevMsg":"uninitialized"}
{"type":"log","@timestamp":"2020-02-16T21:52:40Z","tags":["security","warning"],"pid":1,"message":"Generating a random key for xpack.security.encryptionKey. To prevent sessions from being invalidated on restart, please set xpack.security.encryptionKey in kibana.yml"}
{"type":"log","@timestamp":"2020-02-16T21:52:40Z","tags":["security","warning"],"pid":1,"message":"Session cookies will be transmitted over insecure connections. This is not recommended."}
{"type":"log","@timestamp":"2020-02-16T21:52:40Z","tags":["status","plugin:security@6.8.6","info"],"pid":1,"state":"yellow","message":"Status changed from uninitialized to yellow - Waiting for Elasticsearch","prevState":"uninitialized","prevMsg":"uninitialized"}
{"type":"log","@timestamp":"2020-02-16T21:52:40Z","tags":["status","plugin:searchprofiler@6.8.6","info"],"pid":1,"state":"yellow","message":"Status changed from uninitialized to yellow - Waiting for Elasticsearch","prevState":"uninitialized","prevMsg":"uninitialized"}
{"type":"log","@timestamp":"2020-02-16T21:52:40Z","tags":["status","plugin:ml@6.8.6","info"],"pid":1,"state":"yellow","message":"Status changed from uninitialized to yellow - Waiting for Elasticsearch","prevState":"uninitialized","prevMsg":"uninitialized"}
{"type":"log","@timestamp":"2020-02-16T21:52:40Z","tags":["status","plugin:tilemap@6.8.6","info"],"pid":1,"state":"yellow","message":"Status changed from uninitialized to yellow - Waiting for Elasticsearch","prevState":"uninitialized","prevMsg":"uninitialized"}
{"type":"log","@timestamp":"2020-02-16T21:52:40Z","tags":["status","plugin:watcher@6.8.6","info"],"pid":1,"state":"yellow","message":"Status changed from uninitialized to yellow - Waiting for Elasticsearch","prevState":"uninitialized","prevMsg":"uninitialized"}
{"type":"log","@timestamp":"2020-02-16T21:52:40Z","tags":["status","plugin:grokdebugger@6.8.6","info"],"pid":1,"state":"yellow","message":"Status changed from uninitialized to yellow - Waiting for Elasticsearch","prevState":"uninitialized","prevMsg":"uninitialized"}
{"type":"log","@timestamp":"2020-02-16T21:52:40Z","tags":["status","plugin:dashboard_mode@6.8.6","info"],"pid":1,"state":"green","message":"Status changed from uninitialized to green - Ready","prevState":"uninitialized","prevMsg":"uninitialized"}
{"type":"log","@timestamp":"2020-02-16T21:52:40Z","tags":["status","plugin:logstash@6.8.6","info"],"pid":1,"state":"yellow","message":"Status changed from uninitialized to yellow - Waiting for Elasticsearch","prevState":"uninitialized","prevMsg":"uninitialized"}
{"type":"log","@timestamp":"2020-02-16T21:52:40Z","tags":["status","plugin:beats_management@6.8.6","info"],"pid":1,"state":"yellow","message":"Status changed from uninitialized to yellow - Waiting for Elasticsearch","prevState":"uninitialized","prevMsg":"uninitialized"}
{"type":"log","@timestamp":"2020-02-16T21:52:40Z","tags":["status","plugin:apm@6.8.6","info"],"pid":1,"state":"green","message":"Status changed from uninitialized to green - Ready","prevState":"uninitialized","prevMsg":"uninitialized"}
{"type":"log","@timestamp":"2020-02-16T21:52:40Z","tags":["status","plugin:tile_map@6.8.6","info"],"pid":1,"state":"green","message":"Status changed from uninitialized to green - Ready","prevState":"uninitialized","prevMsg":"uninitialized"}
{"type":"log","@timestamp":"2020-02-16T21:52:40Z","tags":["status","plugin:task_manager@6.8.6","info"],"pid":1,"state":"green","message":"Status changed from uninitialized to green - Ready","prevState":"uninitialized","prevMsg":"uninitialized"}
{"type":"log","@timestamp":"2020-02-16T21:52:40Z","tags":["status","plugin:maps@6.8.6","info"],"pid":1,"state":"yellow","message":"Status changed from uninitialized to yellow - Waiting for Elasticsearch","prevState":"uninitialized","prevMsg":"uninitialized"}
{"type":"log","@timestamp":"2020-02-16T21:52:40Z","tags":["status","plugin:interpreter@6.8.6","info"],"pid":1,"state":"green","message":"Status changed from uninitialized to green - Ready","prevState":"uninitialized","prevMsg":"uninitialized"}
{"type":"log","@timestamp":"2020-02-16T21:52:40Z","tags":["status","plugin:canvas@6.8.6","info"],"pid":1,"state":"green","message":"Status changed from uninitialized to green - Ready","prevState":"uninitialized","prevMsg":"uninitialized"}
{"type":"log","@timestamp":"2020-02-16T21:52:40Z","tags":["status","plugin:license_management@6.8.6","info"],"pid":1,"state":"green","message":"Status changed from uninitialized to green - Ready","prevState":"uninitialized","prevMsg":"uninitialized"}
{"type":"log","@timestamp":"2020-02-16T21:52:40Z","tags":["status","plugin:cloud@6.8.6","info"],"pid":1,"state":"green","message":"Status changed from uninitialized to green - Ready","prevState":"uninitialized","prevMsg":"uninitialized"}
{"type":"log","@timestamp":"2020-02-16T21:52:40Z","tags":["status","plugin:index_management@6.8.6","info"],"pid":1,"state":"yellow","message":"Status changed from uninitialized to yellow - Waiting for Elasticsearch","prevState":"uninitialized","prevMsg":"uninitialized"}
{"type":"log","@timestamp":"2020-02-16T21:52:40Z","tags":["status","plugin:console@6.8.6","info"],"pid":1,"state":"green","message":"Status changed from uninitialized to green - Ready","prevState":"uninitialized","prevMsg":"uninitialized"}
{"type":"log","@timestamp":"2020-02-16T21:52:40Z","tags":["status","plugin:console_extensions@6.8.6","info"],"pid":1,"state":"green","message":"Status changed from uninitialized to green - Ready","prevState":"uninitialized","prevMsg":"uninitialized"}
{"type":"log","@timestamp":"2020-02-16T21:52:40Z","tags":["status","plugin:notifications@6.8.6","info"],"pid":1,"state":"green","message":"Status changed from uninitialized to green - Ready","prevState":"uninitialized","prevMsg":"uninitialized"}
{"type":"log","@timestamp":"2020-02-16T21:52:40Z","tags":["status","plugin:index_lifecycle_management@6.8.6","info"],"pid":1,"state":"yellow","message":"Status changed from uninitialized to yellow - Waiting for Elasticsearch","prevState":"uninitialized","prevMsg":"uninitialized"}
{"type":"log","@timestamp":"2020-02-16T21:52:40Z","tags":["status","plugin:infra@6.8.6","info"],"pid":1,"state":"green","message":"Status changed from uninitialized to green - Ready","prevState":"uninitialized","prevMsg":"uninitialized"}
{"type":"log","@timestamp":"2020-02-16T21:52:40Z","tags":["status","plugin:rollup@6.8.6","info"],"pid":1,"state":"yellow","message":"Status changed from uninitialized to yellow - Waiting for Elasticsearch","prevState":"uninitialized","prevMsg":"uninitialized"}
{"type":"log","@timestamp":"2020-02-16T21:52:40Z","tags":["status","plugin:remote_clusters@6.8.6","info"],"pid":1,"state":"yellow","message":"Status changed from uninitialized to yellow - Waiting for Elasticsearch","prevState":"uninitialized","prevMsg":"uninitialized"}
{"type":"log","@timestamp":"2020-02-16T21:52:40Z","tags":["status","plugin:cross_cluster_replication@6.8.6","info"],"pid":1,"state":"yellow","message":"Status changed from uninitialized to yellow - Waiting for Elasticsearch","prevState":"uninitialized","prevMsg":"uninitialized"}
{"type":"log","@timestamp":"2020-02-16T21:52:40Z","tags":["status","plugin:translations@6.8.6","info"],"pid":1,"state":"green","message":"Status changed from uninitialized to green - Ready","prevState":"uninitialized","prevMsg":"uninitialized"}
{"type":"log","@timestamp":"2020-02-16T21:52:40Z","tags":["status","plugin:upgrade_assistant@6.8.6","info"],"pid":1,"state":"green","message":"Status changed from uninitialized to green - Ready","prevState":"uninitialized","prevMsg":"uninitialized"}
{"type":"log","@timestamp":"2020-02-16T21:52:40Z","tags":["status","plugin:uptime@6.8.6","info"],"pid":1,"state":"green","message":"Status changed from uninitialized to green - Ready","prevState":"uninitialized","prevMsg":"uninitialized"}
{"type":"log","@timestamp":"2020-02-16T21:52:40Z","tags":["status","plugin:oss_telemetry@6.8.6","info"],"pid":1,"state":"green","message":"Status changed from uninitialized to green - Ready","prevState":"uninitialized","prevMsg":"uninitialized"}
{"type":"log","@timestamp":"2020-02-16T21:52:40Z","tags":["status","plugin:metrics@6.8.6","info"],"pid":1,"state":"green","message":"Status changed from uninitialized to green - Ready","prevState":"uninitialized","prevMsg":"uninitialized"}
{"type":"log","@timestamp":"2020-02-16T21:52:40Z","tags":["status","plugin:timelion@6.8.6","info"],"pid":1,"state":"green","message":"Status changed from uninitialized to green - Ready","prevState":"uninitialized","prevMsg":"uninitialized"}
{"type":"log","@timestamp":"2020-02-16T21:52:41Z","tags":["status","plugin:elasticsearch@6.8.6","info"],"pid":1,"state":"green","message":"Status changed from yellow to green - Ready","prevState":"yellow","prevMsg":"Waiting for Elasticsearch"}
{"type":"log","@timestamp":"2020-02-16T21:52:41Z","tags":["license","info","xpack"],"pid":1,"message":"Imported license information from Elasticsearch for the [data] cluster: mode: basic | status: active"}
{"type":"log","@timestamp":"2020-02-16T21:52:41Z","tags":["status","plugin:xpack_main@6.8.6","info"],"pid":1,"state":"green","message":"Status changed from yellow to green - Ready","prevState":"yellow","prevMsg":"Waiting for Elasticsearch"}
{"type":"log","@timestamp":"2020-02-16T21:52:41Z","tags":["status","plugin:graph@6.8.6","info"],"pid":1,"state":"green","message":"Status changed from yellow to green - Ready","prevState":"yellow","prevMsg":"Waiting for Elasticsearch"}
{"type":"log","@timestamp":"2020-02-16T21:52:41Z","tags":["status","plugin:searchprofiler@6.8.6","info"],"pid":1,"state":"green","message":"Status changed from yellow to green - Ready","prevState":"yellow","prevMsg":"Waiting for Elasticsearch"}
{"type":"log","@timestamp":"2020-02-16T21:52:41Z","tags":["status","plugin:ml@6.8.6","info"],"pid":1,"state":"green","message":"Status changed from yellow to green - Ready","prevState":"yellow","prevMsg":"Waiting for Elasticsearch"}
{"type":"log","@timestamp":"2020-02-16T21:52:41Z","tags":["status","plugin:tilemap@6.8.6","info"],"pid":1,"state":"green","message":"Status changed from yellow to green - Ready","prevState":"yellow","prevMsg":"Waiting for Elasticsearch"}
{"type":"log","@timestamp":"2020-02-16T21:52:41Z","tags":["status","plugin:watcher@6.8.6","info"],"pid":1,"state":"green","message":"Status changed from yellow to green - Ready","prevState":"yellow","prevMsg":"Waiting for Elasticsearch"}
{"type":"log","@timestamp":"2020-02-16T21:52:41Z","tags":["status","plugin:grokdebugger@6.8.6","info"],"pid":1,"state":"green","message":"Status changed from yellow to green - Ready","prevState":"yellow","prevMsg":"Waiting for Elasticsearch"}
{"type":"log","@timestamp":"2020-02-16T21:52:41Z","tags":["status","plugin:logstash@6.8.6","info"],"pid":1,"state":"green","message":"Status changed from yellow to green - Ready","prevState":"yellow","prevMsg":"Waiting for Elasticsearch"}
{"type":"log","@timestamp":"2020-02-16T21:52:41Z","tags":["status","plugin:beats_management@6.8.6","info"],"pid":1,"state":"green","message":"Status changed from yellow to green - Ready","prevState":"yellow","prevMsg":"Waiting for Elasticsearch"}
{"type":"log","@timestamp":"2020-02-16T21:52:41Z","tags":["status","plugin:index_management@6.8.6","info"],"pid":1,"state":"green","message":"Status changed from yellow to green - Ready","prevState":"yellow","prevMsg":"Waiting for Elasticsearch"}
{"type":"log","@timestamp":"2020-02-16T21:52:41Z","tags":["status","plugin:index_lifecycle_management@6.8.6","info"],"pid":1,"state":"green","message":"Status changed from yellow to green - Ready","prevState":"yellow","prevMsg":"Waiting for Elasticsearch"}
{"type":"log","@timestamp":"2020-02-16T21:52:41Z","tags":["status","plugin:rollup@6.8.6","info"],"pid":1,"state":"green","message":"Status changed from yellow to green - Ready","prevState":"yellow","prevMsg":"Waiting for Elasticsearch"}
{"type":"log","@timestamp":"2020-02-16T21:52:41Z","tags":["status","plugin:remote_clusters@6.8.6","info"],"pid":1,"state":"green","message":"Status changed from yellow to green - Ready","prevState":"yellow","prevMsg":"Waiting for Elasticsearch"}
{"type":"log","@timestamp":"2020-02-16T21:52:41Z","tags":["status","plugin:cross_cluster_replication@6.8.6","info"],"pid":1,"state":"green","message":"Status changed from yellow to green - Ready","prevState":"yellow","prevMsg":"Waiting for Elasticsearch"}
{"type":"log","@timestamp":"2020-02-16T21:52:41Z","tags":["info","monitoring-ui","kibana-monitoring"],"pid":1,"message":"Monitoring status upload endpoint is not enabled in Elasticsearch:Monitoring stats collection is stopped"}
{"type":"log","@timestamp":"2020-02-16T21:52:41Z","tags":["status","plugin:security@6.8.6","info"],"pid":1,"state":"green","message":"Status changed from yellow to green - Ready","prevState":"yellow","prevMsg":"Waiting for Elasticsearch"}
{"type":"log","@timestamp":"2020-02-16T21:52:41Z","tags":["status","plugin:maps@6.8.6","info"],"pid":1,"state":"green","message":"Status changed from yellow to green - Ready","prevState":"yellow","prevMsg":"Waiting for Elasticsearch"}
{"type":"log","@timestamp":"2020-02-16T21:52:41Z","tags":["license","info","xpack"],"pid":1,"message":"Imported license information from Elasticsearch for the [monitoring] cluster: mode: basic | status: active"}
{"type":"log","@timestamp":"2020-02-16T21:52:43Z","tags":["reporting","browser-driver","warning"],"pid":1,"message":"Enabling the Chromium sandbox provides an additional layer of protection."}
{"type":"log","@timestamp":"2020-02-16T21:52:43Z","tags":["reporting","warning"],"pid":1,"message":"Generating a random key for xpack.reporting.encryptionKey. To prevent pending reports from failing on restart, please set xpack.reporting.encryptionKey in kibana.yml"}
{"type":"log","@timestamp":"2020-02-16T21:52:43Z","tags":["status","plugin:reporting@6.8.6","info"],"pid":1,"state":"green","message":"Status changed from uninitialized to green - Ready","prevState":"uninitialized","prevMsg":"uninitialized"}
{"type":"log","@timestamp":"2020-02-16T21:52:43Z","tags":["info","task_manager"],"pid":1,"message":"Installing .kibana_task_manager index template version: 6080699."}
{"type":"log","@timestamp":"2020-02-16T21:52:43Z","tags":["info","task_manager"],"pid":1,"message":"Installed .kibana_task_manager index template: version 6080699 (API version 1)"}
{"type":"log","@timestamp":"2020-02-16T21:52:43Z","tags":["info","migrations"],"pid":1,"message":"Creating index .kibana_1."}
{"type":"log","@timestamp":"2020-02-16T21:52:44Z","tags":["info","migrations"],"pid":1,"message":"Pointing alias .kibana to .kibana_1."}
{"type":"log","@timestamp":"2020-02-16T21:52:44Z","tags":["info","migrations"],"pid":1,"message":"Finished in 231ms."}
{"type":"log","@timestamp":"2020-02-16T21:52:44Z","tags":["listening","info"],"pid":1,"message":"Server running at http://0:5601"}
{"type":"response","@timestamp":"2020-02-16T21:52:44Z","tags":[],"pid":1,"method":"get","statusCode":302,"req":{"url":"/","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","upgrade-insecure-requests":"1","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","sec-fetch-user":"?1","accept":"text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9","sec-fetch-site":"none","sec-fetch-mode":"navigate","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1"},"res":{"statusCode":302,"responseTime":12,"contentLength":9},"message":"GET / 302 12ms - 9.0B"}
{"type":"log","@timestamp":"2020-02-16T21:52:45Z","tags":["status","plugin:spaces@6.8.6","info"],"pid":1,"state":"green","message":"Status changed from yellow to green - Ready","prevState":"yellow","prevMsg":"Waiting for Elasticsearch"}
{"type":"response","@timestamp":"2020-02-16T21:52:44Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/app/kibana","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","upgrade-insecure-requests":"1","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","sec-fetch-user":"?1","accept":"text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9","sec-fetch-site":"none","sec-fetch-mode":"navigate","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1"},"res":{"statusCode":200,"responseTime":879,"contentLength":9},"message":"GET /app/kibana 200 879ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:45Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/bundles/app/status_page/bootstrap.js","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"*/*","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":16,"contentLength":9},"message":"GET /bundles/app/status_page/bootstrap.js 200 16ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:45Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/bundles/status_page.style.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":16,"contentLength":9},"message":"GET /bundles/status_page.style.css 200 16ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:45Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/built_assets/css/plugins/vega/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":27,"contentLength":9},"message":"GET /built_assets/css/plugins/vega/index.css 200 27ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:45Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/built_assets/css/plugins/timelion/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":25,"contentLength":9},"message":"GET /built_assets/css/plugins/timelion/index.css 200 25ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:45Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/built_assets/css/plugins/tagcloud/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":26,"contentLength":9},"message":"GET /built_assets/css/plugins/tagcloud/index.css 200 26ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:45Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/built_assets/css/plugins/tile_map/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":10,"contentLength":9},"message":"GET /built_assets/css/plugins/tile_map/index.css 200 10ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:45Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/built_assets/dlls/vendors.style.dll.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":42,"contentLength":9},"message":"GET /built_assets/dlls/vendors.style.dll.css 200 42ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:45Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/built_assets/css/plugins/table_vis/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":10,"contentLength":9},"message":"GET /built_assets/css/plugins/table_vis/index.css 200 10ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:45Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/built_assets/css/plugins/region_map/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":10,"contentLength":9},"message":"GET /built_assets/css/plugins/region_map/index.css 200 10ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:45Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/built_assets/css/plugins/metrics/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":16,"contentLength":9},"message":"GET /built_assets/css/plugins/metrics/index.css 200 16ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:45Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/built_assets/css/plugins/markdown_vis/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":10,"contentLength":9},"message":"GET /built_assets/css/plugins/markdown_vis/index.css 200 10ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:45Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/built_assets/css/plugins/metric_vis/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":10,"contentLength":9},"message":"GET /built_assets/css/plugins/metric_vis/index.css 200 10ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:45Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/built_assets/css/plugins/inspector_views/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":12,"contentLength":9},"message":"GET /built_assets/css/plugins/inspector_views/index.css 200 12ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:45Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/built_assets/css/plugins/input_control_vis/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":13,"contentLength":9},"message":"GET /built_assets/css/plugins/input_control_vis/index.css 200 13ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:45Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/built_assets/css/plugins/console/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":12,"contentLength":9},"message":"GET /built_assets/css/plugins/console/index.css 200 12ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:45Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/built_assets/css/plugins/upgrade_assistant/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":15,"contentLength":9},"message":"GET /built_assets/css/plugins/upgrade_assistant/index.css 200 15ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:45Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/bundles/commons.style.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":82,"contentLength":9},"message":"GET /bundles/commons.style.css 200 82ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:45Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/built_assets/css/plugins/cross_cluster_replication/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":20,"contentLength":9},"message":"GET /built_assets/css/plugins/cross_cluster_replication/index.css 200 20ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:45Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/built_assets/css/plugins/kibana/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":48,"contentLength":9},"message":"GET /built_assets/css/plugins/kibana/index.css 200 48ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:45Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/built_assets/css/plugins/remote_clusters/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":23,"contentLength":9},"message":"GET /built_assets/css/plugins/remote_clusters/index.css 200 23ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:45Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/built_assets/css/plugins/rollup/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":23,"contentLength":9},"message":"GET /built_assets/css/plugins/rollup/index.css 200 23ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:45Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/built_assets/css/plugins/infra/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":24,"contentLength":9},"message":"GET /built_assets/css/plugins/infra/index.css 200 24ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:45Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/built_assets/css/plugins/index_lifecycle_management/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":15,"contentLength":9},"message":"GET /built_assets/css/plugins/index_lifecycle_management/index.css 200 15ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:45Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/built_assets/css/plugins/index_management/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":16,"contentLength":9},"message":"GET /built_assets/css/plugins/index_management/index.css 200 16ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:45Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/built_assets/css/plugins/license_management/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":16,"contentLength":9},"message":"GET /built_assets/css/plugins/license_management/index.css 200 16ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:45Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/built_assets/css/plugins/apm/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":17,"contentLength":9},"message":"GET /built_assets/css/plugins/apm/index.css 200 17ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:45Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/built_assets/css/plugins/maps/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":19,"contentLength":9},"message":"GET /built_assets/css/plugins/maps/index.css 200 19ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:45Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/built_assets/css/plugins/canvas/style/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":25,"contentLength":9},"message":"GET /built_assets/css/plugins/canvas/style/index.css 200 25ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:45Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/built_assets/css/plugins/searchprofiler/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":10,"contentLength":9},"message":"GET /built_assets/css/plugins/searchprofiler/index.css 200 10ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:45Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/built_assets/css/plugins/watcher/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":13,"contentLength":9},"message":"GET /built_assets/css/plugins/watcher/index.css 200 13ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:45Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/built_assets/css/plugins/security/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":16,"contentLength":9},"message":"GET /built_assets/css/plugins/security/index.css 200 16ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:45Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/built_assets/css/plugins/spaces/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":17,"contentLength":9},"message":"GET /built_assets/css/plugins/spaces/index.css 200 17ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:45Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/built_assets/css/plugins/graph/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":17,"contentLength":9},"message":"GET /built_assets/css/plugins/graph/index.css 200 17ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:45Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/ui/favicons/favicon.ico","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"image/webp,image/apng,image/*,*/*;q=0.8","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":18,"contentLength":9},"message":"GET /ui/favicons/favicon.ico 200 18ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:45Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/built_assets/css/plugins/ml/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":38,"contentLength":9},"message":"GET /built_assets/css/plugins/ml/index.css 200 38ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:45Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/built_assets/css/plugins/monitoring/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":23,"contentLength":9},"message":"GET /built_assets/css/plugins/monitoring/index.css 200 23ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:45Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/ui/favicons/manifest.json","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"*/*","sec-fetch-site":"same-origin","sec-fetch-mode":"cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":14,"contentLength":9},"message":"GET /ui/favicons/manifest.json 200 14ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:45Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/built_assets/dlls/vendors.bundle.dll.js","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"*/*","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":600,"contentLength":9},"message":"GET /built_assets/dlls/vendors.bundle.dll.js 200 600ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:46Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/bundles/commons.bundle.js","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"*/*","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":251,"contentLength":9},"message":"GET /bundles/commons.bundle.js 200 251ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:46Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/bundles/status_page.bundle.js","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"*/*","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":4,"contentLength":9},"message":"GET /bundles/status_page.bundle.js 200 4ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:46Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/translations/en.json","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"*/*","sec-fetch-site":"same-origin","sec-fetch-mode":"cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":1,"contentLength":9},"message":"GET /translations/en.json 200 1ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:46Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/api/spaces/space","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","accept":"application/json, text/plain, */*","kbn-version":"6.8.6","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","sec-fetch-site":"same-origin","sec-fetch-mode":"cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":18,"contentLength":9},"message":"GET /api/spaces/space 200 18ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:46Z","tags":["api"],"pid":1,"method":"get","statusCode":200,"req":{"url":"/api/status","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"*/*","sec-fetch-site":"same-origin","sec-fetch-mode":"cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":13,"contentLength":9},"message":"GET /api/status 200 13ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:46Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/bundles/ebdca7741674eca4e1fadeca157f3ae6.svg","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"image/webp,image/apng,image/*,*/*;q=0.8","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/bundles/commons.style.css","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/bundles/commons.style.css"},"res":{"statusCode":200,"responseTime":5,"contentLength":9},"message":"GET /bundles/ebdca7741674eca4e1fadeca157f3ae6.svg 200 5ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:46Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/api/xpack/v1/info","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","accept":"application/json, text/plain, */*","kbn-version":"6.8.6","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","sec-fetch-site":"same-origin","sec-fetch-mode":"cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":3,"contentLength":9},"message":"GET /api/xpack/v1/info 200 3ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:46Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/ui/fonts/open_sans/open_sans_v15_latin_regular.woff2","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","origin":"http://localhost:5601","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"*/*","sec-fetch-site":"same-origin","sec-fetch-mode":"cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":7,"contentLength":9},"message":"GET /ui/fonts/open_sans/open_sans_v15_latin_regular.woff2 200 7ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:46Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/ui/fonts/open_sans/open_sans_v15_latin_600.woff2","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","origin":"http://localhost:5601","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"*/*","sec-fetch-site":"same-origin","sec-fetch-mode":"cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":3,"contentLength":9},"message":"GET /ui/fonts/open_sans/open_sans_v15_latin_600.woff2 200 3ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:46Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/ui/fonts/open_sans/open_sans_v15_latin_700.woff2","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","origin":"http://localhost:5601","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"*/*","sec-fetch-site":"same-origin","sec-fetch-mode":"cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":3,"contentLength":9},"message":"GET /ui/fonts/open_sans/open_sans_v15_latin_700.woff2 200 3ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:50Z","tags":["api"],"pid":1,"method":"get","statusCode":200,"req":{"url":"/api/status","method":"get","headers":{"host":"kibana:5601","user-agent":"Go-http-client/1.1","accept":"application/json","content-type":"application/json","accept-encoding":"gzip"},"remoteAddress":"172.19.0.4","userAgent":"172.19.0.4"},"res":{"statusCode":200,"responseTime":10,"contentLength":9},"message":"GET /api/status 200 10ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:50Z","tags":["api"],"pid":1,"method":"post","statusCode":200,"req":{"url":"/api/kibana/dashboards/import?force=true","method":"post","headers":{"host":"kibana:5601","user-agent":"Go-http-client/1.1","content-length":"399087","accept":"application/json","content-type":"application/json","kbn-version":"6.8.6","accept-encoding":"gzip"},"remoteAddress":"172.19.0.4","userAgent":"172.19.0.4"},"res":{"statusCode":200,"responseTime":1302,"contentLength":9},"message":"POST /api/kibana/dashboards/import?force=true 200 1302ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:52Z","tags":["api"],"pid":1,"method":"post","statusCode":200,"req":{"url":"/api/kibana/dashboards/import?exclude=index-pattern&force=true","method":"post","headers":{"host":"kibana:5601","user-agent":"Go-http-client/1.1","content-length":"12825","accept":"application/json","content-type":"application/json","kbn-version":"6.8.6","accept-encoding":"gzip"},"remoteAddress":"172.19.0.4","userAgent":"172.19.0.4"},"res":{"statusCode":200,"responseTime":1008,"contentLength":9},"message":"POST /api/kibana/dashboards/import?exclude=index-pattern&force=true 200 1008ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:53Z","tags":["api"],"pid":1,"method":"post","statusCode":200,"req":{"url":"/api/kibana/dashboards/import?exclude=index-pattern&force=true","method":"post","headers":{"host":"kibana:5601","user-agent":"Go-http-client/1.1","content-length":"9314","accept":"application/json","content-type":"application/json","kbn-version":"6.8.6","accept-encoding":"gzip"},"remoteAddress":"172.19.0.4","userAgent":"172.19.0.4"},"res":{"statusCode":200,"responseTime":1017,"contentLength":9},"message":"POST /api/kibana/dashboards/import?exclude=index-pattern&force=true 200 1017ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:55Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/app/kibana","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","cache-control":"max-age=0","upgrade-insecure-requests":"1","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","sec-fetch-user":"?1","accept":"text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9","sec-fetch-site":"same-origin","sec-fetch-mode":"navigate","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":25,"contentLength":9},"message":"GET /app/kibana 200 25ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:54Z","tags":["api"],"pid":1,"method":"post","statusCode":200,"req":{"url":"/api/kibana/dashboards/import?exclude=index-pattern&force=true","method":"post","headers":{"host":"kibana:5601","user-agent":"Go-http-client/1.1","content-length":"15825","accept":"application/json","content-type":"application/json","kbn-version":"6.8.6","accept-encoding":"gzip"},"remoteAddress":"172.19.0.4","userAgent":"172.19.0.4"},"res":{"statusCode":200,"responseTime":1034,"contentLength":9},"message":"POST /api/kibana/dashboards/import?exclude=index-pattern&force=true 200 1034ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:55Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/bundles/app/kibana/bootstrap.js","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"*/*","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":13,"contentLength":9},"message":"GET /bundles/app/kibana/bootstrap.js 200 13ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:55Z","tags":[],"pid":1,"method":"get","statusCode":304,"req":{"url":"/built_assets/dlls/vendors.style.dll.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6","if-none-match":"\"083029548ac61cc79cc8230e69969b8e70d072c8-/built_assets/dlls/-gzip\""},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":304,"responseTime":7,"contentLength":9},"message":"GET /built_assets/dlls/vendors.style.dll.css 304 7ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:55Z","tags":[],"pid":1,"method":"get","statusCode":304,"req":{"url":"/bundles/commons.style.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6","if-none-match":"\"8219dcd932eb2f81e2eefcec8e738c65d435b8d4-/bundles/-gzip\""},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":304,"responseTime":8,"contentLength":9},"message":"GET /bundles/commons.style.css 304 8ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:55Z","tags":[],"pid":1,"method":"get","statusCode":304,"req":{"url":"/built_assets/css/plugins/vega/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6","if-none-match":"\"a0b167aa4ab952016f2ea6f54927a559a73e1831-gzip\"","if-modified-since":"Fri, 13 Dec 2019 17:47:00 GMT"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":304,"responseTime":5,"contentLength":9},"message":"GET /built_assets/css/plugins/vega/index.css 304 5ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:55Z","tags":[],"pid":1,"method":"get","statusCode":304,"req":{"url":"/built_assets/css/plugins/timelion/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6","if-none-match":"\"b6215984bfb82e19e3bdefb254ca7c96d92b5cac-gzip\"","if-modified-since":"Fri, 13 Dec 2019 17:47:00 GMT"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":304,"responseTime":6,"contentLength":9},"message":"GET /built_assets/css/plugins/timelion/index.css 304 6ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:55Z","tags":[],"pid":1,"method":"get","statusCode":304,"req":{"url":"/built_assets/css/plugins/tagcloud/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6","if-none-match":"\"193c00ccd469bdac43247581dca195757554dca5-gzip\"","if-modified-since":"Fri, 13 Dec 2019 17:47:00 GMT"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":304,"responseTime":6,"contentLength":9},"message":"GET /built_assets/css/plugins/tagcloud/index.css 304 6ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:55Z","tags":[],"pid":1,"method":"get","statusCode":304,"req":{"url":"/built_assets/css/plugins/tile_map/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6","if-none-match":"\"93369eee7f9d1b81f1c700ed0f83a11b6e0f6550\"","if-modified-since":"Fri, 13 Dec 2019 17:47:00 GMT"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":304,"responseTime":3,"contentLength":9},"message":"GET /built_assets/css/plugins/tile_map/index.css 304 3ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:55Z","tags":[],"pid":1,"method":"get","statusCode":304,"req":{"url":"/built_assets/css/plugins/table_vis/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6","if-none-match":"\"0625b80a301a957f9504b55897001e844e7e5622-gzip\"","if-modified-since":"Fri, 13 Dec 2019 17:47:00 GMT"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":304,"responseTime":4,"contentLength":9},"message":"GET /built_assets/css/plugins/table_vis/index.css 304 4ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:55Z","tags":[],"pid":1,"method":"get","statusCode":304,"req":{"url":"/built_assets/css/plugins/metrics/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6","if-none-match":"\"72398b407556203f0bca82661ae022df4354e8d8-gzip\"","if-modified-since":"Fri, 13 Dec 2019 17:47:00 GMT"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":304,"responseTime":3,"contentLength":9},"message":"GET /built_assets/css/plugins/metrics/index.css 304 3ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:55Z","tags":[],"pid":1,"method":"get","statusCode":304,"req":{"url":"/built_assets/css/plugins/region_map/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6","if-none-match":"\"cd895e59d0811c884789f76681eca8bb9f1a6be7\"","if-modified-since":"Fri, 13 Dec 2019 17:47:00 GMT"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":304,"responseTime":4,"contentLength":9},"message":"GET /built_assets/css/plugins/region_map/index.css 304 4ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:55Z","tags":[],"pid":1,"method":"get","statusCode":304,"req":{"url":"/built_assets/css/plugins/markdown_vis/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6","if-none-match":"\"d7e300ab717618722b124581b87c0948a1e09c53\"","if-modified-since":"Fri, 13 Dec 2019 17:47:00 GMT"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":304,"responseTime":5,"contentLength":9},"message":"GET /built_assets/css/plugins/markdown_vis/index.css 304 5ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:55Z","tags":[],"pid":1,"method":"get","statusCode":304,"req":{"url":"/built_assets/css/plugins/metric_vis/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6","if-none-match":"\"1a3636f3cbb18b545efb928d0c17938cc3c45fd7-gzip\"","if-modified-since":"Fri, 13 Dec 2019 17:47:00 GMT"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":304,"responseTime":8,"contentLength":9},"message":"GET /built_assets/css/plugins/metric_vis/index.css 304 8ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:55Z","tags":[],"pid":1,"method":"get","statusCode":304,"req":{"url":"/built_assets/css/plugins/kibana/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6","if-none-match":"\"450fc2aee83099280b704eff272d2d11a2e2949a-gzip\"","if-modified-since":"Fri, 13 Dec 2019 17:47:00 GMT"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":304,"responseTime":8,"contentLength":9},"message":"GET /built_assets/css/plugins/kibana/index.css 304 8ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:55Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/bundles/kibana.style.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":47,"contentLength":9},"message":"GET /bundles/kibana.style.css 200 47ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:55Z","tags":[],"pid":1,"method":"get","statusCode":304,"req":{"url":"/built_assets/css/plugins/inspector_views/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6","if-none-match":"\"67da3df9174be70fc549badaeee80df847fbe016-gzip\"","if-modified-since":"Fri, 13 Dec 2019 17:47:00 GMT"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":304,"responseTime":4,"contentLength":9},"message":"GET /built_assets/css/plugins/inspector_views/index.css 304 4ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:55Z","tags":[],"pid":1,"method":"get","statusCode":304,"req":{"url":"/built_assets/css/plugins/input_control_vis/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6","if-none-match":"\"a62601f662fa6b0168055371c1e691201ffa6c0f-gzip\"","if-modified-since":"Fri, 13 Dec 2019 17:47:00 GMT"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":304,"responseTime":8,"contentLength":9},"message":"GET /built_assets/css/plugins/input_control_vis/index.css 304 8ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:55Z","tags":[],"pid":1,"method":"get","statusCode":304,"req":{"url":"/built_assets/css/plugins/console/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6","if-none-match":"\"e0c8034c9b3b84c11a03dcf65e3eced117766029-gzip\"","if-modified-since":"Fri, 13 Dec 2019 17:47:00 GMT"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":304,"responseTime":8,"contentLength":9},"message":"GET /built_assets/css/plugins/console/index.css 304 8ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:55Z","tags":[],"pid":1,"method":"get","statusCode":304,"req":{"url":"/built_assets/css/plugins/upgrade_assistant/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6","if-none-match":"\"6a8366ac4a99dd321d76d86a1f1a75f846aaed40-gzip\"","if-modified-since":"Fri, 13 Dec 2019 17:47:00 GMT"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":304,"responseTime":6,"contentLength":9},"message":"GET /built_assets/css/plugins/upgrade_assistant/index.css 304 6ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:55Z","tags":[],"pid":1,"method":"get","statusCode":304,"req":{"url":"/built_assets/css/plugins/cross_cluster_replication/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6","if-none-match":"\"8d4d56b4c304f068edc303e699a82063a1cebb14\"","if-modified-since":"Fri, 13 Dec 2019 17:47:00 GMT"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":304,"responseTime":7,"contentLength":9},"message":"GET /built_assets/css/plugins/cross_cluster_replication/index.css 304 7ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:55Z","tags":[],"pid":1,"method":"get","statusCode":304,"req":{"url":"/built_assets/css/plugins/remote_clusters/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6","if-none-match":"\"ccd819273cfd26c6e9757d1bfce38ada58a4e2f8-gzip\"","if-modified-since":"Fri, 13 Dec 2019 17:47:00 GMT"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":304,"responseTime":4,"contentLength":9},"message":"GET /built_assets/css/plugins/remote_clusters/index.css 304 4ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:55Z","tags":[],"pid":1,"method":"get","statusCode":304,"req":{"url":"/built_assets/css/plugins/rollup/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6","if-none-match":"\"004aa39ace18020f02b5b6a50879e9658ac5f5b1-gzip\"","if-modified-since":"Fri, 13 Dec 2019 17:47:00 GMT"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":304,"responseTime":4,"contentLength":9},"message":"GET /built_assets/css/plugins/rollup/index.css 304 4ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:55Z","tags":[],"pid":1,"method":"get","statusCode":304,"req":{"url":"/built_assets/css/plugins/infra/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6","if-none-match":"\"2a9d65535c1a958147aa4c7a745b9551c3ce3b2a-gzip\"","if-modified-since":"Fri, 13 Dec 2019 17:47:00 GMT"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":304,"responseTime":4,"contentLength":9},"message":"GET /built_assets/css/plugins/infra/index.css 304 4ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:55Z","tags":[],"pid":1,"method":"get","statusCode":304,"req":{"url":"/built_assets/css/plugins/index_lifecycle_management/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6","if-none-match":"\"f27b44af25a0c59006c73dcfb520a0fc2919c011\"","if-modified-since":"Fri, 13 Dec 2019 17:47:00 GMT"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":304,"responseTime":5,"contentLength":9},"message":"GET /built_assets/css/plugins/index_lifecycle_management/index.css 304 5ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:55Z","tags":[],"pid":1,"method":"get","statusCode":304,"req":{"url":"/built_assets/css/plugins/index_management/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6","if-none-match":"\"5d76b88cb611e105334abec0c41acb41c919646e\"","if-modified-since":"Fri, 13 Dec 2019 17:47:00 GMT"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":304,"responseTime":2,"contentLength":9},"message":"GET /built_assets/css/plugins/index_management/index.css 304 2ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:55Z","tags":[],"pid":1,"method":"get","statusCode":304,"req":{"url":"/built_assets/css/plugins/license_management/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6","if-none-match":"\"74a70d9f7922f6b5bc2952907d4a9270a37b1efd\"","if-modified-since":"Fri, 13 Dec 2019 17:47:00 GMT"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":304,"responseTime":2,"contentLength":9},"message":"GET /built_assets/css/plugins/license_management/index.css 304 2ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:55Z","tags":[],"pid":1,"method":"get","statusCode":304,"req":{"url":"/built_assets/css/plugins/canvas/style/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6","if-none-match":"\"d91c2ca8431f750e3c1c7a3d54abb2206bbda1d2-gzip\"","if-modified-since":"Fri, 13 Dec 2019 17:47:00 GMT"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":304,"responseTime":2,"contentLength":9},"message":"GET /built_assets/css/plugins/canvas/style/index.css 304 2ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:55Z","tags":[],"pid":1,"method":"get","statusCode":304,"req":{"url":"/built_assets/css/plugins/maps/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6","if-none-match":"\"98f6fa118d3000b88155ad48d9c2c02a7edfeef1-gzip\"","if-modified-since":"Fri, 13 Dec 2019 17:47:00 GMT"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":304,"responseTime":4,"contentLength":9},"message":"GET /built_assets/css/plugins/maps/index.css 304 4ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:55Z","tags":[],"pid":1,"method":"get","statusCode":304,"req":{"url":"/built_assets/css/plugins/apm/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6","if-none-match":"\"b19b0e62e02dbfc2816f3f16ea5a61676f053b16\"","if-modified-since":"Fri, 13 Dec 2019 17:47:00 GMT"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":304,"responseTime":4,"contentLength":9},"message":"GET /built_assets/css/plugins/apm/index.css 304 4ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:55Z","tags":[],"pid":1,"method":"get","statusCode":304,"req":{"url":"/built_assets/css/plugins/security/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6","if-none-match":"\"97e0a19414b265bf8eb8669065411cea2aab52f8-gzip\"","if-modified-since":"Fri, 13 Dec 2019 17:47:00 GMT"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":304,"responseTime":5,"contentLength":9},"message":"GET /built_assets/css/plugins/security/index.css 304 5ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:55Z","tags":[],"pid":1,"method":"get","statusCode":304,"req":{"url":"/built_assets/css/plugins/searchprofiler/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6","if-none-match":"\"915ffe4e0cbd4ba3e042a578ed4b519219c1cb89-gzip\"","if-modified-since":"Fri, 13 Dec 2019 17:47:00 GMT"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":304,"responseTime":5,"contentLength":9},"message":"GET /built_assets/css/plugins/searchprofiler/index.css 304 5ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:55Z","tags":[],"pid":1,"method":"get","statusCode":304,"req":{"url":"/built_assets/css/plugins/watcher/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6","if-none-match":"\"fcbc4e1f0aa6658723892c61aee23a676f4dd022-gzip\"","if-modified-since":"Fri, 13 Dec 2019 17:47:00 GMT"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":304,"responseTime":5,"contentLength":9},"message":"GET /built_assets/css/plugins/watcher/index.css 304 5ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:55Z","tags":[],"pid":1,"method":"get","statusCode":304,"req":{"url":"/built_assets/css/plugins/ml/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6","if-none-match":"\"4e878ea6cd3780fab29efc7edcf89356f60b273a-gzip\"","if-modified-since":"Fri, 13 Dec 2019 17:47:00 GMT"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":304,"responseTime":7,"contentLength":9},"message":"GET /built_assets/css/plugins/ml/index.css 304 7ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:55Z","tags":[],"pid":1,"method":"get","statusCode":304,"req":{"url":"/built_assets/css/plugins/spaces/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6","if-none-match":"\"5b59a2ae676dfff5e0248f0cff356b6a442a34a5-gzip\"","if-modified-since":"Fri, 13 Dec 2019 17:47:00 GMT"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":304,"responseTime":8,"contentLength":9},"message":"GET /built_assets/css/plugins/spaces/index.css 304 8ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:55Z","tags":[],"pid":1,"method":"get","statusCode":304,"req":{"url":"/built_assets/css/plugins/monitoring/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6","if-none-match":"\"aa87badfd68d6e9b2857589a2bb5c6ad8b6bc288-gzip\"","if-modified-since":"Fri, 13 Dec 2019 17:47:00 GMT"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":304,"responseTime":6,"contentLength":9},"message":"GET /built_assets/css/plugins/monitoring/index.css 304 6ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:55Z","tags":[],"pid":1,"method":"get","statusCode":304,"req":{"url":"/built_assets/css/plugins/graph/index.css","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"text/css,*/*;q=0.1","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6","if-none-match":"\"d5c85f4593f9f6815b812b9011cfa4e1e910430d-gzip\"","if-modified-since":"Fri, 13 Dec 2019 17:47:00 GMT"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":304,"responseTime":2,"contentLength":9},"message":"GET /built_assets/css/plugins/graph/index.css 304 2ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:55Z","tags":[],"pid":1,"method":"get","statusCode":304,"req":{"url":"/built_assets/dlls/vendors.bundle.dll.js","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"*/*","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6","if-none-match":"\"41e041b9362ed26f541380c51ca8278a445021a2-/built_assets/dlls/-gzip\""},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":304,"responseTime":6,"contentLength":9},"message":"GET /built_assets/dlls/vendors.bundle.dll.js 304 6ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:55Z","tags":[],"pid":1,"method":"get","statusCode":304,"req":{"url":"/ui/favicons/manifest.json","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"*/*","sec-fetch-site":"same-origin","sec-fetch-mode":"cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6","if-none-match":"\"5e276a366bb6b3befd4812917195267cf3013c0c\"","if-modified-since":"Fri, 13 Dec 2019 17:47:00 GMT"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":304,"responseTime":11,"contentLength":9},"message":"GET /ui/favicons/manifest.json 304 11ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:55Z","tags":[],"pid":1,"method":"get","statusCode":304,"req":{"url":"/bundles/commons.bundle.js","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"*/*","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6","if-none-match":"\"8125183250ae2804eb25f0382f516f5e50effd34-/bundles/-gzip\""},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":304,"responseTime":2,"contentLength":9},"message":"GET /bundles/commons.bundle.js 304 2ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:55Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/bundles/kibana.bundle.js","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"*/*","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":102,"contentLength":9},"message":"GET /bundles/kibana.bundle.js 200 102ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:56Z","tags":[],"pid":1,"method":"get","statusCode":304,"req":{"url":"/translations/en.json","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"*/*","sec-fetch-site":"same-origin","sec-fetch-mode":"cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6","if-none-match":"\"37992637719f97813c3068cfbf877b2d3bb43b97\""},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":304,"responseTime":0,"contentLength":9},"message":"GET /translations/en.json 304 0ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:55Z","tags":["api"],"pid":1,"method":"post","statusCode":200,"req":{"url":"/api/kibana/dashboards/import?exclude=index-pattern&force=true","method":"post","headers":{"host":"kibana:5601","user-agent":"Go-http-client/1.1","content-length":"7811","accept":"application/json","content-type":"application/json","kbn-version":"6.8.6","accept-encoding":"gzip"},"remoteAddress":"172.19.0.4","userAgent":"172.19.0.4"},"res":{"statusCode":200,"responseTime":1015,"contentLength":9},"message":"POST /api/kibana/dashboards/import?exclude=index-pattern&force=true 200 1015ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:56Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/api/spaces/space","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","accept":"application/json, text/plain, */*","kbn-version":"6.8.6","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","sec-fetch-site":"same-origin","sec-fetch-mode":"cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":10,"contentLength":9},"message":"GET /api/spaces/space 200 10ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:56Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/api/security/v1/me","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","accept":"application/json, text/plain, */*","kbn-version":"6.8.6","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","sec-fetch-site":"same-origin","sec-fetch-mode":"cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":0,"contentLength":9},"message":"GET /api/security/v1/me 200 0ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:56Z","tags":[],"pid":1,"method":"get","statusCode":304,"req":{"url":"/ui/fonts/open_sans/open_sans_v15_latin_regular.woff2","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","origin":"http://localhost:5601","if-none-match":"\"2c07a9656f1e38da408f20f1cf11581a15cbd7a2\"","if-modified-since":"Fri, 13 Dec 2019 17:47:00 GMT","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"*/*","sec-fetch-site":"same-origin","sec-fetch-mode":"cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":304,"responseTime":6,"contentLength":9},"message":"GET /ui/fonts/open_sans/open_sans_v15_latin_regular.woff2 304 6ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:56Z","tags":[],"pid":1,"method":"get","statusCode":304,"req":{"url":"/bundles/ebdca7741674eca4e1fadeca157f3ae6.svg","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"image/webp,image/apng,image/*,*/*;q=0.8","sec-fetch-site":"same-origin","sec-fetch-mode":"no-cors","referer":"http://localhost:5601/bundles/commons.style.css","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6","if-none-match":"\"eacd5acd1258d9b09e78dbc1958744f30e38bcbd-gzip\"","if-modified-since":"Fri, 13 Dec 2019 17:46:59 GMT"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/bundles/commons.style.css"},"res":{"statusCode":304,"responseTime":8,"contentLength":9},"message":"GET /bundles/ebdca7741674eca4e1fadeca157f3ae6.svg 304 8ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:56Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/api/console/api_server?sense_version=%40%40SENSE_VERSION&apis=es_6_0","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","accept":"application/json, text/javascript, */*; q=0.01","x-requested-with":"XMLHttpRequest","kbn-version":"6.8.6","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","sec-fetch-site":"same-origin","sec-fetch-mode":"cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":210,"contentLength":9},"message":"GET /api/console/api_server?sense_version=%40%40SENSE_VERSION&apis=es_6_0 200 210ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:56Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/api/saved_objects/_find?type=index-pattern&fields=title&search=*&search_fields=title&per_page=1&page=1","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","kbn-version":"6.8.6","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","content-type":"application/json","accept":"*/*","sec-fetch-site":"same-origin","sec-fetch-mode":"cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":36,"contentLength":9},"message":"GET /api/saved_objects/_find?type=index-pattern&fields=title&search=*&search_fields=title&per_page=1&page=1 200 36ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:56Z","tags":[],"pid":1,"method":"get","statusCode":304,"req":{"url":"/ui/fonts/open_sans/open_sans_v15_latin_600.woff2","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","origin":"http://localhost:5601","if-none-match":"\"24234c1c81b3948758c1a0be8e5a65386ca94c52\"","if-modified-since":"Fri, 13 Dec 2019 17:47:00 GMT","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"*/*","sec-fetch-site":"same-origin","sec-fetch-mode":"cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":304,"responseTime":1,"contentLength":9},"message":"GET /ui/fonts/open_sans/open_sans_v15_latin_600.woff2 304 1ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:56Z","tags":[],"pid":1,"method":"get","statusCode":304,"req":{"url":"/ui/fonts/open_sans/open_sans_v15_latin_700.woff2","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","origin":"http://localhost:5601","if-none-match":"\"5a6a45d6f98752b11ccb7c4f0f6fd7faf18ad1a7\"","if-modified-since":"Fri, 13 Dec 2019 17:47:00 GMT","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","accept":"*/*","sec-fetch-site":"same-origin","sec-fetch-mode":"cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":304,"responseTime":1,"contentLength":9},"message":"GET /ui/fonts/open_sans/open_sans_v15_latin_700.woff2 304 1ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:56Z","tags":["api"],"pid":1,"method":"post","statusCode":200,"req":{"url":"/api/kibana/dashboards/import?exclude=index-pattern&force=true","method":"post","headers":{"host":"kibana:5601","user-agent":"Go-http-client/1.1","content-length":"1740","accept":"application/json","content-type":"application/json","kbn-version":"6.8.6","accept-encoding":"gzip"},"remoteAddress":"172.19.0.4","userAgent":"172.19.0.4"},"res":{"statusCode":200,"responseTime":1009,"contentLength":9},"message":"POST /api/kibana/dashboards/import?exclude=index-pattern&force=true 200 1009ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:57Z","tags":["api"],"pid":1,"method":"post","statusCode":200,"req":{"url":"/api/kibana/dashboards/import?exclude=index-pattern&force=true","method":"post","headers":{"host":"kibana:5601","user-agent":"Go-http-client/1.1","content-length":"949","accept":"application/json","content-type":"application/json","kbn-version":"6.8.6","accept-encoding":"gzip"},"remoteAddress":"172.19.0.4","userAgent":"172.19.0.4"},"res":{"statusCode":200,"responseTime":1008,"contentLength":9},"message":"POST /api/kibana/dashboards/import?exclude=index-pattern&force=true 200 1008ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:58Z","tags":["api"],"pid":1,"method":"post","statusCode":200,"req":{"url":"/api/kibana/dashboards/import?exclude=index-pattern&force=true","method":"post","headers":{"host":"kibana:5601","user-agent":"Go-http-client/1.1","content-length":"2145","accept":"application/json","content-type":"application/json","kbn-version":"6.8.6","accept-encoding":"gzip"},"remoteAddress":"172.19.0.4","userAgent":"172.19.0.4"},"res":{"statusCode":200,"responseTime":1013,"contentLength":9},"message":"POST /api/kibana/dashboards/import?exclude=index-pattern&force=true 200 1013ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:58Z","tags":[],"pid":1,"method":"post","statusCode":200,"req":{"url":"/api/telemetry/v1/optIn","method":"post","headers":{"host":"localhost:5601","connection":"keep-alive","content-length":"17","accept":"application/json, text/plain, */*","origin":"http://localhost:5601","kbn-version":"6.8.6","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","content-type":"application/json;charset=UTF-8","sec-fetch-site":"same-origin","sec-fetch-mode":"cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":508,"contentLength":9},"message":"POST /api/telemetry/v1/optIn 200 508ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:52:59Z","tags":["api"],"pid":1,"method":"post","statusCode":200,"req":{"url":"/api/kibana/dashboards/import?exclude=index-pattern&force=true","method":"post","headers":{"host":"kibana:5601","user-agent":"Go-http-client/1.1","content-length":"1153","accept":"application/json","content-type":"application/json","kbn-version":"6.8.6","accept-encoding":"gzip"},"remoteAddress":"172.19.0.4","userAgent":"172.19.0.4"},"res":{"statusCode":200,"responseTime":1021,"contentLength":9},"message":"POST /api/kibana/dashboards/import?exclude=index-pattern&force=true 200 1021ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:53:00Z","tags":["api"],"pid":1,"method":"post","statusCode":200,"req":{"url":"/api/kibana/dashboards/import?exclude=index-pattern&force=true","method":"post","headers":{"host":"kibana:5601","user-agent":"Go-http-client/1.1","content-length":"1753","accept":"application/json","content-type":"application/json","kbn-version":"6.8.6","accept-encoding":"gzip"},"remoteAddress":"172.19.0.4","userAgent":"172.19.0.4"},"res":{"statusCode":200,"responseTime":1014,"contentLength":9},"message":"POST /api/kibana/dashboards/import?exclude=index-pattern&force=true 200 1014ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:53:01Z","tags":["api"],"pid":1,"method":"post","statusCode":200,"req":{"url":"/api/kibana/dashboards/import?exclude=index-pattern&force=true","method":"post","headers":{"host":"kibana:5601","user-agent":"Go-http-client/1.1","content-length":"1413","accept":"application/json","content-type":"application/json","kbn-version":"6.8.6","accept-encoding":"gzip"},"remoteAddress":"172.19.0.4","userAgent":"172.19.0.4"},"res":{"statusCode":200,"responseTime":1008,"contentLength":9},"message":"POST /api/kibana/dashboards/import?exclude=index-pattern&force=true 200 1008ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:53:03Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/api/saved_objects/_find?type=index-pattern&fields=title&fields=type&per_page=10000&page=1","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","kbn-version":"6.8.6","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","content-type":"application/json","accept":"*/*","sec-fetch-site":"same-origin","sec-fetch-mode":"cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":19,"contentLength":9},"message":"GET /api/saved_objects/_find?type=index-pattern&fields=title&fields=type&per_page=10000&page=1 200 19ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:53:03Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/api/saved_objects/_find?type=index-pattern&fields=title&fields=type&per_page=10000&page=1","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","kbn-version":"6.8.6","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","content-type":"application/json","accept":"*/*","sec-fetch-site":"same-origin","sec-fetch-mode":"cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":10,"contentLength":9},"message":"GET /api/saved_objects/_find?type=index-pattern&fields=title&fields=type&per_page=10000&page=1 200 10ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:53:02Z","tags":["api"],"pid":1,"method":"post","statusCode":200,"req":{"url":"/api/kibana/dashboards/import?exclude=index-pattern&force=true","method":"post","headers":{"host":"kibana:5601","user-agent":"Go-http-client/1.1","content-length":"29634","accept":"application/json","content-type":"application/json","kbn-version":"6.8.6","accept-encoding":"gzip"},"remoteAddress":"172.19.0.4","userAgent":"172.19.0.4"},"res":{"statusCode":200,"responseTime":1034,"contentLength":9},"message":"POST /api/kibana/dashboards/import?exclude=index-pattern&force=true 200 1034ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:53:03Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/api/rollup/indices","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","accept":"application/json, text/plain, */*","kbn-version":"6.8.6","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","sec-fetch-site":"same-origin","sec-fetch-mode":"cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":26,"contentLength":9},"message":"GET /api/rollup/indices 200 26ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:53:03Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/api/rollup/indices","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","accept":"application/json, text/plain, */*","kbn-version":"6.8.6","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","sec-fetch-site":"same-origin","sec-fetch-mode":"cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":11,"contentLength":9},"message":"GET /api/rollup/indices 200 11ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:53:03Z","tags":[],"pid":1,"method":"post","statusCode":200,"req":{"url":"/elasticsearch/*:*/_search?ignore_unavailable=true","method":"post","headers":{"host":"localhost:5601","connection":"keep-alive","content-length":"67","accept":"application/json, text/plain, */*","origin":"http://localhost:5601","kbn-version":"6.8.6","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","content-type":"application/json","sec-fetch-site":"same-origin","sec-fetch-mode":"cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":17,"contentLength":9},"message":"POST /elasticsearch/*:*/_search?ignore_unavailable=true 200 17ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:53:03Z","tags":[],"pid":1,"method":"post","statusCode":200,"req":{"url":"/elasticsearch/*/_search?ignore_unavailable=true","method":"post","headers":{"host":"localhost:5601","connection":"keep-alive","content-length":"69","accept":"application/json, text/plain, */*","origin":"http://localhost:5601","kbn-version":"6.8.6","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","content-type":"application/json","sec-fetch-site":"same-origin","sec-fetch-mode":"cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":72,"contentLength":9},"message":"POST /elasticsearch/*/_search?ignore_unavailable=true 200 72ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:53:03Z","tags":["api"],"pid":1,"method":"post","statusCode":200,"req":{"url":"/api/kibana/dashboards/import?exclude=index-pattern&force=true","method":"post","headers":{"host":"kibana:5601","user-agent":"Go-http-client/1.1","content-length":"49313","accept":"application/json","content-type":"application/json","kbn-version":"6.8.6","accept-encoding":"gzip"},"remoteAddress":"172.19.0.4","userAgent":"172.19.0.4"},"res":{"statusCode":200,"responseTime":1006,"contentLength":9},"message":"POST /api/kibana/dashboards/import?exclude=index-pattern&force=true 200 1006ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:53:04Z","tags":[],"pid":1,"method":"post","statusCode":200,"req":{"url":"/elasticsearch/*:*/_search?ignore_unavailable=true","method":"post","headers":{"host":"localhost:5601","connection":"keep-alive","content-length":"67","accept":"application/json, text/plain, */*","origin":"http://localhost:5601","kbn-version":"6.8.6","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","content-type":"application/json","sec-fetch-site":"same-origin","sec-fetch-mode":"cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":3,"contentLength":9},"message":"POST /elasticsearch/*:*/_search?ignore_unavailable=true 200 3ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:53:04Z","tags":[],"pid":1,"method":"post","statusCode":200,"req":{"url":"/elasticsearch/*/_search?ignore_unavailable=true","method":"post","headers":{"host":"localhost:5601","connection":"keep-alive","content-length":"69","accept":"application/json, text/plain, */*","origin":"http://localhost:5601","kbn-version":"6.8.6","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","content-type":"application/json","sec-fetch-site":"same-origin","sec-fetch-mode":"cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":8,"contentLength":9},"message":"POST /elasticsearch/*/_search?ignore_unavailable=true 200 8ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:53:04Z","tags":["api"],"pid":1,"method":"post","statusCode":200,"req":{"url":"/api/kibana/dashboards/import?exclude=index-pattern&force=true","method":"post","headers":{"host":"kibana:5601","user-agent":"Go-http-client/1.1","content-length":"23185","accept":"application/json","content-type":"application/json","kbn-version":"6.8.6","accept-encoding":"gzip"},"remoteAddress":"172.19.0.4","userAgent":"172.19.0.4"},"res":{"statusCode":200,"responseTime":1006,"contentLength":9},"message":"POST /api/kibana/dashboards/import?exclude=index-pattern&force=true 200 1006ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:53:05Z","tags":["api"],"pid":1,"method":"post","statusCode":200,"req":{"url":"/api/kibana/dashboards/import?exclude=index-pattern&force=true","method":"post","headers":{"host":"kibana:5601","user-agent":"Go-http-client/1.1","content-length":"7079","accept":"application/json","content-type":"application/json","kbn-version":"6.8.6","accept-encoding":"gzip"},"remoteAddress":"172.19.0.4","userAgent":"172.19.0.4"},"res":{"statusCode":200,"responseTime":1017,"contentLength":9},"message":"POST /api/kibana/dashboards/import?exclude=index-pattern&force=true 200 1017ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:53:06Z","tags":["api"],"pid":1,"method":"post","statusCode":200,"req":{"url":"/api/kibana/dashboards/import?exclude=index-pattern&force=true","method":"post","headers":{"host":"kibana:5601","user-agent":"Go-http-client/1.1","content-length":"28186","accept":"application/json","content-type":"application/json","kbn-version":"6.8.6","accept-encoding":"gzip"},"remoteAddress":"172.19.0.4","userAgent":"172.19.0.4"},"res":{"statusCode":200,"responseTime":1015,"contentLength":9},"message":"POST /api/kibana/dashboards/import?exclude=index-pattern&force=true 200 1015ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:53:07Z","tags":["api"],"pid":1,"method":"post","statusCode":200,"req":{"url":"/api/kibana/dashboards/import?exclude=index-pattern&force=true","method":"post","headers":{"host":"kibana:5601","user-agent":"Go-http-client/1.1","content-length":"20127","accept":"application/json","content-type":"application/json","kbn-version":"6.8.6","accept-encoding":"gzip"},"remoteAddress":"172.19.0.4","userAgent":"172.19.0.4"},"res":{"statusCode":200,"responseTime":1030,"contentLength":9},"message":"POST /api/kibana/dashboards/import?exclude=index-pattern&force=true 200 1030ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:53:08Z","tags":["api"],"pid":1,"method":"post","statusCode":200,"req":{"url":"/api/kibana/dashboards/import?exclude=index-pattern&force=true","method":"post","headers":{"host":"kibana:5601","user-agent":"Go-http-client/1.1","content-length":"13294","accept":"application/json","content-type":"application/json","kbn-version":"6.8.6","accept-encoding":"gzip"},"remoteAddress":"172.19.0.4","userAgent":"172.19.0.4"},"res":{"statusCode":200,"responseTime":1009,"contentLength":9},"message":"POST /api/kibana/dashboards/import?exclude=index-pattern&force=true 200 1009ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:53:09Z","tags":["api"],"pid":1,"method":"post","statusCode":200,"req":{"url":"/api/kibana/dashboards/import?exclude=index-pattern&force=true","method":"post","headers":{"host":"kibana:5601","user-agent":"Go-http-client/1.1","content-length":"7606","accept":"application/json","content-type":"application/json","kbn-version":"6.8.6","accept-encoding":"gzip"},"remoteAddress":"172.19.0.4","userAgent":"172.19.0.4"},"res":{"statusCode":200,"responseTime":1013,"contentLength":9},"message":"POST /api/kibana/dashboards/import?exclude=index-pattern&force=true 200 1013ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:53:10Z","tags":["api"],"pid":1,"method":"post","statusCode":200,"req":{"url":"/api/kibana/dashboards/import?exclude=index-pattern&force=true","method":"post","headers":{"host":"kibana:5601","user-agent":"Go-http-client/1.1","content-length":"14615","accept":"application/json","content-type":"application/json","kbn-version":"6.8.6","accept-encoding":"gzip"},"remoteAddress":"172.19.0.4","userAgent":"172.19.0.4"},"res":{"statusCode":200,"responseTime":1010,"contentLength":9},"message":"POST /api/kibana/dashboards/import?exclude=index-pattern&force=true 200 1010ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:53:11Z","tags":["api"],"pid":1,"method":"post","statusCode":200,"req":{"url":"/api/kibana/dashboards/import?exclude=index-pattern&force=true","method":"post","headers":{"host":"kibana:5601","user-agent":"Go-http-client/1.1","content-length":"12647","accept":"application/json","content-type":"application/json","kbn-version":"6.8.6","accept-encoding":"gzip"},"remoteAddress":"172.19.0.4","userAgent":"172.19.0.4"},"res":{"statusCode":200,"responseTime":1013,"contentLength":9},"message":"POST /api/kibana/dashboards/import?exclude=index-pattern&force=true 200 1013ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:53:12Z","tags":["api"],"pid":1,"method":"post","statusCode":200,"req":{"url":"/api/kibana/dashboards/import?exclude=index-pattern&force=true","method":"post","headers":{"host":"kibana:5601","user-agent":"Go-http-client/1.1","content-length":"22367","accept":"application/json","content-type":"application/json","kbn-version":"6.8.6","accept-encoding":"gzip"},"remoteAddress":"172.19.0.4","userAgent":"172.19.0.4"},"res":{"statusCode":200,"responseTime":1007,"contentLength":9},"message":"POST /api/kibana/dashboards/import?exclude=index-pattern&force=true 200 1007ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:53:13Z","tags":["api"],"pid":1,"method":"post","statusCode":200,"req":{"url":"/api/kibana/dashboards/import?exclude=index-pattern&force=true","method":"post","headers":{"host":"kibana:5601","user-agent":"Go-http-client/1.1","content-length":"3862","accept":"application/json","content-type":"application/json","kbn-version":"6.8.6","accept-encoding":"gzip"},"remoteAddress":"172.19.0.4","userAgent":"172.19.0.4"},"res":{"statusCode":200,"responseTime":1007,"contentLength":9},"message":"POST /api/kibana/dashboards/import?exclude=index-pattern&force=true 200 1007ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:53:14Z","tags":["api"],"pid":1,"method":"post","statusCode":200,"req":{"url":"/api/kibana/dashboards/import?exclude=index-pattern&force=true","method":"post","headers":{"host":"kibana:5601","user-agent":"Go-http-client/1.1","content-length":"10126","accept":"application/json","content-type":"application/json","kbn-version":"6.8.6","accept-encoding":"gzip"},"remoteAddress":"172.19.0.4","userAgent":"172.19.0.4"},"res":{"statusCode":200,"responseTime":1005,"contentLength":9},"message":"POST /api/kibana/dashboards/import?exclude=index-pattern&force=true 200 1005ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:53:15Z","tags":["api"],"pid":1,"method":"post","statusCode":200,"req":{"url":"/api/kibana/dashboards/import?exclude=index-pattern&force=true","method":"post","headers":{"host":"kibana:5601","user-agent":"Go-http-client/1.1","content-length":"9585","accept":"application/json","content-type":"application/json","kbn-version":"6.8.6","accept-encoding":"gzip"},"remoteAddress":"172.19.0.4","userAgent":"172.19.0.4"},"res":{"statusCode":200,"responseTime":1009,"contentLength":9},"message":"POST /api/kibana/dashboards/import?exclude=index-pattern&force=true 200 1009ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:53:16Z","tags":["api"],"pid":1,"method":"post","statusCode":200,"req":{"url":"/api/kibana/dashboards/import?exclude=index-pattern&force=true","method":"post","headers":{"host":"kibana:5601","user-agent":"Go-http-client/1.1","content-length":"12032","accept":"application/json","content-type":"application/json","kbn-version":"6.8.6","accept-encoding":"gzip"},"remoteAddress":"172.19.0.4","userAgent":"172.19.0.4"},"res":{"statusCode":200,"responseTime":1011,"contentLength":9},"message":"POST /api/kibana/dashboards/import?exclude=index-pattern&force=true 200 1011ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:53:25Z","tags":[],"pid":1,"method":"post","statusCode":200,"req":{"url":"/elasticsearch/*:*/_search?ignore_unavailable=true","method":"post","headers":{"host":"localhost:5601","connection":"keep-alive","content-length":"67","accept":"application/json, text/plain, */*","origin":"http://localhost:5601","kbn-version":"6.8.6","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","content-type":"application/json","sec-fetch-site":"same-origin","sec-fetch-mode":"cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":2,"contentLength":9},"message":"POST /elasticsearch/*:*/_search?ignore_unavailable=true 200 2ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:53:25Z","tags":[],"pid":1,"method":"post","statusCode":200,"req":{"url":"/elasticsearch/*/_search?ignore_unavailable=true","method":"post","headers":{"host":"localhost:5601","connection":"keep-alive","content-length":"69","accept":"application/json, text/plain, */*","origin":"http://localhost:5601","kbn-version":"6.8.6","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","content-type":"application/json","sec-fetch-site":"same-origin","sec-fetch-mode":"cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":9,"contentLength":9},"message":"POST /elasticsearch/*/_search?ignore_unavailable=true 200 9ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:53:25Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/api/saved_objects/_find?type=index-pattern&fields=title&per_page=10000&page=1","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","kbn-version":"6.8.6","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","content-type":"application/json","accept":"*/*","sec-fetch-site":"same-origin","sec-fetch-mode":"cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":15,"contentLength":9},"message":"GET /api/saved_objects/_find?type=index-pattern&fields=title&per_page=10000&page=1 200 15ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:53:30Z","tags":[],"pid":1,"method":"post","statusCode":200,"req":{"url":"/elasticsearch/*/_search?ignore_unavailable=true","method":"post","headers":{"host":"localhost:5601","connection":"keep-alive","content-length":"69","accept":"application/json, text/plain, */*","origin":"http://localhost:5601","kbn-version":"6.8.6","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","content-type":"application/json","sec-fetch-site":"same-origin","sec-fetch-mode":"cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":6,"contentLength":9},"message":"POST /elasticsearch/*/_search?ignore_unavailable=true 200 6ms - 9.0B"}
{"type":"response","@timestamp":"2020-02-16T21:53:32Z","tags":[],"pid":1,"method":"get","statusCode":200,"req":{"url":"/api/index_patterns/_fields_for_wildcard?pattern=*&meta_fields=_source&meta_fields=_id&meta_fields=_type&meta_fields=_index&meta_fields=_score","method":"get","headers":{"host":"localhost:5601","connection":"keep-alive","accept":"application/json, text/plain, */*","kbn-version":"6.8.6","user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36","sec-fetch-site":"same-origin","sec-fetch-mode":"cors","referer":"http://localhost:5601/app/kibana","accept-encoding":"gzip, deflate, br","accept-language":"en-US,en;q=0.9,sv;q=0.8,th;q=0.7,nb;q=0.6"},"remoteAddress":"172.19.0.1","userAgent":"172.19.0.1","referer":"http://localhost:5601/app/kibana"},"res":{"statusCode":200,"responseTime":115,"contentLength":9},"message":"GET /api/index_patterns/_fields_for_wildcard?pattern=*&meta_fields=_source&meta_fields=_id&meta_fields=_type&meta_fields=_index&meta_fields=_score 200 115ms - 9.0B"}
```



MetricBeat Successful logs: 
```
PS C:\> docker logs 4bbbe5bd8adc
2020-02-16T21:52:27.482Z        INFO    instance/beat.go:611    Home path: [/usr/share/metricbeat] Config path: [/usr/share/metricbeat] Data path: [/usr/share/metricbeat/data] Logs path: [/usr/share/metricbeat/logs]
2020-02-16T21:52:27.486Z        INFO    instance/beat.go:618    Beat UUID: e85367c4-d9c3-43bc-8286-7627d8161b73
2020-02-16T21:52:27.486Z        INFO    [seccomp]       seccomp/seccomp.go:116  Syscall filter successfully installed
2020-02-16T21:52:27.487Z        INFO    [beat]  instance/beat.go:931    Beat info       {"system_info": {"beat": {"path": {"config": "/usr/share/metricbeat", "data": "/usr/share/metricbeat/data", "home": "/usr/share/metricbeat", "logs": "/usr/share/metricbeat/logs"}, "type": "metricbeat", "uuid": "e85367c4-d9c3-43bc-8286-7627d8161b73"}}}
2020-02-16T21:52:27.487Z        INFO    [beat]  instance/beat.go:940    Build info      {"system_info": {"build": {"commit": "4fa63eb23a94bf23650023317bdff335c4705fc2", "libbeat": "6.8.6", "time": "2019-12-13T16:24:23.000Z", "version": "6.8.6"}}}
2020-02-16T21:52:27.487Z        INFO    [beat]  instance/beat.go:943    Go runtime info {"system_info": {"go": {"os":"linux","arch":"amd64","max_procs":2,"version":"go1.10.8"}}}
2020-02-16T21:52:27.492Z        INFO    [beat]  instance/beat.go:947    Host info       {"system_info": {"host": {"architecture":"x86_64","boot_time":"2020-02-16T21:40:50Z","containerized":true,"name":"4bbbe5bd8adc","ip":["127.0.0.1/8","172.19.0.4/16"],"kernel_version":"4.19.76-linuxkit","mac":["02:42:ac:13:00:04"],"os":{"family":"redhat","platform":"centos","name":"CentOS Linux","version":"7 (Core)","major":7,"minor":7,"patch":1908,"codename":"Core"},"timezone":"UTC","timezone_offset_sec":0}}}
2020-02-16T21:52:27.492Z        INFO    [beat]  instance/beat.go:976    Process info    {"system_info": {"process": {"capabilities": {"inheritable":["chown","dac_override","fowner","fsetid","kill","setgid","setuid","setpcap","net_bind_service","net_raw","sys_chroot","mknod","audit_write","setfcap"],"permitted":null,"effective":null,"bounding":["chown","dac_override","fowner","fsetid","kill","setgid","setuid","setpcap","net_bind_service","net_raw","sys_chroot","mknod","audit_write","setfcap"],"ambient":null}, "cwd": "/usr/share/metricbeat", "exe": "/usr/share/metricbeat/metricbeat", "name": "metricbeat", "pid": 1, "ppid": 0, "seccomp": {"mode":"filter","no_new_privs":true}, "start_time": "2020-02-16T21:52:26.860Z"}}}
2020-02-16T21:52:27.492Z        INFO    instance/beat.go:280    Setup Beat: metricbeat; Version: 6.8.6
2020-02-16T21:52:27.493Z        INFO    elasticsearch/client.go:164     Elasticsearch url: http://es:9200
2020-02-16T21:52:27.495Z        INFO    [publisher]     pipeline/module.go:110  Beat name: 4bbbe5bd8adc
2020-02-16T21:52:27.496Z        INFO    socket/socket.go:79     socket process info will only be available for metricbeat because the process is running as a non-root user
2020-02-16T21:52:27.497Z        INFO    [monitoring]    log/log.go:117  Starting metrics logging every 30s
2020-02-16T21:52:27.497Z        INFO    elasticsearch/client.go:164     Elasticsearch url: http://es:9200
2020-02-16T21:52:27.504Z        ERROR   elasticsearch/elasticsearch.go:252      Error connecting to Elasticsearch at http://es:9200: Get http://es:9200: dial tcp 172.19.0.2:9200: connect: connection refused
2020-02-16T21:52:27.506Z        INFO    [monitoring]    log/log.go:152  Total non-zero metrics  {"monitoring": {"metrics": {"beat":{"cpu":{"system":{"ticks":30,"time":{"ms":34}},"total":{"ticks":60,"time":{"ms":67},"value":60},"user":{"ticks":30,"time":{"ms":33}}},"handles":{"limit":{"hard":1048576,"soft":1048576},"open":5},"info":{"ephemeral_id":"2203de85-531b-4aac-979b-6d1ad32d9149","uptime":{"ms":119}},"memstats":{"gc_next":10190128,"memory_alloc":6003832,"memory_total":9185448,"rss":36241408}},"libbeat":{"config":{"module":{"running":0}},"output":{"type":"elasticsearch"},"pipeline":{"clients":0,"events":{"active":0}}},"system":{"cpu":{"cores":2},"load":{"1":1.51,"15":0.24,"5":0.62,"norm":{"1":0.755,"15":0.12,"5":0.31}}}}}}
2020-02-16T21:52:27.506Z        INFO    [monitoring]    log/log.go:153  Uptime: 120.7592ms
2020-02-16T21:52:27.506Z        INFO    [monitoring]    log/log.go:130  Stopping metrics logging.
2020-02-16T21:52:27.506Z        INFO    instance/beat.go:393    metricbeat stopped.
2020-02-16T21:52:27.506Z        ERROR   instance/beat.go:906    Exiting: Error importing Kibana dashboards: fail to create the Elasticsearch loader: Error creating Elasticsearch client: Couldn't connect to any of the configured Elasticsearch hosts. Errors: [Error connection to Elasticsearch http://es:9200: Get http://es:9200: dial tcp 172.19.0.2:9200: connect: connection refused]
Exiting: Error importing Kibana dashboards: fail to create the Elasticsearch loader: Error creating Elasticsearch client: Couldn't connect to any of the configured Elasticsearch hosts. Errors: [Error connection to Elasticsearch http://es:9200: Get http://es:9200: dial tcp 172.19.0.2:9200: connect: connection refused]
2020-02-16T21:52:28.375Z        INFO    instance/beat.go:611    Home path: [/usr/share/metricbeat] Config path: [/usr/share/metricbeat] Data path: [/usr/share/metricbeat/data] Logs path: [/usr/share/metricbeat/logs]
2020-02-16T21:52:28.378Z        INFO    instance/beat.go:618    Beat UUID: e85367c4-d9c3-43bc-8286-7627d8161b73
2020-02-16T21:52:28.378Z        INFO    [seccomp]       seccomp/seccomp.go:116  Syscall filter successfully installed
2020-02-16T21:52:28.378Z        INFO    [beat]  instance/beat.go:931    Beat info       {"system_info": {"beat": {"path": {"config": "/usr/share/metricbeat", "data": "/usr/share/metricbeat/data", "home": "/usr/share/metricbeat", "logs": "/usr/share/metricbeat/logs"}, "type": "metricbeat", "uuid": "e85367c4-d9c3-43bc-8286-7627d8161b73"}}}
2020-02-16T21:52:28.378Z        INFO    [beat]  instance/beat.go:940    Build info      {"system_info": {"build": {"commit": "4fa63eb23a94bf23650023317bdff335c4705fc2", "libbeat": "6.8.6", "time": "2019-12-13T16:24:23.000Z", "version": "6.8.6"}}}
2020-02-16T21:52:28.379Z        INFO    [beat]  instance/beat.go:943    Go runtime info {"system_info": {"go": {"os":"linux","arch":"amd64","max_procs":2,"version":"go1.10.8"}}}
2020-02-16T21:52:28.381Z        INFO    [beat]  instance/beat.go:947    Host info       {"system_info": {"host": {"architecture":"x86_64","boot_time":"2020-02-16T21:40:50Z","containerized":true,"name":"4bbbe5bd8adc","ip":["127.0.0.1/8","172.19.0.4/16"],"kernel_version":"4.19.76-linuxkit","mac":["02:42:ac:13:00:04"],"os":{"family":"redhat","platform":"centos","name":"CentOS Linux","version":"7 (Core)","major":7,"minor":7,"patch":1908,"codename":"Core"},"timezone":"UTC","timezone_offset_sec":0}}}
2020-02-16T21:52:28.381Z        INFO    [beat]  instance/beat.go:976    Process info    {"system_info": {"process": {"capabilities": {"inheritable":["chown","dac_override","fowner","fsetid","kill","setgid","setuid","setpcap","net_bind_service","net_raw","sys_chroot","mknod","audit_write","setfcap"],"permitted":null,"effective":null,"bounding":["chown","dac_override","fowner","fsetid","kill","setgid","setuid","setpcap","net_bind_service","net_raw","sys_chroot","mknod","audit_write","setfcap"],"ambient":null}, "cwd": "/usr/share/metricbeat", "exe": "/usr/share/metricbeat/metricbeat", "name": "metricbeat", "pid": 1, "ppid": 0, "seccomp": {"mode":"filter","no_new_privs":true}, "start_time": "2020-02-16T21:52:27.910Z"}}}
2020-02-16T21:52:28.381Z        INFO    instance/beat.go:280    Setup Beat: metricbeat; Version: 6.8.6
2020-02-16T21:52:28.381Z        INFO    elasticsearch/client.go:164     Elasticsearch url: http://es:9200
2020-02-16T21:52:28.381Z        INFO    [publisher]     pipeline/module.go:110  Beat name: 4bbbe5bd8adc
2020-02-16T21:52:28.382Z        INFO    socket/socket.go:79     socket process info will only be available for metricbeat because the process is running as a non-root user
2020-02-16T21:52:28.382Z        INFO    elasticsearch/client.go:164     Elasticsearch url: http://es:9200
2020-02-16T21:52:28.383Z        INFO    [monitoring]    log/log.go:117  Starting metrics logging every 30s
2020-02-16T21:52:28.384Z        ERROR   elasticsearch/elasticsearch.go:252      Error connecting to Elasticsearch at http://es:9200: Get http://es:9200: dial tcp 172.19.0.2:9200: connect: connection refused
2020-02-16T21:52:28.385Z        INFO    [monitoring]    log/log.go:152  Total non-zero metrics  {"monitoring": {"metrics": {"beat":{"cpu":{"system":{"ticks":10,"time":{"ms":17}},"total":{"ticks":30,"time":{"ms":45},"value":30},"user":{"ticks":20,"time":{"ms":28}}},"handles":{"limit":{"hard":1048576,"soft":1048576},"open":5},"info":{"ephemeral_id":"98796ace-26ce-4331-a72b-f9368dcbc7b6","uptime":{"ms":44}},"memstats":{"gc_next":7382864,"memory_alloc":4697640,"memory_total":9217952,"rss":37113856}},"libbeat":{"config":{"module":{"running":0}},"output":{"type":"elasticsearch"},"pipeline":{"clients":0,"events":{"active":0}}},"system":{"cpu":{"cores":2},"load":{"1":1.51,"15":0.24,"5":0.62,"norm":{"1":0.755,"15":0.12,"5":0.31}}}}}}
2020-02-16T21:52:28.385Z        INFO    [monitoring]    log/log.go:153  Uptime: 45.3385ms
2020-02-16T21:52:28.385Z        INFO    [monitoring]    log/log.go:130  Stopping metrics logging.
2020-02-16T21:52:28.385Z        INFO    instance/beat.go:393    metricbeat stopped.
2020-02-16T21:52:28.385Z        ERROR   instance/beat.go:906    Exiting: Error importing Kibana dashboards: fail to create the Elasticsearch loader: Error creating Elasticsearch client: Couldn't connect to any of the configured Elasticsearch hosts. Errors: [Error connection to Elasticsearch http://es:9200: Get http://es:9200: dial tcp 172.19.0.2:9200: connect: connection refused]
Exiting: Error importing Kibana dashboards: fail to create the Elasticsearch loader: Error creating Elasticsearch client: Couldn't connect to any of the configured Elasticsearch hosts. Errors: [Error connection to Elasticsearch http://es:9200: Get http://es:9200: dial tcp 172.19.0.2:9200: connect: connection refused]
2020-02-16T21:52:29.280Z        INFO    instance/beat.go:611    Home path: [/usr/share/metricbeat] Config path: [/usr/share/metricbeat] Data path: [/usr/share/metricbeat/data] Logs path: [/usr/share/metricbeat/logs]
2020-02-16T21:52:29.280Z        INFO    instance/beat.go:618    Beat UUID: e85367c4-d9c3-43bc-8286-7627d8161b73
2020-02-16T21:52:29.280Z        INFO    [seccomp]       seccomp/seccomp.go:116  Syscall filter successfully installed
2020-02-16T21:52:29.280Z        INFO    [beat]  instance/beat.go:931    Beat info       {"system_info": {"beat": {"path": {"config": "/usr/share/metricbeat", "data": "/usr/share/metricbeat/data", "home": "/usr/share/metricbeat", "logs": "/usr/share/metricbeat/logs"}, "type": "metricbeat", "uuid": "e85367c4-d9c3-43bc-8286-7627d8161b73"}}}
2020-02-16T21:52:29.280Z        INFO    [beat]  instance/beat.go:940    Build info      {"system_info": {"build": {"commit": "4fa63eb23a94bf23650023317bdff335c4705fc2", "libbeat": "6.8.6", "time": "2019-12-13T16:24:23.000Z", "version": "6.8.6"}}}
2020-02-16T21:52:29.280Z        INFO    [beat]  instance/beat.go:943    Go runtime info {"system_info": {"go": {"os":"linux","arch":"amd64","max_procs":2,"version":"go1.10.8"}}}
2020-02-16T21:52:29.281Z        INFO    [beat]  instance/beat.go:947    Host info       {"system_info": {"host": {"architecture":"x86_64","boot_time":"2020-02-16T21:40:50Z","containerized":true,"name":"4bbbe5bd8adc","ip":["127.0.0.1/8","172.19.0.4/16"],"kernel_version":"4.19.76-linuxkit","mac":["02:42:ac:13:00:04"],"os":{"family":"redhat","platform":"centos","name":"CentOS Linux","version":"7 (Core)","major":7,"minor":7,"patch":1908,"codename":"Core"},"timezone":"UTC","timezone_offset_sec":0}}}
2020-02-16T21:52:29.282Z        INFO    [beat]  instance/beat.go:976    Process info    {"system_info": {"process": {"capabilities": {"inheritable":["chown","dac_override","fowner","fsetid","kill","setgid","setuid","setpcap","net_bind_service","net_raw","sys_chroot","mknod","audit_write","setfcap"],"permitted":null,"effective":null,"bounding":["chown","dac_override","fowner","fsetid","kill","setgid","setuid","setpcap","net_bind_service","net_raw","sys_chroot","mknod","audit_write","setfcap"],"ambient":null}, "cwd": "/usr/share/metricbeat", "exe": "/usr/share/metricbeat/metricbeat", "name": "metricbeat", "pid": 1, "ppid": 0, "seccomp": {"mode":"filter","no_new_privs":true}, "start_time": "2020-02-16T21:52:28.800Z"}}}
2020-02-16T21:52:29.282Z        INFO    instance/beat.go:280    Setup Beat: metricbeat; Version: 6.8.6
2020-02-16T21:52:29.282Z        INFO    elasticsearch/client.go:164     Elasticsearch url: http://es:9200
2020-02-16T21:52:29.287Z        INFO    [publisher]     pipeline/module.go:110  Beat name: 4bbbe5bd8adc
2020-02-16T21:52:29.288Z        INFO    socket/socket.go:79     socket process info will only be available for metricbeat because the process is running as a non-root user
2020-02-16T21:52:29.293Z        INFO    [monitoring]    log/log.go:117  Starting metrics logging every 30s
2020-02-16T21:52:29.293Z        INFO    elasticsearch/client.go:164     Elasticsearch url: http://es:9200
2020-02-16T21:52:29.295Z        ERROR   elasticsearch/elasticsearch.go:252      Error connecting to Elasticsearch at http://es:9200: Get http://es:9200: dial tcp 172.19.0.2:9200: connect: connection refused
2020-02-16T21:52:29.298Z        INFO    [monitoring]    log/log.go:152  Total non-zero metrics  {"monitoring": {"metrics": {"beat":{"cpu":{"system":{"ticks":20,"time":{"ms":20}},"total":{"ticks":50,"time":{"ms":54},"value":50},"user":{"ticks":30,"time":{"ms":34}}},"handles":{"limit":{"hard":1048576,"soft":1048576},"open":5},"info":{"ephemeral_id":"97d31f92-6a25-4764-94a3-f95c770eae1f","uptime":{"ms":86}},"memstats":{"gc_next":5757840,"memory_alloc":3075528,"memory_total":9212840,"rss":36237312}},"libbeat":{"config":{"module":{"running":0}},"output":{"type":"elasticsearch"},"pipeline":{"clients":0,"events":{"active":0}}},"system":{"cpu":{"cores":2},"load":{"1":1.51,"15":0.24,"5":0.62,"norm":{"1":0.755,"15":0.12,"5":0.31}}}}}}
2020-02-16T21:52:29.298Z        INFO    [monitoring]    log/log.go:153  Uptime: 86.618ms
2020-02-16T21:52:29.298Z        INFO    [monitoring]    log/log.go:130  Stopping metrics logging.
2020-02-16T21:52:29.298Z        INFO    instance/beat.go:393    metricbeat stopped.
2020-02-16T21:52:29.298Z        ERROR   instance/beat.go:906    Exiting: Error importing Kibana dashboards: fail to create the Elasticsearch loader: Error creating Elasticsearch client: Couldn't connect to any of the configured Elasticsearch hosts. Errors: [Error connection to Elasticsearch http://es:9200: Get http://es:9200: dial tcp 172.19.0.2:9200: connect: connection refused]
Exiting: Error importing Kibana dashboards: fail to create the Elasticsearch loader: Error creating Elasticsearch client: Couldn't connect to any of the configured Elasticsearch hosts. Errors: [Error connection to Elasticsearch http://es:9200: Get http://es:9200: dial tcp 172.19.0.2:9200: connect: connection refused]
2020-02-16T21:52:30.426Z        INFO    instance/beat.go:611    Home path: [/usr/share/metricbeat] Config path: [/usr/share/metricbeat] Data path: [/usr/share/metricbeat/data] Logs path: [/usr/share/metricbeat/logs]
2020-02-16T21:52:30.426Z        INFO    instance/beat.go:618    Beat UUID: e85367c4-d9c3-43bc-8286-7627d8161b73
2020-02-16T21:52:30.427Z        INFO    [seccomp]       seccomp/seccomp.go:116  Syscall filter successfully installed
2020-02-16T21:52:30.427Z        INFO    [beat]  instance/beat.go:931    Beat info       {"system_info": {"beat": {"path": {"config": "/usr/share/metricbeat", "data": "/usr/share/metricbeat/data", "home": "/usr/share/metricbeat", "logs": "/usr/share/metricbeat/logs"}, "type": "metricbeat", "uuid": "e85367c4-d9c3-43bc-8286-7627d8161b73"}}}
2020-02-16T21:52:30.427Z        INFO    [beat]  instance/beat.go:940    Build info      {"system_info": {"build": {"commit": "4fa63eb23a94bf23650023317bdff335c4705fc2", "libbeat": "6.8.6", "time": "2019-12-13T16:24:23.000Z", "version": "6.8.6"}}}
2020-02-16T21:52:30.427Z        INFO    [beat]  instance/beat.go:943    Go runtime info {"system_info": {"go": {"os":"linux","arch":"amd64","max_procs":2,"version":"go1.10.8"}}}
2020-02-16T21:52:30.428Z        INFO    [beat]  instance/beat.go:947    Host info       {"system_info": {"host": {"architecture":"x86_64","boot_time":"2020-02-16T21:40:50Z","containerized":true,"name":"4bbbe5bd8adc","ip":["127.0.0.1/8","172.19.0.4/16"],"kernel_version":"4.19.76-linuxkit","mac":["02:42:ac:13:00:04"],"os":{"family":"redhat","platform":"centos","name":"CentOS Linux","version":"7 (Core)","major":7,"minor":7,"patch":1908,"codename":"Core"},"timezone":"UTC","timezone_offset_sec":0}}}
2020-02-16T21:52:30.428Z        INFO    [beat]  instance/beat.go:976    Process info    {"system_info": {"process": {"capabilities": {"inheritable":["chown","dac_override","fowner","fsetid","kill","setgid","setuid","setpcap","net_bind_service","net_raw","sys_chroot","mknod","audit_write","setfcap"],"permitted":null,"effective":null,"bounding":["chown","dac_override","fowner","fsetid","kill","setgid","setuid","setpcap","net_bind_service","net_raw","sys_chroot","mknod","audit_write","setfcap"],"ambient":null}, "cwd": "/usr/share/metricbeat", "exe": "/usr/share/metricbeat/metricbeat", "name": "metricbeat", "pid": 1, "ppid": 0, "seccomp": {"mode":"filter","no_new_privs":true}, "start_time": "2020-02-16T21:52:29.910Z"}}}
2020-02-16T21:52:30.428Z        INFO    instance/beat.go:280    Setup Beat: metricbeat; Version: 6.8.6
2020-02-16T21:52:30.429Z        INFO    elasticsearch/client.go:164     Elasticsearch url: http://es:9200
2020-02-16T21:52:30.434Z        INFO    [publisher]     pipeline/module.go:110  Beat name: 4bbbe5bd8adc
2020-02-16T21:52:30.435Z        INFO    socket/socket.go:79     socket process info will only be available for metricbeat because the process is running as a non-root user
2020-02-16T21:52:30.435Z        INFO    elasticsearch/client.go:164     Elasticsearch url: http://es:9200
2020-02-16T21:52:30.435Z        INFO    [monitoring]    log/log.go:117  Starting metrics logging every 30s
2020-02-16T21:52:30.436Z        ERROR   elasticsearch/elasticsearch.go:252      Error connecting to Elasticsearch at http://es:9200: Get http://es:9200: dial tcp 172.19.0.2:9200: connect: connection refused
2020-02-16T21:52:30.439Z        INFO    [monitoring]    log/log.go:152  Total non-zero metrics  {"monitoring": {"metrics": {"beat":{"cpu":{"system":{"ticks":10,"time":{"ms":13}},"total":{"ticks":40,"time":{"ms":50},"value":40},"user":{"ticks":30,"time":{"ms":37}}},"handles":{"limit":{"hard":1048576,"soft":1048576},"open":5},"info":{"ephemeral_id":"1e068f9a-11df-4169-96d5-57869b57c124","uptime":{"ms":59}},"memstats":{"gc_next":6028096,"memory_alloc":3215944,"memory_total":9526360,"rss":37490688}},"libbeat":{"config":{"module":{"running":0}},"output":{"type":"elasticsearch"},"pipeline":{"clients":0,"events":{"active":0}}},"system":{"cpu":{"cores":2},"load":{"1":1.51,"15":0.24,"5":0.62,"norm":{"1":0.755,"15":0.12,"5":0.31}}}}}}
2020-02-16T21:52:30.442Z        INFO    [monitoring]    log/log.go:153  Uptime: 65.322799ms
2020-02-16T21:52:30.442Z        INFO    [monitoring]    log/log.go:130  Stopping metrics logging.
2020-02-16T21:52:30.442Z        INFO    instance/beat.go:393    metricbeat stopped.
2020-02-16T21:52:30.442Z        ERROR   instance/beat.go:906    Exiting: Error importing Kibana dashboards: fail to create the Elasticsearch loader: Error creating Elasticsearch client: Couldn't connect to any of the configured Elasticsearch hosts. Errors: [Error connection to Elasticsearch http://es:9200: Get http://es:9200: dial tcp 172.19.0.2:9200: connect: connection refused]
Exiting: Error importing Kibana dashboards: fail to create the Elasticsearch loader: Error creating Elasticsearch client: Couldn't connect to any of the configured Elasticsearch hosts. Errors: [Error connection to Elasticsearch http://es:9200: Get http://es:9200: dial tcp 172.19.0.2:9200: connect: connection refused]
2020-02-16T21:52:31.803Z        INFO    instance/beat.go:611    Home path: [/usr/share/metricbeat] Config path: [/usr/share/metricbeat] Data path: [/usr/share/metricbeat/data] Logs path: [/usr/share/metricbeat/logs]
2020-02-16T21:52:31.803Z        INFO    instance/beat.go:618    Beat UUID: e85367c4-d9c3-43bc-8286-7627d8161b73
2020-02-16T21:52:31.804Z        INFO    [seccomp]       seccomp/seccomp.go:116  Syscall filter successfully installed
2020-02-16T21:52:31.804Z        INFO    [beat]  instance/beat.go:931    Beat info       {"system_info": {"beat": {"path": {"config": "/usr/share/metricbeat", "data": "/usr/share/metricbeat/data", "home": "/usr/share/metricbeat", "logs": "/usr/share/metricbeat/logs"}, "type": "metricbeat", "uuid": "e85367c4-d9c3-43bc-8286-7627d8161b73"}}}
2020-02-16T21:52:31.804Z        INFO    [beat]  instance/beat.go:940    Build info      {"system_info": {"build": {"commit": "4fa63eb23a94bf23650023317bdff335c4705fc2", "libbeat": "6.8.6", "time": "2019-12-13T16:24:23.000Z", "version": "6.8.6"}}}
2020-02-16T21:52:31.804Z        INFO    [beat]  instance/beat.go:943    Go runtime info {"system_info": {"go": {"os":"linux","arch":"amd64","max_procs":2,"version":"go1.10.8"}}}
2020-02-16T21:52:31.805Z        INFO    [beat]  instance/beat.go:947    Host info       {"system_info": {"host": {"architecture":"x86_64","boot_time":"2020-02-16T21:40:50Z","containerized":true,"name":"4bbbe5bd8adc","ip":["127.0.0.1/8","172.19.0.4/16"],"kernel_version":"4.19.76-linuxkit","mac":["02:42:ac:13:00:04"],"os":{"family":"redhat","platform":"centos","name":"CentOS Linux","version":"7 (Core)","major":7,"minor":7,"patch":1908,"codename":"Core"},"timezone":"UTC","timezone_offset_sec":0}}}
2020-02-16T21:52:31.806Z        INFO    [beat]  instance/beat.go:976    Process info    {"system_info": {"process": {"capabilities": {"inheritable":["chown","dac_override","fowner","fsetid","kill","setgid","setuid","setpcap","net_bind_service","net_raw","sys_chroot","mknod","audit_write","setfcap"],"permitted":null,"effective":null,"bounding":["chown","dac_override","fowner","fsetid","kill","setgid","setuid","setpcap","net_bind_service","net_raw","sys_chroot","mknod","audit_write","setfcap"],"ambient":null}, "cwd": "/usr/share/metricbeat", "exe": "/usr/share/metricbeat/metricbeat", "name": "metricbeat", "pid": 1, "ppid": 0, "seccomp": {"mode":"filter","no_new_privs":true}, "start_time": "2020-02-16T21:52:31.420Z"}}}
2020-02-16T21:52:31.806Z        INFO    instance/beat.go:280    Setup Beat: metricbeat; Version: 6.8.6
2020-02-16T21:52:31.806Z        INFO    elasticsearch/client.go:164     Elasticsearch url: http://es:9200
2020-02-16T21:52:31.806Z        INFO    [publisher]     pipeline/module.go:110  Beat name: 4bbbe5bd8adc
2020-02-16T21:52:31.807Z        INFO    socket/socket.go:79     socket process info will only be available for metricbeat because the process is running as a non-root user
2020-02-16T21:52:31.807Z        INFO    [monitoring]    log/log.go:117  Starting metrics logging every 30s
2020-02-16T21:52:31.807Z        INFO    elasticsearch/client.go:164     Elasticsearch url: http://es:9200
2020-02-16T21:52:31.809Z        ERROR   elasticsearch/elasticsearch.go:252      Error connecting to Elasticsearch at http://es:9200: Get http://es:9200: dial tcp 172.19.0.2:9200: connect: connection refused
2020-02-16T21:52:31.811Z        INFO    [monitoring]    log/log.go:152  Total non-zero metrics  {"monitoring": {"metrics": {"beat":{"cpu":{"system":{"ticks":10,"time":{"ms":15}},"total":{"ticks":30,"time":{"ms":43},"value":30},"user":{"ticks":20,"time":{"ms":28}}},"handles":{"limit":{"hard":1048576,"soft":1048576},"open":5},"info":{"ephemeral_id":"eccb77cd-4fe0-4971-a4f8-756c0983b615","uptime":{"ms":34}},"memstats":{"gc_next":5943728,"memory_alloc":7630800,"memory_total":9227760,"rss":38453248}},"libbeat":{"config":{"module":{"running":0}},"output":{"type":"elasticsearch"},"pipeline":{"clients":0,"events":{"active":0}}},"system":{"cpu":{"cores":2},"load":{"1":1.79,"15":0.26,"5":0.69,"norm":{"1":0.895,"15":0.13,"5":0.345}}}}}}
2020-02-16T21:52:31.811Z        INFO    [monitoring]    log/log.go:153  Uptime: 36.415199ms
2020-02-16T21:52:31.811Z        INFO    [monitoring]    log/log.go:130  Stopping metrics logging.
2020-02-16T21:52:31.811Z        INFO    instance/beat.go:393    metricbeat stopped.
2020-02-16T21:52:31.811Z        ERROR   instance/beat.go:906    Exiting: Error importing Kibana dashboards: fail to create the Elasticsearch loader: Error creating Elasticsearch client: Couldn't connect to any of the configured Elasticsearch hosts. Errors: [Error connection to Elasticsearch http://es:9200: Get http://es:9200: dial tcp 172.19.0.2:9200: connect: connection refused]
Exiting: Error importing Kibana dashboards: fail to create the Elasticsearch loader: Error creating Elasticsearch client: Couldn't connect to any of the configured Elasticsearch hosts. Errors: [Error connection to Elasticsearch http://es:9200: Get http://es:9200: dial tcp 172.19.0.2:9200: connect: connection refused]
2020-02-16T21:52:33.952Z        INFO    instance/beat.go:611    Home path: [/usr/share/metricbeat] Config path: [/usr/share/metricbeat] Data path: [/usr/share/metricbeat/data] Logs path: [/usr/share/metricbeat/logs]
2020-02-16T21:52:33.952Z        INFO    instance/beat.go:618    Beat UUID: e85367c4-d9c3-43bc-8286-7627d8161b73
2020-02-16T21:52:33.952Z        INFO    [seccomp]       seccomp/seccomp.go:116  Syscall filter successfully installed
2020-02-16T21:52:33.952Z        INFO    [beat]  instance/beat.go:931    Beat info       {"system_info": {"beat": {"path": {"config": "/usr/share/metricbeat", "data": "/usr/share/metricbeat/data", "home": "/usr/share/metricbeat", "logs": "/usr/share/metricbeat/logs"}, "type": "metricbeat", "uuid": "e85367c4-d9c3-43bc-8286-7627d8161b73"}}}
2020-02-16T21:52:33.952Z        INFO    [beat]  instance/beat.go:940    Build info      {"system_info": {"build": {"commit": "4fa63eb23a94bf23650023317bdff335c4705fc2", "libbeat": "6.8.6", "time": "2019-12-13T16:24:23.000Z", "version": "6.8.6"}}}
2020-02-16T21:52:33.952Z        INFO    [beat]  instance/beat.go:943    Go runtime info {"system_info": {"go": {"os":"linux","arch":"amd64","max_procs":2,"version":"go1.10.8"}}}
2020-02-16T21:52:33.953Z        INFO    [beat]  instance/beat.go:947    Host info       {"system_info": {"host": {"architecture":"x86_64","boot_time":"2020-02-16T21:40:50Z","containerized":true,"name":"4bbbe5bd8adc","ip":["127.0.0.1/8","172.19.0.4/16"],"kernel_version":"4.19.76-linuxkit","mac":["02:42:ac:13:00:04"],"os":{"family":"redhat","platform":"centos","name":"CentOS Linux","version":"7 (Core)","major":7,"minor":7,"patch":1908,"codename":"Core"},"timezone":"UTC","timezone_offset_sec":0}}}
2020-02-16T21:52:33.954Z        INFO    [beat]  instance/beat.go:976    Process info    {"system_info": {"process": {"capabilities": {"inheritable":["chown","dac_override","fowner","fsetid","kill","setgid","setuid","setpcap","net_bind_service","net_raw","sys_chroot","mknod","audit_write","setfcap"],"permitted":null,"effective":null,"bounding":["chown","dac_override","fowner","fsetid","kill","setgid","setuid","setpcap","net_bind_service","net_raw","sys_chroot","mknod","audit_write","setfcap"],"ambient":null}, "cwd": "/usr/share/metricbeat", "exe": "/usr/share/metricbeat/metricbeat", "name": "metricbeat", "pid": 1, "ppid": 0, "seccomp": {"mode":"filter","no_new_privs":true}, "start_time": "2020-02-16T21:52:33.540Z"}}}
2020-02-16T21:52:33.954Z        INFO    instance/beat.go:280    Setup Beat: metricbeat; Version: 6.8.6
2020-02-16T21:52:33.954Z        INFO    elasticsearch/client.go:164     Elasticsearch url: http://es:9200
2020-02-16T21:52:33.954Z        INFO    [publisher]     pipeline/module.go:110  Beat name: 4bbbe5bd8adc
2020-02-16T21:52:33.955Z        INFO    socket/socket.go:79     socket process info will only be available for metricbeat because the process is running as a non-root user
2020-02-16T21:52:33.955Z        INFO    [monitoring]    log/log.go:117  Starting metrics logging every 30s
2020-02-16T21:52:33.956Z        INFO    elasticsearch/client.go:164     Elasticsearch url: http://es:9200
2020-02-16T21:52:34.121Z        INFO    elasticsearch/client.go:739     Attempting to connect to Elasticsearch version 6.8.6
2020-02-16T21:52:34.223Z        INFO    kibana/client.go:118    Kibana url: http://kibana:5601
2020-02-16T21:52:38.794Z        INFO    [monitoring]    log/log.go:152  Total non-zero metrics  {"monitoring": {"metrics": {"beat":{"cpu":{"system":{"ticks":20,"time":{"ms":22}},"total":{"ticks":50,"time":{"ms":53},"value":50},"user":{"ticks":30,"time":{"ms":31}}},"handles":{"limit":{"hard":1048576,"soft":1048576},"open":7},"info":{"ephemeral_id":"e7a18274-724e-4296-91d0-afcb8bc21471","uptime":{"ms":4871}},"memstats":{"gc_next":5923040,"memory_alloc":3321208,"memory_total":9557208,"rss":32415744}},"libbeat":{"config":{"module":{"running":0}},"output":{"type":"elasticsearch"},"pipeline":{"clients":0,"events":{"active":0}}},"system":{"cpu":{"cores":2},"load":{"1":1.72,"15":0.27,"5":0.7,"norm":{"1":0.86,"15":0.135,"5":0.35}}}}}}
2020-02-16T21:52:38.795Z        INFO    [monitoring]    log/log.go:153  Uptime: 4.878777289s
2020-02-16T21:52:38.795Z        INFO    [monitoring]    log/log.go:130  Stopping metrics logging.
2020-02-16T21:52:38.795Z        INFO    instance/beat.go:393    metricbeat stopped.
2020-02-16T21:52:38.795Z        ERROR   instance/beat.go:906    Exiting: Error importing Kibana dashboards: fail to create the Kibana loader: Error creating Kibana client: Error creating Kibana client: fail to get the Kibana version: HTTP GET request to /api/status fails: parsing kibana response: invalid character 'K' looking for beginning of value. Response: Kibana server is not ready yet.
Exiting: Error importing Kibana dashboards: fail to create the Kibana loader: Error creating Kibana client: Error creating Kibana client: fail to get the Kibana version: HTTP GET request to /api/status fails: parsing kibana response: invalid character 'K' looking for beginning of value. Response: Kibana server is not ready yet.
2020-02-16T21:52:43.204Z        INFO    instance/beat.go:611    Home path: [/usr/share/metricbeat] Config path: [/usr/share/metricbeat] Data path: [/usr/share/metricbeat/data] Logs path: [/usr/share/metricbeat/logs]
2020-02-16T21:52:43.208Z        INFO    instance/beat.go:618    Beat UUID: e85367c4-d9c3-43bc-8286-7627d8161b73
2020-02-16T21:52:43.208Z        INFO    [seccomp]       seccomp/seccomp.go:116  Syscall filter successfully installed
2020-02-16T21:52:43.208Z        INFO    [beat]  instance/beat.go:931    Beat info       {"system_info": {"beat": {"path": {"config": "/usr/share/metricbeat", "data": "/usr/share/metricbeat/data", "home": "/usr/share/metricbeat", "logs": "/usr/share/metricbeat/logs"}, "type": "metricbeat", "uuid": "e85367c4-d9c3-43bc-8286-7627d8161b73"}}}
2020-02-16T21:52:43.208Z        INFO    [beat]  instance/beat.go:940    Build info      {"system_info": {"build": {"commit": "4fa63eb23a94bf23650023317bdff335c4705fc2", "libbeat": "6.8.6", "time": "2019-12-13T16:24:23.000Z", "version": "6.8.6"}}}
2020-02-16T21:52:43.208Z        INFO    [beat]  instance/beat.go:943    Go runtime info {"system_info": {"go": {"os":"linux","arch":"amd64","max_procs":2,"version":"go1.10.8"}}}
2020-02-16T21:52:43.210Z        INFO    [beat]  instance/beat.go:947    Host info       {"system_info": {"host": {"architecture":"x86_64","boot_time":"2020-02-16T21:40:50Z","containerized":true,"name":"4bbbe5bd8adc","ip":["127.0.0.1/8","172.19.0.4/16"],"kernel_version":"4.19.76-linuxkit","mac":["02:42:ac:13:00:04"],"os":{"family":"redhat","platform":"centos","name":"CentOS Linux","version":"7 (Core)","major":7,"minor":7,"patch":1908,"codename":"Core"},"timezone":"UTC","timezone_offset_sec":0}}}
2020-02-16T21:52:43.210Z        INFO    [beat]  instance/beat.go:976    Process info    {"system_info": {"process": {"capabilities": {"inheritable":["chown","dac_override","fowner","fsetid","kill","setgid","setuid","setpcap","net_bind_service","net_raw","sys_chroot","mknod","audit_write","setfcap"],"permitted":null,"effective":null,"bounding":["chown","dac_override","fowner","fsetid","kill","setgid","setuid","setpcap","net_bind_service","net_raw","sys_chroot","mknod","audit_write","setfcap"],"ambient":null}, "cwd": "/usr/share/metricbeat", "exe": "/usr/share/metricbeat/metricbeat", "name": "metricbeat", "pid": 1, "ppid": 0, "seccomp": {"mode":"filter","no_new_privs":true}, "start_time": "2020-02-16T21:52:42.540Z"}}}
2020-02-16T21:52:43.210Z        INFO    instance/beat.go:280    Setup Beat: metricbeat; Version: 6.8.6
2020-02-16T21:52:43.211Z        INFO    elasticsearch/client.go:164     Elasticsearch url: http://es:9200
2020-02-16T21:52:43.212Z        INFO    [publisher]     pipeline/module.go:110  Beat name: 4bbbe5bd8adc
2020-02-16T21:52:43.213Z        INFO    socket/socket.go:79     socket process info will only be available for metricbeat because the process is running as a non-root user
2020-02-16T21:52:43.213Z        INFO    [monitoring]    log/log.go:117  Starting metrics logging every 30s
2020-02-16T21:52:43.213Z        INFO    elasticsearch/client.go:164     Elasticsearch url: http://es:9200
2020-02-16T21:52:43.234Z        INFO    elasticsearch/client.go:739     Attempting to connect to Elasticsearch version 6.8.6
2020-02-16T21:52:43.249Z        INFO    kibana/client.go:118    Kibana url: http://kibana:5601
2020-02-16T21:52:43.257Z        INFO    [monitoring]    log/log.go:152  Total non-zero metrics  {"monitoring": {"metrics": {"beat":{"cpu":{"system":{"ticks":30,"time":{"ms":31}},"total":{"ticks":60,"time":{"ms":64},"value":60},"user":{"ticks":30,"time":{"ms":33}}},"handles":{"limit":{"hard":1048576,"soft":1048576},"open":7},"info":{"ephemeral_id":"41067d64-3fd3-4068-910f-ed90d72de723","uptime":{"ms":122}},"memstats":{"gc_next":5053664,"memory_alloc":3773056,"memory_total":9791296,"rss":36651008}},"libbeat":{"config":{"module":{"running":0}},"output":{"type":"elasticsearch"},"pipeline":{"clients":0,"events":{"active":0}}},"system":{"cpu":{"cores":2},"load":{"1":1.99,"15":0.29,"5":0.77,"norm":{"1":0.995,"15":0.145,"5":0.385}}}}}}
2020-02-16T21:52:43.257Z        INFO    [monitoring]    log/log.go:153  Uptime: 122.7154ms
2020-02-16T21:52:43.257Z        INFO    [monitoring]    log/log.go:130  Stopping metrics logging.
2020-02-16T21:52:43.257Z        INFO    instance/beat.go:393    metricbeat stopped.
2020-02-16T21:52:43.257Z        ERROR   instance/beat.go:906    Exiting: Error importing Kibana dashboards: fail to create the Kibana loader: Error creating Kibana client: Error creating Kibana client: fail to get the Kibana version: HTTP GET request to /api/status fails: parsing kibana response: invalid character 'K' looking for beginning of value. Response: Kibana server is not ready yet.
Exiting: Error importing Kibana dashboards: fail to create the Kibana loader: Error creating Kibana client: Error creating Kibana client: fail to get the Kibana version: HTTP GET request to /api/status fails: parsing kibana response: invalid character 'K' looking for beginning of value. Response: Kibana server is not ready yet.
2020-02-16T21:52:50.827Z        INFO    instance/beat.go:611    Home path: [/usr/share/metricbeat] Config path: [/usr/share/metricbeat] Data path: [/usr/share/metricbeat/data] Logs path: [/usr/share/metricbeat/logs]
2020-02-16T21:52:50.833Z        INFO    instance/beat.go:618    Beat UUID: e85367c4-d9c3-43bc-8286-7627d8161b73
2020-02-16T21:52:50.834Z        INFO    [seccomp]       seccomp/seccomp.go:116  Syscall filter successfully installed
2020-02-16T21:52:50.834Z        INFO    [beat]  instance/beat.go:931    Beat info       {"system_info": {"beat": {"path": {"config": "/usr/share/metricbeat", "data": "/usr/share/metricbeat/data", "home": "/usr/share/metricbeat", "logs": "/usr/share/metricbeat/logs"}, "type": "metricbeat", "uuid": "e85367c4-d9c3-43bc-8286-7627d8161b73"}}}
2020-02-16T21:52:50.834Z        INFO    [beat]  instance/beat.go:940    Build info      {"system_info": {"build": {"commit": "4fa63eb23a94bf23650023317bdff335c4705fc2", "libbeat": "6.8.6", "time": "2019-12-13T16:24:23.000Z", "version": "6.8.6"}}}
2020-02-16T21:52:50.834Z        INFO    [beat]  instance/beat.go:943    Go runtime info {"system_info": {"go": {"os":"linux","arch":"amd64","max_procs":2,"version":"go1.10.8"}}}
2020-02-16T21:52:50.837Z        INFO    [beat]  instance/beat.go:947    Host info       {"system_info": {"host": {"architecture":"x86_64","boot_time":"2020-02-16T21:40:50Z","containerized":true,"name":"4bbbe5bd8adc","ip":["127.0.0.1/8","172.19.0.4/16"],"kernel_version":"4.19.76-linuxkit","mac":["02:42:ac:13:00:04"],"os":{"family":"redhat","platform":"centos","name":"CentOS Linux","version":"7 (Core)","major":7,"minor":7,"patch":1908,"codename":"Core"},"timezone":"UTC","timezone_offset_sec":0}}}
2020-02-16T21:52:50.837Z        INFO    [beat]  instance/beat.go:976    Process info    {"system_info": {"process": {"capabilities": {"inheritable":["chown","dac_override","fowner","fsetid","kill","setgid","setuid","setpcap","net_bind_service","net_raw","sys_chroot","mknod","audit_write","setfcap"],"permitted":null,"effective":null,"bounding":["chown","dac_override","fowner","fsetid","kill","setgid","setuid","setpcap","net_bind_service","net_raw","sys_chroot","mknod","audit_write","setfcap"],"ambient":null}, "cwd": "/usr/share/metricbeat", "exe": "/usr/share/metricbeat/metricbeat", "name": "metricbeat", "pid": 1, "ppid": 0, "seccomp": {"mode":"filter","no_new_privs":true}, "start_time": "2020-02-16T21:52:49.980Z"}}}
2020-02-16T21:52:50.837Z        INFO    instance/beat.go:280    Setup Beat: metricbeat; Version: 6.8.6
2020-02-16T21:52:50.838Z        INFO    elasticsearch/client.go:164     Elasticsearch url: http://es:9200
2020-02-16T21:52:50.841Z        INFO    [publisher]     pipeline/module.go:110  Beat name: 4bbbe5bd8adc
2020-02-16T21:52:50.842Z        INFO    socket/socket.go:79     socket process info will only be available for metricbeat because the process is running as a non-root user
2020-02-16T21:52:50.843Z        INFO    [monitoring]    log/log.go:117  Starting metrics logging every 30s
2020-02-16T21:52:50.843Z        INFO    elasticsearch/client.go:164     Elasticsearch url: http://es:9200
2020-02-16T21:52:50.850Z        INFO    elasticsearch/client.go:739     Attempting to connect to Elasticsearch version 6.8.6
2020-02-16T21:52:50.862Z        INFO    kibana/client.go:118    Kibana url: http://kibana:5601
2020-02-16T21:53:17.656Z        INFO    instance/beat.go:736    Kibana dashboards successfully loaded.
2020-02-16T21:53:17.656Z        INFO    instance/beat.go:402    metricbeat start running.
```


**The hard way - Installing ELK-B:**

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
https://jenkins.io/doc/book/

### Jenkins Master - Agent Setup:
Link: https://plugins.jenkins.io/swarm </br>
Swarm plugin is the easiest way to utilize master-agent configurations. 


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
