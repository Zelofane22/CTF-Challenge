#TryHackMe #linux 
# Enum
## nmap
┌──(root㉿kali)-[~]
└─# nmap -Pn -sS -sC -sV 10.10.237.247
Starting Nmap 7.93 ( https://nmap.org ) at 2023-06-21 20:16 GMT
Nmap scan report for 10.10.237.247 (10.10.237.247)
Host is up (0.36s latency).
Not shown: 916 closed tcp ports (reset)
PORT      STATE SERVICE    VERSION
22/tcp    open  ssh        OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 3f15197035fddd0d07a050a37dfa10a0 (RSA)
|   256 a8675c52770241d790e7ed32d201d965 (ECDSA)
|_  256 2692592d5e25908909f5e5e03381776a (ED25519)
9000/tcp  open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
9001/tcp  open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
9002/tcp  open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
9003/tcp  open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
9009/tcp  open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
9010/tcp  open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
9011/tcp  open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
9040/tcp  open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
9050/tcp  open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
9071/tcp  open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
9080/tcp  open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
9081/tcp  open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
9090/tcp  open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
9091/tcp  open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
9099/tcp  open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
9100/tcp  open  jetdirect?
9101/tcp  open  jetdirect?
9102/tcp  open  jetdirect?
9103/tcp  open  jetdirect?
9110/tcp  open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
9111/tcp  open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
9200/tcp  open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
9207/tcp  open  ssh        Dropbear sshd (protocol 2.0)
9220/tcp  open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
9290/tcp  open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
9415/tcp  open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
9418/tcp  open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
9485/tcp  open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
9500/tcp  open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
9502/tcp  open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
9503/tcp  open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
9535/tcp  open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
9575/tcp  open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
9593/tcp  open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
9594/tcp  open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
9595/tcp  open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
9618/tcp  open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
9666/tcp  open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
9876/tcp  open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
9877/tcp  open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
9878/tcp  open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
9898/tcp  open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
9900/tcp  open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
9917/tcp  open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
9929/tcp  open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
9943/tcp  open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
9944/tcp  open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
9968/tcp  open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
9998/tcp  open  ssh        Dropbear sshd (protocol 2.0)
|_uptime-agent-info: SSH-2.0-dropbear\x0D
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
9999/tcp  open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
10000/tcp open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
10001/tcp open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
10002/tcp open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
10003/tcp open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
10004/tcp open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
10009/tcp open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
10010/tcp open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
10012/tcp open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
10024/tcp open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
10025/tcp open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
10082/tcp open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
10180/tcp open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
10215/tcp open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
10243/tcp open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
10566/tcp open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
10616/tcp open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
10617/tcp open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
10621/tcp open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
10626/tcp open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
10628/tcp open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
10629/tcp open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
10778/tcp open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
11110/tcp open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
11111/tcp open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
11967/tcp open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
12000/tcp open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
12174/tcp open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
12265/tcp open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
12345/tcp open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
13456/tcp open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
13722/tcp open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
13782/tcp open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
13783/tcp open  ssh        Dropbear sshd (protocol 2.0)
| ssh-hostkey: 
|_  2048 fff4db79a9bcb88ad43f56c2cfcb7d11 (RSA)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

┌──(root㉿kali)-[~]
└─# nmap -Pn -script=vuln 10.10.79.243                    
Starting Nmap 7.93 ( https://nmap.org ) at 2023-06-21 20:30 GMT
Nmap scan report for 10.10.79.243 (10.10.79.243)
Host is up (0.34s latency).
Not shown: 916 closed tcp ports (reset)
PORT      STATE SERVICE
22/tcp    open  ssh
9000/tcp  open  cslistener
9001/tcp  open  tor-orport
ssl-ccs-injection: No reply from server (TIMEOUT)
9002/tcp  open  dynamid
9003/tcp  open  unknown
9009/tcp  open  pichat
9010/tcp  open  sdr
9011/tcp  open  d-star
9040/tcp  open  tor-trans
9050/tcp  open  tor-socks
9071/tcp  open  unknown
9080/tcp  open  glrpc
9081/tcp  open  cisco-aqos
9090/tcp  open  zeus-admin
9091/tcp  open  xmltec-xmlmail
9099/tcp  open  unknown
9100/tcp  open  jetdirect
9101/tcp  open  jetdirect
9102/tcp  open  jetdirect
9103/tcp  open  jetdirect
9110/tcp  open  unknown
9111/tcp  open  DragonIDSConsole
9200/tcp  open  wap-wsp
9207/tcp  open  wap-vcal-s
9220/tcp  open  unknown
9290/tcp  open  unknown
9415/tcp  open  unknown
9418/tcp  open  *git*
9485/tcp  open  unknown
9500/tcp  open  ismserver
9502/tcp  open  unknown
9503/tcp  open  unknown
9535/tcp  open  man
9575/tcp  open  unknown
9593/tcp  open  cba8
9594/tcp  open  msgsys
9595/tcp  open  pds
9618/tcp  open  condor
9666/tcp  open  zoomcp
9876/tcp  open  sd
9877/tcp  open  x510
9878/tcp  open  kca-service
9898/tcp  open  monkeycom
9900/tcp  open  iua
9917/tcp  open  unknown
9929/tcp  open  nping-echo
9943/tcp  open  unknown
9944/tcp  open  unknown
9968/tcp  open  unknown
9998/tcp  open  distinct32
9999/tcp  open  abyss
10000/tcp open  snet-sensor-mgmt
|http-vuln-cve2006-3392: ERROR: Script execution failed (use -d to debug)
10001/tcp open  scp-config
10002/tcp open  documentum
10003/tcp open  documentum_s
10004/tcp open  emcrmirccd
10009/tcp open  swdtp-sv
10010/tcp open  rxapi
10012/tcp open  unknown
10024/tcp open  unknown
10025/tcp open  unknown
10082/tcp open  amandaidx
10180/tcp open  unknown
10215/tcp open  unknown
10243/tcp open  unknown
10566/tcp open  unknown
10616/tcp open  unknown
10617/tcp open  unknown
10621/tcp open  unknown
10626/tcp open  unknown
10628/tcp open  unknown
10629/tcp open  unknown
10778/tcp open  unknown
11110/tcp open  sgi-soap
11111/tcp open  vce
11967/tcp open  sysinfo-sp
12000/tcp open  cce4x
12174/tcp open  unknown
12265/tcp open  unknown
12345/tcp open  netbus
13456/tcp open  unknown
*13722/tcp open  netbackup
13782/tcp open  netbackup
13783/tcp open  netbackup*


