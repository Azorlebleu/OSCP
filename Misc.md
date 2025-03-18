# SSH connection

```bash
ssh -o "UserKnownHostsFile=/dev/null" -o "StrictHostKeyChecking=no" learner@192.168.50.52
```

The _UserKnownHostsFile=/dev/null_ and _StrictHostKeyChecking=no_ options have been added to prevent the known-hosts file on our local Kali machine from being corrupted.

# Locating files

```bash
sudo updatedb
locate file_name
```

# Flag format

```bash
local.txt
proof.txt
flag.txt
```

# Displaying file permissions

```bash
ls -l filename
```

# Changing file permissions

```bash
chmod +x filename # Adding execution permissions to the files
chmod -x filename # Removing them
```

# Generating custom usernames

[https://github.com/urbanadventurer/username-anarchy](https://github.com/urbanadventurer/username-anarchy)

# Copy / Paste not working
```
/usr/bin/vmtoolsd -n vmusr
```

