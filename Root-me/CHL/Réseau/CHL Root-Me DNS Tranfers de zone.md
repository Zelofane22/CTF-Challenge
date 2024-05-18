# scan
	PORT      STATE SERVICE VERSION
	54011/tcp open  domain  ISC BIND 9.16.1 (Ubuntu Linux)
IP : 212.129.38.224


# Enum
## Gobuster
	/.hta                 (Status: 403) [Size: 146]
	/.bash_history        (Status: 403) [Size: 146]
	/.cvsignore           (Status: 403) [Size: 146]
	/.forward             (Status: 403) [Size: 146]
	/.git/HEAD            (Status: 403) [Size: 146]
	/.cvs                 (Status: 403) [Size: 146]
	/.cache               (Status: 403) [Size: 146]
	/.config              (Status: 403) [Size: 146]
	/.bashrc              (Status: 403) [Size: 146]
	/.history             (Status: 403) [Size: 146]
	/.htaccess            (Status: 403) [Size: 146]
	/.listings            (Status: 403) [Size: 146]
	/.htpasswd            (Status: 403) [Size: 146]
	/.listing             (Status: 403) [Size: 146]
	/.mysql_history       (Status: 403) [Size: 146]
	/.passwd              (Status: 403) [Size: 146]
	/.profile             (Status: 403) [Size: 146]
	/.perf                (Status: 403) [Size: 146]
	/.rhosts              (Status: 403) [Size: 146]
	/.sh_history          (Status: 403) [Size: 146]
	/.ssh                 (Status: 403) [Size: 146]
	/.subversion          (Status: 403) [Size: 146]
	/.svn                 (Status: 403) [Size: 146]
	/.svn/entries         (Status: 403) [Size: 146]
	/.swf                 (Status: 403) [Size: 146]
	/.web                 (Status: 403) [Size: 146]

## Transfère de zone
dig axfr -p54011 ch11.challenge01.root-me.org @challenge01.root-me.org
	; <<>> DiG 9.18.11-2-Debian <<>> axfr -p54011 ch11.challenge01.root-me.org @challenge01.root-me.org
	;; global options: +cmd
	ch11.challenge01.root-me.org. 604800 IN SOA     ch11.challenge01.root-me.org. root.ch11.challenge01.root-me.org. 2 604800 86400 2419200 604800
	ch11.challenge01.root-me.org. 604800 IN TXT     "DNS transfer secret key : CBkFRwfNMMtRjHY"
	ch11.challenge01.root-me.org. 604800 IN NS      ch11.challenge01.root-me.org.
	ch11.challenge01.root-me.org. 604800 IN A       127.0.0.1
	challenge01.ch11.challenge01.root-me.org. 604800 IN A 192.168.27.101
	ch11.challenge01.root-me.org. 604800 IN SOA     ch11.challenge01.root-me.org. root.ch11.challenge01.root-me.org. 2 604800 86400 2419200 604800
	;; Query time: 76 msec
	;; SERVER: 212.129.38.224#54011(challenge01.root-me.org) (TCP)
	;; WHEN: Wed Feb 15 16:51:20 EST 2023
	;; XFR size: 6 records (messages 1, bytes 274)

# Flag : CBkFRwfNMMtRjHY