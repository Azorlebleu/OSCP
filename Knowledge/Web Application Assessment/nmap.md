
# Service discovery

```bash
# Finding the exposed services
sudo nmap -p80  -sV 192.168.50.20

# Fingerprinting
sudo nmap -p80 --script=http-enum 192.168.50.20
```