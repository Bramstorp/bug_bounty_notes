`nmap -sC 10.129.75.71`


```
Starting Nmap 7.93 ( https://nmap.org ) at 2022-11-26 08:53 EST
Nmap scan report for 10.129.75.71
Host is up (0.043s latency).
Not shown: 998 closed tcp ports (conn-refused)
PORT   STATE SERVICE
21/tcp open  ftp
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
| -rw-r--r--    1 ftp      ftp            33 Jun 08  2021 allowed.userlist
|_-rw-r--r--    1 ftp      ftp            62 Apr 20  2021 allowed.userlist.passwd
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.10.14.132
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 3
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
80/tcp open  http
|_http-title: Smash - Bootstrap Business Template
```

`ftp 10.129.75.71`

```
ftp> ls
229 Entering Extended Passive Mode (|||42698|)
150 Here comes the directory listing.
-rw-r--r--    1 ftp      ftp            33 Jun 08  2021 allowed.userlist
-rw-r--r--    1 ftp      ftp            62 Apr 20  2021 allowed.userlist.passwd
226 Directory send OK.
ftp> 
```

```
ftp> get allowed.userlist
allowed.userlist        allowed.userlist.passwd
ftp> get allowed.userlist
local: allowed.userlist remote: allowed.userlist
229 Entering Extended Passive Mode (|||45432|)
150 Opening BINARY mode data connection for allowed.userlist (33 bytes).
100% |***************************************************************************************************************|    33        4.48 KiB/s    00:00 ETA
226 Transfer complete.
33 bytes received in 00:00 (0.72 KiB/s)
ftp> get allowed.userlist.passwd
local: allowed.userlist.passwd remote: allowed.userlist.passwd
229 Entering Extended Passive Mode (|||41888|)
150 Opening BINARY mode data connection for allowed.userlist.passwd (62 bytes).
100% |***************************************************************************************************************|    62       28.29 KiB/s    00:00 ETA
226 Transfer complete.
62 bytes received in 00:00 (1.46 KiB/s)
```

```
┌──(kali㉿kali)-[~]
└─$ cat allowed.userlist
aron
pwnmeow
egotisticalsw
admin
                                                                                                                                                            
┌──(kali㉿kali)-[~]
└─$ cat allowed.userlist.passwd 
root
Supersecretpassword1
@BaASD&9032123sADS
rKXM59ESxesUFHAd
```

```
──(kali㉿kali)-[~]
└─$ gobuster dir -u http://10.129.75.71 -w /usr/share/dirb/wordlists/common.txt                  
===============================================================
Gobuster v3.3
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.129.75.71
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/dirb/wordlists/common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.3
[+] Timeout:                 10s
===============================================================
2022/11/26 09:02:47 Starting gobuster in directory enumeration mode
===============================================================
/.hta                 (Status: 403) [Size: 277]
/.htaccess            (Status: 403) [Size: 277]
/.htpasswd            (Status: 403) [Size: 277]
/assets               (Status: 301) [Size: 313] [--> http://10.129.75.71/assets/]
/css                  (Status: 301) [Size: 310] [--> http://10.129.75.71/css/]
/dashboard            (Status: 301) [Size: 316] [--> http://10.129.75.71/dashboard/]
/fonts                (Status: 301) [Size: 312] [--> http://10.129.75.71/fonts/]
/index.html           (Status: 200) [Size: 58565]
/js                   (Status: 301) [Size: 309] [--> http://10.129.75.71/js/]
/server-status        (Status: 403) [Size: 277]
Progress: 4549 / 4615 (98.57%)===============================================================
2022/11/26 09:03:04 Finished
===============================================================
```

```
┌──(kali㉿kali)-[~]
└─$ gobuster dir -u http://10.129.75.71 -w /usr/share/dirb/wordlists/common.txt -x .php
===============================================================
Gobuster v3.3
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.129.75.71
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/dirb/wordlists/common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.3
[+] Extensions:              php
[+] Timeout:                 10s
===============================================================
2022/11/26 09:04:10 Starting gobuster in directory enumeration mode
===============================================================
/.php                 (Status: 403) [Size: 277]
/.hta                 (Status: 403) [Size: 277]
/.hta.php             (Status: 403) [Size: 277]
/.htaccess            (Status: 403) [Size: 277]
/.htaccess.php        (Status: 403) [Size: 277]
/.htpasswd.php        (Status: 403) [Size: 277]
/.htpasswd            (Status: 403) [Size: 277]
/assets               (Status: 301) [Size: 313] [--> http://10.129.75.71/assets/]
/config.php           (Status: 200) [Size: 0]
/css                  (Status: 301) [Size: 310] [--> http://10.129.75.71/css/]
/dashboard            (Status: 301) [Size: 316] [--> http://10.129.75.71/dashboard/]
/fonts                (Status: 301) [Size: 312] [--> http://10.129.75.71/fonts/]
/index.html           (Status: 200) [Size: 58565]
/js                   (Status: 301) [Size: 309] [--> http://10.129.75.71/js/]
/login.php            (Status: 200) [Size: 1577]
/logout.php           (Status: 302) [Size: 0] [--> login.php]
/server-status        (Status: 403) [Size: 277]
Progress: 9165 / 9230 (99.30%)===============================================================
2022/11/26 09:04:45 Finished
===============================================================
```



