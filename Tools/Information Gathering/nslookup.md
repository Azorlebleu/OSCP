#Windows

# Default query

```sh
nslookup megacorptwo.com
```

# TXT records

```sh
nslookup -type=TXT info.megacorptwo.com 192.168.168.151
# Look for TXT records. Asks the 2nd IP to act as the DNS server.
```
