```bash
# Enum 4 Linux
for ip in $(cat hosts.txt); do enum4linux $ip;done;

# Creating a word list
for ip in $(seq 1 254); do echo 192.168.50.$ip; done > ips
```
