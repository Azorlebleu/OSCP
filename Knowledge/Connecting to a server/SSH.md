
```bash
# Classic connection
ssh offsec@mountaindesserts.com

# Specify the port
ssh -p 6969 offsec@mountaindesserts.com

# Connecting using a SSH key
chmod 600 dt_key # The key file must only be readable by our current user
rm ~/.ssh/known_hosts # Cleaning past sessions
ssh -i dt_key -p 2222 offsec@mountaindesserts.com
```

