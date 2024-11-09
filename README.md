<h1>My Bug Bounty Hunting Methodology</h1>



<h2>Recon :-</h2>


```bash
 assetfinder --subs-only vulncrax.com | sudo httpx  -mc 200 -o live-subs.txt
```

```bash
cat live-subs.txt | sudo httpx -status-code -title -tech-detect -mc 200
```


```bash
cat live-subs.txt | sudo  gau --threads 5 --o links.txt
```



```bash
cat live-subs.txt | sudo dirsearch --stdin
```

```bash
ffuf -u https://target.com/FUZZ -w wordlist.txt -mc 200,403,301,302 -c true -v -o output.txt
```
<h2>Finding api end point :-</h2>

```bash
python3 api_fuzzer.py https://target.com
```



<h2>Subdomain Takeover :-</h2>

```bash
nuclei -t /root/nuclei-templates/takeovers/ -l live-subs.txt
```
```bash
subzy run --targets live-subs.txt
```





<h2>collecting urls and Parameters :</h2>



```bash
cat live-subs.txt | grep -i "="

```



<h2>Shodan Dorking :-</h2>

```bash
ssl.cert.subject.CN:"example.com*" 200
```

```bash
ssl.cert.subject.CN:"*.example.com" "230 login successful" port:"21"
```
```bash
ssl.cert.subject.CN:"*.target.com"+200 http.title:"Admin"
```

```bash
ssl:"target.com" http.title:"index of / "
```








