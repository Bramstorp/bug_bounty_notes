```nmap --script=mysql-info 10.129.233.146```

```
Starting Nmap 7.92 ( https://nmap.org ) at 2022-09-17 18:55 EDT
Nmap scan report for 10.129.233.146
Host is up (0.033s latency).
Not shown: 999 closed tcp ports (conn-refused)
PORT     STATE SERVICE
3306/tcp open  mysql
| mysql-info:
|   Protocol: 10
|   Version: 5.5.5-10.3.27-MariaDB-0+deb10u1
|   Thread ID: 60
|   Capabilities flags: 63486
|   Some Capabilities: ODBCClient, FoundRows, SupportsLoadDataLocal, Support41Auth, IgnoreSigpipes, IgnoreSpaceBeforeParenthesis, DontAllowDatabaseTableColumn, SupportsCompression, Speaks41ProtocolOld, InteractiveClient, Speaks41ProtocolNew, SupportsTransactions, LongColumnFlag, ConnectWithDatabase, SupportsMultipleStatments, SupportsAuthPlugins, SupportsMultipleResults
|   Status: Autocommit
|   Salt: cxPh8~5+v4=v\J<.nb~u
|_  Auth Plugin Name: mysql_native_password

Nmap done: 1 IP address (1 host up) scanned in 20.94 seconds
```
