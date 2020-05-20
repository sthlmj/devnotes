

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
