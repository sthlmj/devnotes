## Useful linux stuff
```
Linux find a directory recursively by name: 
find / -type d -name "svn" -print 

Linux find a file by filename:
find / -name *docker-compose* -print

Linux find files containing specific text: 
find / -type f -exec grep -H 'text-to-find-here' {} \;

Linux change keyboard layout to SE:
loadkeys se

Linux find docker command used to start the container?:
ps -ef | grep -i gerrit

Linux cat output of find results: 
find / -name *codeversioncontrol.config* -exec cat {} +

How to write to a file when there is now vim vi nano: 
cat > resolv.conf
search corp.biz
nameserver 123.12.12.12

Linux, change keyboard to Swedish:
loadkeys se

Identify OS: 
cat /etc/os-release
awk -F= '/^NAME/{print $2}' /etc/os-release

Scroll screen: 
history | less


Linux groups and users commands: 
List all users in the system: 
$ awk -F: '{ print $1}' /etc/passwd

List all groups in the system: 
$ getent group | awk -F: '{ print $1}'

List all users in a group: 
$ grep 'docker' /etc/group


Run a shell script: 
chmod +x myshellfile.sh
./myshellfile.sh

Linux run shell script in background: 
nohup myshellfile.sh &


List files in a folder by newest on top: 

ls -t
ls -lt | less
ls -lt ~/Downloads/ | less

CURLA -O för att hämta ner filen:
curl -v -O https://artifactory.com/sdfsfal.javadoc

curl -x "http://proxy.biz:80" --trace-ascii /tmp/dump.txt get.docker.com -o get-docker.sh 


Linux disk usage command: 
$ du -sh /opt/app


Trace network routing in cmder: 
$ tracert daaabbb.bbaaa.biz

Path: 
export PATH=/root/npm/stuff/bin/node:$PATH
```
## Links

20 useful linux yum centos commands: </br>
https://www.tecmint.com/20-linux-yum-yellowdog-updater-modified-commands-for-package-mangement/

25 Useful linux apt ubuntu debian commands: </br>
https://www.tecmint.com/useful-basic-commands-of-apt-get-and-apt-cache-for-package-management/

Wget proxy: </br>
https://stackoverflow.com/questions/11211705/how-to-set-proxy-for-wget

SSH agent: </br>
https://kb.iu.edu/d/aeww

Checking for existing ssh key: </br>
https://help.github.com/en/github/authenticating-to-github/checking-for-existing-ssh-keys

SELinux set levels: </br>
https://www.thegeekdiary.com/how-to-disable-or-set-selinux-to-permissive-mode/

Grep display all output but highlight search matches: </br>
https://superuser.com/questions/914856/grep-display-all-output-but-highlight-search-matches

Install Curl - ubuntu: </br>
https://www.cyberciti.biz/faq/how-to-install-curl-command-on-a-ubuntu-linux/

Podman on windows 10: </br>
https://computingforgeeks.com/run-podman-on-windows-server-with-wsl2/
