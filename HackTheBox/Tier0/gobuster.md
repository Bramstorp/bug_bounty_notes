Cheatsheat: https://linuxcscom.wordpress.com/gobuster/

## brute-force URI

`gobuster dir -u http://{target_ip} -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt` 

search for specefic files like php

`gobuster dir -x php -u [http](http://{target_ip}) -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt`
