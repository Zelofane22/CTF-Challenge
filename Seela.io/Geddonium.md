# Recon
## nmap
└─# nmap -Pn -sV -p 80 -sC 10.42.6.248 
```
Starting Nmap 7.94 ( https://nmap.org ) at 2023-07-26 17:43 EDT
Nmap scan report for 10.42.6.248 (10.42.6.248)
Host is up (0.069s latency).

PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.56 ((Debian))
|_http-generator: Drupal 8 (https://www.drupal.org)
|_http-title: Home | Geddonium
| http-robots.txt: 22 disallowed entries (15 shown)
| /core/ /profiles/ /README.txt /web.config /admin/ 
| /comment/reply/ /filter/tips/ /node/add/ /search/ /user/register/ 
| /user/password/ /user/login/ /user/logout/ /index.php/admin/ 
|_/index.php/comment/reply/
|_http-server-header: Apache/2.4.56 (Debian)
```


## gobuster
/234                  (Status: 302) [Size: 386] [--> /core/install.php]

## cms : drupal 8.5.0

# Exploit : metasploit 
search drupal 8.5.0

# foothold
## _enum_
www-data@geddonium-6c687db7fc-5p8sj:/var/www/html/Secr3T_D1r3ctoRy$ cat pass.txt
```
F7qXKwEWi3ZaOYZkjOf6BQ
```

[www-data@geddonium-6c687db7fc-5p8sj:/var/www/html$ ]sudo /usr/lib/dbus-1.0/dbus-daemon-launch-helper
```
We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.
```

cat /etc/passwd | grep sh$
```
root:x:0:0:root:/root:/bin/bash
stephane:x:1000:1000::/home/stephane:/bin/bash
```

## Privesc

$ su stephane
Password: 
```
nxv+P5fCQa2FtlGQB5sGZg
```

[stephane@geddonium-6c687db7fc-5p8sj:/tmp$] sudo -l #ExploitLDPRELOAD
```
Matching Defaults entries for stephane on geddonium-6c687db7fc-qk5cg:
    env_reset, mail_badpass,             
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin,                                                       env_keep+=LD_PRELOAD
    
User stephane may run the following commands on geddonium-6c687db7fc-kbn8d:
    (ALL) NOPASSWD: /usr/sbin/apache2 restart
```

## _exploit LD_PRELOAD_
```

```

