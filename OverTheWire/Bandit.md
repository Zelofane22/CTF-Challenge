# Cible :  *bandit.labs.overthewire.org Port = 2220* 

# Connexion ssh bandit0
creds = bandit0:bandit0

# bandit1:NH2SXQwcBdpmTEzi3bvBHMM9H66vVXjL

# bandit4:2EW7BBsr6aMMoJ2HjW067dm8EgX26xNe

# bandit5:lrIWWI6bB37kxfiCQZqUdOIYfr6eEeqR

# bandit6:P4L4vucdmLnm8I7Vl7jG1ApGSfjYKqJU

# bandit7:z7WtoNQU2XfjmMtWA8u5rN4vzqu4v99S

# [bandit8:TESKZC0XvTetK0S9xNwm25STk5iWrBvP]
## programme python utilisé
					with open("rap.txt", "r") as text:  
					    content = text.read()  
					    pas = content.split("\n")  
					    for i in pas:  
					        if pas.count(i) == 1:  
					            print(i)



# [bandit9:EN632PlfYiZbn3PhVK3XOGSlNInNE00t]
## string data.txt

# [bandit10:G7w8LIi6J3kTb8A7j9LgrywtEUlyyp6s]
## base64 -d data.txt

# [bandit11:6zPeziLdR2RKNdNYFNb6nVCKzphlXHBM]
## programme python utilisé
import string
with open("/home/bandit11/data.txt", "r") as data:
        content = data.read()
        result = ""
        for i in content:
                if i == " ":
                        result += i
                if i.islower():
                        d = string.ascii_lowercase.index(i) +13
                        if d >= len(string.ascii_lowercase):
                                d -= len(string.ascii_lowercase)
                        i = string.ascii_lowercase[d]
                        result += i
                if i.isupper():
                        d = string.ascii_uppercase.index(i)+13
                        if d >= len(string.ascii_lowercase):
                                d -= len(string.ascii_lowercase)
                        i = string.ascii_uppercase[d]
                        result += i
                if i.isdigit():
	                result += i
		 print(result)
## commande utilisable
cat data.txt | tr “a-zA-Z” “n-za-mN-ZA-M”

# [bandit12:JVNBBFSmZwKKOP0XbFXOoW8chDz5yVRv]
## copie de data.txt dans /tmp/myname123
check file type and rename and unzip with good format

# [bandit13:wbWdlBxEir4CaE8LaPhauuOo6pwRmrDw]
## use private key to connect on bandit14

# [bandit14:fGrHPx402xGC7U7rXKDaxiWFTOiF0ENq]
## # nc  localhost 30000
# [bandit15:jN2kgmIXJ6fShzhT2avhotn4Zcka6tnt]
### bandit15@bandit:~$ openssl s_client -connect localhost:30001

# [bandit16:JQttfApK4SeyHwDlI9SXGR50qclOAil1]
Afficher uniquement les ports qui écoutent sur ssl
### bandit16@bandit:/$ nmap -p 31000-32000 -sV --open -v localhost | grep -E '^[0-9]+/tcp.*ssl/.*$'
31518/tcp open  ssl/echo
[O31790/tcp open  ssl/unknown (port fonctionnel)
ssh private key of bandit17 got]

# [bandit17:id_rsa]
bandit17@bandit:~$ diff passwords.old passwords.new  (différence entre les deux mots de passes)
[f9wS9ZUDvZoo3PooHgYuuWdawDFvGld2
hga5tuuCLF6fFzUpnagiMN8ssu9LFrdg]

# [bandit18:hga5tuuCLF6fFzUpnagiMN8ssu9LFrdg]
ssh -p 2220 bandit18@bandit.labs.overthewire.org -t "cat /home/bandit18/readme"
	
		                         _                     _ _ _   
	                        | |__   __ _ _ __   __| (_) |_ 
	                        | '_ \ / _` | '_ \ / _` | | __|
	                        | |_) | (_| | | | | (_| | | |_ 
	                        |_.__/ \__,_|_| |_|\__,_|_|\__|                
	
	                      This is an OverTheWire game server. 
	            More information on http://www.overthewire.org/wargames
	
	bandit18@bandit.labs.overthewire.org's password: 
	awhqfNnAbc1naukrpqDYcF95h7HoMTrC
	Connection to bandit.labs.overthewire.org closed.


# [bandit19:awhqfNnAbc1naukrpqDYcF95h7HoMTrC]
bandit19@bandit:~$ ./bandit20-do cat /etc/bandit_pass/bandit20
	VxCazJaVykI6W36BkBU0mJTCM8rR95XT
# [bandit20:VxCazJaVykI6W36BkBU0mJTCM8rR95XT]
bandit20@bandit:~$ nmap -sV --open -v localhost
	PORT      STATE SERVICE          VERSION
	22/tcp    open  ssh              OpenSSH 8.9p1 (protocol 2.0)
	1111/tcp  open  time             (32 bits)
	1840/tcp  open  ssl/netopia-vo2?
	4321/tcp  open  rwhois?
	8000/tcp  open  http-alt?
	30000/tcp open  ndmps?

# [bandit20:VxCazJaVykI6W36BkBU0mJTCM8rR95XT]
[bandit20@bandit:~$] echo 'VxCazJaVykI6W36BkBU0mJTCM8rR95XT' | nc -l localhost 1478 &
[bandit20@bandit:~$] ./suconnect 1478
	Read: VxCazJaVykI6W36BkBU0mJTCM8rR95XT
	Password matches, sending next password
	NvEJF7oVjkddltPSrdKEFOllh9V1IBcq

# [bandit21:NvEJF7oVjkddltPSrdKEFOllh9V1IBcq]

# [bandit22:WdDozAdTM2z9DiFEQ2mGlwngMfj4EZff]
[bandit22@bandit:~$] cat /etc/cron.d/cronjob_bandit23
@reboot bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
* * * * * bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
bandit22@bandit:~$ cat /usr/bin/cronjob_bandit23.sh 
#!/bin/bash

myname=$(whoami)
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

cat /etc/bandit_pass/$myname > /tmp/$mytarget

[bandit22@bandit:/tmp$] echo I am user bandit23 | md5sum | cut -d ' ' -f 1
8ca319486bfbbc3663ea0fbe81326349

[bandit22@bandit:/tmp$] cat 8ca319486bfbbc3663ea0fbe81326349
QYw0Y2aiA672PsMmh9puTQuhoz8SyR2G

# [bandit23:QYw0Y2aiA672PsMmh9puTQuhoz8SyR2G]
[bandit23@bandit:~$] cat /etc/cron.d/cronjob_bandit24
@reboot bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
* * * * * bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
bandit23@bandit:~$ cat /usr/bin/cronjob_bandit24.sh
#!/bin/bash

myname=$(whoami)

cd /var/spool/$myname/foo
echo "Executing and deleting all scripts in /var/spool/$myname/foo:"
for i in * .*;
do
    if [ "$i" != "." -a "$i" != ".." ];
    then
        echo "Handling $i"
        owner="$(stat --format "%U" ./$i)"
        if [ "${owner}" = "bandit23" ]; then
            timeout -s 9 60 ./$i
        fi
        rm -f ./$i
    fi
done

[bandit23@bandit:~$]


# [bandit24:VAfGXJ1PBSsPSnvsjI8p759leLZ9GGar]
[bandit24@bandit:/tmp/.z$] nano ./b25pass.sh
	#!/bin/bash
	
	for i in {0000..9999}
	do
	        echo VAfGXJ1PBSsPSnvsjI8p759leLZ9GGar $i >> possibilities.txt
	done
	
	cat possibilities.txt | nc localhost 30002 > result.txt
	cat result.txt | grep -v "Wrong!"

[bandit24@bandit:/tmp/.z$] ./b25pass.sh
	I am the pincode checker for user bandit25. Please enter the password for user bandit24 and the secret pincode on a single line, separated by a space.
	Correct!
	The password of user bandit25 is p7TaowMYrmu23Ol8hiZh9UvD0O9hpx8d

# [bandit25:p7TaowMYrmu23Ol8hiZh9UvD0O9hpx8d]

# [bandit26:c7GvcKlw9mC7aUQaPx7nwFstuAIBw1o1]

# [bandit27:YnQpBuifNMas1hcUFk70ZmqkhUU2EuaS]