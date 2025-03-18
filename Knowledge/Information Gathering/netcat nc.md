# TCP Port scanner
```bash
# Listing open ports
nc -nv -w 1 -z 192.168.163.151 1-100 | grep "open"

# Full verbose mode
nc -nvv -w 1 -z 192.168.163.151 1-100
```
# UDP Port scanner
```bash
nc -nv -u -z -w 1 192.168.163.151 150-200
```
# SMTP Port scanner
```bash
nc -nv 192.168.50.8 25
```
