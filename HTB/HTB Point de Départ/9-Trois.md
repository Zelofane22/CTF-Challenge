#HackTheBox 
# Scan Nmap
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.7 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 178bd425452a20b879f8e258d78e79f4 (RSA)
|   256 e60f1af6328a40ef2da73b22d1c714fa (ECDSA)
|_  256 2de1874175f391544116b72b80c68f05 (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-title: The Toppers
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: Apache/2.4.29 (Ubuntu)

# Find
in web page 
	Phone: +01 343 123 6102  
	Email: mail@thetoppers.htb

# subdomaines scan
