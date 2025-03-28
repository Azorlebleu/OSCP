
```bash
# Enumerate shares with guest connection
smbclient -L 192.168.249.10 -U guest%

# Reading a specific share
smbclient -L 192.168.249.10/offsec -U guest%

# Connection with a specific password
smbclient -L 192.168.249.10/offsec -U offsec%lab

# Pushing a file via SMB no auth
smbclient //192.168.50.223.195/share -c 'put config.Library-ms'

# Download all files
mask "" 
recurse ON 
prompt OFF 
mget *
	# This only works for the files where the user has the right permissions!
```