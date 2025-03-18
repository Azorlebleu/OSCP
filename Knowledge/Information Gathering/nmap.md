# Arguments

```bash
# Recommendation from Nils
sudo nmap -sV -sC -v ip

# Stealth Syn
sudo nmap -sS 192.168.163.1-254

# Connect Scan (to run when the user is not administrator, or want to go through proxies)
sudo nmap -sT 192.168.163.1-254

# UDP Scan 
sudo nmap -sU 192.168.163.1-254

# Combining different scans
sudo nmap -sS -sU 192.168.163.1-254

# Host discovery
sudo nmap -sn 192.168.50.1-253

# Saving to file
sudo nmap 192.168.163.1 -oN scan_result.txt

# Making it greppable
sudo nmap 192.168.163.1 -oG scan_result.txt

# Probing a specific port
sudo nmap -p 445 192.168.163.1

# Choosing top ports
sudo nmap -sT --top-ports=20 192.168.50.1-253

# Enable OS version detection + scripts scanning + traceroute
sudo nmap -sT -A 192.168.50.1-253

# OS Fingerprinting
sudo nmap -O 192.168.50.14 --osscan-guess

# Read input from a file
sudo nmap -iL hosts.txt

# Recommendation from Alex
sudo nmap -sVC -Pn 
# C launches the default scripts
# V is for the services
# Pn to remove ICMP / pings



# SMB enum
nmap -v -p 139,445 -oN smb.txt 192.168.50.1-254

# SNMP enum
sudo nmap -sU --open -p 161 192.168.50.1-254 -oG open-snmp.txt
```

# Scripts
```bash
# HTTP Script
sudo nmap --script http-headers 192.168.50.6
sudo nmap --script http-titles 192.168.163.1-254
nmap -v -p 139,445 --script smb-os-discovery 192.168.50.152
```