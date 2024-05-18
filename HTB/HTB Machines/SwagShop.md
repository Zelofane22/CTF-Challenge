#HackTheBox #linux #easy 
# Enum
## _nmap_

## _gobuster_
/app                  (Status: 301) [Size: 310] [--> http://swagshop.htb/app/]
/errors               (Status: 301) [Size: 313] [--> http://swagshop.htb/errors/]
/favicon.ico          (Status: 200) [Size: 1150]
/includes             (Status: 301) [Size: 315] [--> http://swagshop.htb/includes/]
/index.php            (Status: 200) [Size: 16593]
/js                   (Status: 301) [Size: 309] [--> http://swagshop.htb/js/]
/lib                  (Status: 301) [Size: 310] [--> http://swagshop.htb/lib/]
/media                (Status: 301) [Size: 312] [--> http://swagshop.htb/media/]
/pkginfo              (Status: 301) [Size: 314] [--> http://swagshop.htb/pkginfo/]
/server-status        (Status: 403) [Size: 300]
/shell                (Status: 301) [Size: 312] [--> http://swagshop.htb/shell/]
/skin                 (Status: 301) [Size: 311] [--> http://swagshop.htb/skin/]
/var                  (Status: 301) [Size: 310] [--> http://swagshop.htb/var/]

# Find
in http://swagshop.htb/app/etc/local.xml
```
<key>
<![CDATA[ b355a9e0cd018d3f7f03607141518419 ]]>
</key>
<password>
<![CDATA[ fMVWh7bDHpgZkyfqQXreTjU9 ]]>
</password>
<dbname>
<![CDATA[ swagshop ]]>
</dbname>
```
in http://swagshop.htb/RELEASE_NOTES.txt
```
version 1.7.0.2
```

