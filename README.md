<h1>My Bug Bounty Hunting Methodology</h1>



<h2>Recon :-</h2>


```bash
 assetfinder --subs-only vulncrax.com | sudo httpx -mc 200 -o live-subs.txt 



```bash
cat live-subs.txt | sudo  gau --threads 5 --o links.txt
```

```bash
cat all-subs.txt | httpx -o live-subs.txt
```

```bash
cat live-subs.txt | dirsearch --stdin
```

```bash
ffuf -u https://target.com/FUZZ -w wordlist.txt -mc 200,403,301,302 -c true -v -o output.txt
```
<h2>Subdomain Takeover :-</h2>

```bash
nuclei -t /root/nuclei-templates/takeovers/ -l live-subs.txt
```
```bash
subzy run --targets live-subs.txt
```
```bash
subzy run --target test.target.com
```

```bash
subzy run --target test.target1.com,https://test.target2.com
```
<h2>collecting urls and Parameters :</h2>

```bash

waymore -i target.com -mode U -oU result.txt
```

```bash
cat result.txt | sort -u > sorted.txt
```

<h2>#Getting live urls :-</h2>

```bash
cat sorted.txt | httpx -mc 200 -o live-urls.txt
```

<h2>#Getting parameters from live urls :-</h2>

```bash
cat live-urls.txt | grep "=" > live-parameters.txt
```

<h2>JS Hunting :-</h2>

```bash
echo target.com | gau | grep ".js" | httpx -content-type | grep 'application/javascript'" | awk '{print $1}' | nuclei -t /root/nuclei-templates/exposures/ -silent > secrets.txt
```

```bash
 echo exapmle.com | gau | grep '.js$' | httpx -status-code -mc 200 -content-type | grep 'application/javascript'

```
```bash
echo "example.com" | waybackurls | grep -iE '.js'|grep -ivE '.json'|sort -u  > j.txt
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
```bash
net:192.168.43/24, 192.168.40/24
```
<h2>Collect all interisting ips from Shodan and save them in ips.txt</h2>

```bash
cat ips.txt | httpx > live-ips.txt
```

```bash
cat live_ips.txt | dirsearch --stdin
```










