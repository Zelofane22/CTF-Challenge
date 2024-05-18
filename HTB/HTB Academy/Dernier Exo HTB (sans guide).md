#HackTheBox 
# Cible 10.129.60.117
# 1.Scan nmap
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.1 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 4c73a025f5fe817b822b3649a54dc85e (RSA)
|   256 e1c056d052042f3cac9ae7b1792bbb13 (ECDSA)
|_  256 523147140dc38e1573e3c424a23a1277 (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
| http-robots.txt: 1 disallowed entry 
|_/admin/
|_http-title: Welcome to GetSimple! - gettingstarted
|_http-server-header: Apache/2.4.41 (Ubuntu)


# 2. Enumeration
## first analyse Gobuster
/.hta                 (Status: 403) [Size: 279]
/.htpasswd            (Status: 403) [Size: 279]
/.htaccess            (Status: 403) [Size: 279]
/admin                (Status: 301) [Size: 316] [--> http://10.129.107.206/admin/]
/backups              (Status: 301) [Size: 318] [--> http://10.129.107.206/backups/]
/data                 (Status: 301) [Size: 315] [--> http://10.129.107.206/data/]
/index.php            (Status: 200) [Size: 5485]
/plugins              (Status: 301) [Size: 318] [--> http://10.129.107.206/plugins/]
/robots.txt           (Status: 200) [Size: 32]
/server-status        (Status: 403) [Size: 279]
/sitemap.xml          (Status: 200) [Size: 431]
/theme                (Status: 301) [Size: 316] [--> http://10.129.107.206/theme/]

## second enum Gobuster
Commande : gobuster dir -u http://10.129.107.206/admin/ -w /usr/share/dirb/wordlists/common.txt
Answer :
	/.hta                 (Status: 403) [Size: 279]
/.htaccess            (Status: 403) [Size: 279]
/.htpasswd            (Status: 403) [Size: 279]
/inc                  (Status: 301) [Size: 320] [--> http://10.129.107.206/admin/inc/]
/index.php            (Status: 200) [Size: 2623]
/lang                 (Status: 301) [Size: 321] [--> http://10.129.107.206/admin/lang/]
/template             (Status: 301) [Size: 325] [--> http://10.129.107.206/admin/template/]

## Searchsploit getsimple
Exploite posible

## Piste 
getsimple est un outil du serveur potentiellement exploitable
http://10.129.42.249/admin/index.php (connexion page of admin)
getsimple est un outil du serveur potentiellement exploitable

Contenue de : http://10.129.107.206/data/cache/2a4c6447379fba09620ba05582eb61af.txt      
<<{"status":"0","latest":"3.3.16","your_version":"3.3.15","message":"You have an old version - please upgrade"} >>
getsimple version = 3.3.15   ?

## Search exploit getsimple 3.3.15 (exploit table)
                   

# Exploit
Connexion succes on admin page conxion : admin:admin

Connexion au meterpreter grace à metasploit en exploitant la vulnerabilte de getsimple cm (user.txt trouvé).

Modification du contenue du fichier template.php dans l'onglet de configuration des thèmes : http://10.129.42.249/admin/theme-edit.php?t=Cardinal&f=template.php puis je clic sur http://10.129.42.249/theme/Cardinal/template.php . 

REVERSHELL OBTENU SUR MON BASH.

# Find
## Version Linux : 5.4.0-65-generic

## Track on linpeas
	╔══════════╣ Unexpected in root
	/swap.img