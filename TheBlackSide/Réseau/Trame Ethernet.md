# Trame decode
GET /36YN18e$ HTTP/1.1
Authorization: Basic cGItPnp4TlJtYnU0==
User-Agent: 8008135
Host: www.myipv6.org
Accept: */*

# Find in www.myipv6.org
@Ip : 154.124.76.203

# Base64 decode
cGItPnp4TlJtYnU0==  ---->  pb->zxNRmbu4

# Scan Nmap 
## de myipv6.org
PORT    STATE SERVICE  VERSION
22/tcp  open  ssh      OpenSSH 7.9 (protocol 2.0)
| ssh-hostkey: 
|   2048 0edad8a20c1e8de877acc19d714a2de6 (RSA)
|   256 2950d2888587de2877a2f125a74f5651 (ECDSA)
|_  256 b04fb69aebbaf5a712bb51e32fce36a9 (ED25519)
25/tcp  open  smtp     Postfix smtpd
|_smtp-commands: koreus.com, PIPELINING, SIZE 10240000, VRFY, ETRN, ENHANCEDSTATUSCODES, 8BITMIME, DSN, SMTPUTF8
80/tcp  open  http     nginx 1.14.2
|_http-server-header: nginx/1.14.2
|_http-title: Did not follow redirect to https://myipv6.org/
443/tcp open  ssl/http nginx 1.14.2
| ssl-cert: Subject: commonName=myipv6.org
| Subject Alternative Name: DNS:myipv6.org, DNS:www.myipv6.org
| Not valid before: 2023-02-14T01:17:10
|_Not valid after:  2023-05-15T01:17:09
|_http-title: Test IPv6
|_http-server-header: nginx/1.14.2

## de 154.124.76.203


# Gobuster de 154.124.76.203
/.well-known/assetlinks.json (Status: 200) [Size: 203]
/.well-known/host-meta.json (Status: 200) [Size: 203]
/.well-known/jwks.json (Status: 200) [Size: 203]
/_framework/blazor.boot.json (Status: 200) [Size: 203]
/contribute.json      (Status: 200) [Size: 203]
/crossdomain.xml      (Status: 200) [Size: 203]
/jwks.json            (Status: 200) [Size: 203]
/sitemap.xml          (Status: 200) [Size: 203]
/web.xml              (Status: 200) [Size: 203]
/webpack.manifest.json (Status: 200) [Size: 203]
