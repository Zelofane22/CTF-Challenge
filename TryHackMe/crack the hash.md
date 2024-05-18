#TryHackMe
#crackPass
┌──(root㉿kali)-[/media/sf_kali_partage/challenge/THM/crack_the_hash]
└─# echo "CBFDAC6008F9CAB4083784CBD1874F76618D2A97" > sha1

┌──(root㉿kali)-[/media/sf_kali_partage/challenge/THM/crack_the_hash]
└─# john sha1 -wordlist=/usr/share/wordlists/rockyou.txt 
	password123      (?)

┌──(root㉿kali)-[/media/sf_kali_partage/challenge/THM/crack_the_hash]
└─# echo "1C8BFE8F801D79745C4631D09FFF36C82AA37FC4CCE4FC946683D7B336B63032" > sha-256

┌──(root㉿kali)-[/media/sf_kali_partage/challenge/THM/crack_the_hash]
└─# john sha-256 -wordlist=/usr/share/wordlists/rockyou.txt --format=Raw-SHA256      
	letmein          (?)

