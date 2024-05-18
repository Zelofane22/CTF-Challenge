#HackTheBox 
Cible : 10.129.13.183
## Scan - Recon
	PORT   STATE SERVICE VERSION
	22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.2 (Ubuntu Linux; protocol 2.0)
	| ssh-hostkey: 
	|   2048 c4f8ade8f80477decf150d630a187e49 (RSA)
	|   256 228fb197bf0f1708fc7e2c8fe9773a48 (ECDSA)
	|_  256 e6ac27a3b5a9f1123c34a55d5beb3de9 (ED25519)
	80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
	|_http-server-header: Apache/2.4.18 (Ubuntu)
	| http-methods: 
	|_  Supported Methods: GET HEAD POST OPTIONS
	|_http-title: Site doesn't have a title (text/html).


## Enumeration
Contenue de la page web http :
	<b>Hello world!</b>
	<!-- directory. Nothing interesting here! -->
### Piste sérieuses
	 /nibbleblog/


Gobuster enume 10.129.227.124
	/.htpasswd            (Status: 403) [Size: 298]
	/.hta                 (Status: 403) [Size: 293]
	/.htaccess            (Status: 403) [Size: 298]
	/index.html           (Status: 200) [Size: 93]
	/server-status        (Status: 403) [Size: 302]
	
Gobuster enum 10.129.227.124/nibbleblog/
	/.htaccess            (Status: 403) [Size: 309]
	/.hta                 (Status: 403) [Size: 304]
	/.htpasswd            (Status: 403) [Size: 309]
	/admin                (Status: 301) [Size: 327] [--> http://10.129.227.124/nibbleblog/admin/]
	/admin.php            (Status: 200) [Size: 1401]
	/content              (Status: 301) [Size: 329] [--> http://10.129.227.124/nibbleblog/content/]
		/.htaccess            (Status: 403) [Size: 317]
		/.hta                 (Status: 403) [Size: 312]
		/.htpasswd            (Status: 403) [Size: 317]
		/private              (Status: 301) [Size: 337] [--> http://10.129.227.124/nibbleblog/content/private/]
		/public               (Status: 301) [Size: 336] [--> http://10.129.227.124/nibbleblog/content/public/]
		/tmp                  (Status: 301) [Size: 333] [--> http://10.129.227.124/nibbleblog/content/tmp/]
	/index.php            (Status: 200) [Size: 2987]
	/languages            (Status: 301) [Size: 331] [--> http://10.129.227.124/nibbleblog/languages/]
	/plugins              (Status: 301) [Size: 329] [--> http://10.129.227.124/nibbleblog/plugins/]
	/README               (Status: 200) [Size: 4628]
	/themes               (Status: 301) [Size: 328] [--> http://10.129.227.124/nibbleblog/themes/]
	
curl 10.129.227.124/nibbleblog/README
	====== Nibbleblog ======
	Version: v4.0.3
	Codename: Coffee
	Release date: 2014-04-01

## Pistes :
Page de connexion de l'admin :
	http://10.129.227.124/nibbleblog/admin.php
	
====== Nibbleblog ======
	Version: v4.0.3
	Codename: Coffee
Nibbles version exploitable (metasploit)

10.129.227.124/nibbleblog/README
	USERNAME = admin

	


## Exploits
Connexion réussi sur la page admin.php
	Identifiants "admin:nibbles"

injection de fichier php réussi dans plugin image pour le webshell
Execution de commande webshell réussi
Mise à nivau du control shell avec python3

