#HackTheBox #linux #medium 
# recon
##  *nmap -sV 10.129.229.62                  
	Starting Nmap 7.93 ( https://nmap.org ) at 2023-08-03 21:42 GMT
	Nmap scan report for 10.129.229.62 (10.129.229.62)
	Host is up (0.19s latency).
	Not shown: 996 closed tcp ports (reset)
	PORT     STATE SERVICE VERSION
	22/tcp   open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
	80/tcp   open  http    Apache httpd 2.4.41 ((Ubuntu))
	3000/tcp open  ppp?
	3306/tcp open  mysql   MySQL 8.0.30-0ubuntu0.20.04.2

## *nmap -sVC -p 22,80,3000,3306 -T4 10.129.229.62*
	Starting Nmap 7.93 ( https://nmap.org ) at 2023-08-03 22:10 GMT
	Nmap scan report for 10.129.229.62 (10.129.229.62)
	Host is up (0.13s latency).
	
	PORT     STATE SERVICE VERSION
	22/tcp   open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
	| ssh-hostkey: 
	|   3072 29dd8ed7171e8e3090873cc651007c75 (RSA)
	|   256 80a4c52e9ab1ecda276439a408973bef (ECDSA)
	|_  256 f590ba7ded55cb7007f2bbc891931bf6 (ED25519)
	80/tcp   open  http    Apache httpd 2.4.41 ((Ubuntu))
	|_http-generator: Hugo 0.94.2
	|_http-server-header: Apache/2.4.41 (Ubuntu)
	|_http-title: Ambassador Development Server
	3000/tcp open  ppp?
	| fingerprint-strings: 
	|   FourOhFourRequest: 
	|     HTTP/1.0 302 Found
	|     Cache-Control: no-cache
	|     Content-Type: text/html; charset=utf-8
	|     Expires: -1
	|     Location: /login
	|     Pragma: no-cache
	|     Set-Cookie: redirect_to=%2Fnice%2520ports%252C%2FTri%256Eity.txt%252ebak; Path=/; HttpOnly; SameSite=Lax
	|     X-Content-Type-Options: nosniff
	|     X-Frame-Options: deny
	|     X-Xss-Protection: 1; mode=block
	|     Date: Thu, 03 Aug 2023 22:11:45 GMT
	|     Content-Length: 29
	|     href="/login">Found</a>.
	|   GenericLines, Help, Kerberos, RTSPRequest, SSLSessionReq, TLSSessionReq, TerminalServerCookie: 
	|     HTTP/1.1 400 Bad Request
	|     Content-Type: text/plain; charset=utf-8
	|     Connection: close
	|     Request
	|   GetRequest: 
	|     HTTP/1.0 302 Found
	|     Cache-Control: no-cache
	|     Content-Type: text/html; charset=utf-8
	|     Expires: -1
	|     Location: /login
	|     Pragma: no-cache
	|     Set-Cookie: redirect_to=%2F; Path=/; HttpOnly; SameSite=Lax
	|     X-Content-Type-Options: nosniff
	|     X-Frame-Options: deny
	|     X-Xss-Protection: 1; mode=block
	|     Date: Thu, 03 Aug 2023 22:11:07 GMT
	|     Content-Length: 29
	|     href="/login">Found</a>.
	|   HTTPOptions: 
	|     HTTP/1.0 302 Found
	|     Cache-Control: no-cache
	|     Expires: -1
	|     Location: /login
	|     Pragma: no-cache
	|     Set-Cookie: redirect_to=%2F; Path=/; HttpOnly; SameSite=Lax
	|     X-Content-Type-Options: nosniff
	|     X-Frame-Options: deny
	|     X-Xss-Protection: 1; mode=block
	|     Date: Thu, 03 Aug 2023 22:11:14 GMT
	|_    Content-Length: 0
	3306/tcp open  mysql   MySQL 8.0.30-0ubuntu0.20.04.2
	| mysql-info: 
	|   Protocol: 10
	|   Version: 8.0.30-0ubuntu0.20.04.2
	|   Thread ID: 11
	|   Capabilities flags: 65535
	|   Some Capabilities: FoundRows, IgnoreSigpipes, IgnoreSpaceBeforeParenthesis, Speaks41ProtocolOld, LongColumnFlag, Support41Auth, DontAllowDatabaseTableColumn, LongPassword, SupportsTransactions, SupportsCompression, SwitchToSSLAfterHandshake, InteractiveClient, Speaks41ProtocolNew, ConnectWithDatabase, SupportsLoadDataLocal, ODBCClient, SupportsMultipleStatments, SupportsAuthPlugins, SupportsMultipleResults
	|   Status: Autocommit
	|   Salt: X}(\x07WF=Z	!qhwp[yc*v\x12
	|_  Auth Plugin Name: caching_sha2_password
	| ssl-cert: Subject: commonName=MySQL_Server_8.0.28_Auto_Generated_Server_Certificate