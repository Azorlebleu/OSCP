#Windows 
# Running it
```bash
telnet 192.168.50.8 25
```

# Installing it
```bash
# If it's not already installed on the host
dism /online /Enable-Feature /FeatureName:TelnetClient
```