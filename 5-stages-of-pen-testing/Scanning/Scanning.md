# Testing env

Vulnerable virtual machine: https://information.rapid7.com/download-metasploitable-2017.html?LS=1631875&CS=web

free: https://sourceforge.net/projects/metasploitable/files/latest/download

# Scanning local netdiscover

```sudo netdiscover```

```netstat -nr```

# Nmap scan ip for open ports etc.

```nmap <Target ip>```

```nmap -sS <Target ip> - TCP Scan```

```nmap -sT <Target ip> - TCP Scan```

```nmap -sU <Target ip> - UDP Scan```

```nmap -O <Target ip>  - Operating system```

```nmap -sV <Target ip> - Version of service running on an open port```

```nmap -A <Target ip>  -Enable OS detection, version detection, script scanning, and traceroute ```

```nmap -sn <Target ip>-<1-255 range> ```

```nmap -p <port ranges>: Only scan specified ports Ex: -p22; -p1-65535; -p U:53,111,137,T:21-25,80,139,8080,S:9 ```


```nmap -f <Target ip> - Fast mode - Scan fewer ports than the default scan```

```nmap  -sS <Target ip> >> outputofscan.txt - create output in file```

if ports are filtered try this

```nmap -D RND:5 <Target ip>```

```nmap -D RND:5 <Target ip> -sS```

check wireshark