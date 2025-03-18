1. From your Kali Linux VM, connect to the remote MySQL instance on VM 1 and replicate the steps to enumerate the MySQL database. Then explore all values assigned to the user _offsec_. Which _plugin_ value is used as a password authentication scheme?

```bash
# IP: 192.168.122.16

# Connect to the databse:
mysql -u root -p'root' -h 192.168.122.16 -P 3306 --ssl=False
mysql -u root -p'root' -h 192.168.122.16 -P 3306 --skip-ssl

# Quite straight forward
show databases;
use mysql;
show tables;
select * from user; # Returns way too much
describe user;
select * from user where User = "offsec";
select plugin from user where User = "offsec";


```
![[Pasted image 20250314172306.png]]

2. From your Kali Linux VM, connect to the remote MSSQL instance on VM 2 and replicate the steps to enumerate the MSSQL database. Then explore the records of the _sysusers_ table inside the master database. What is the value of the first user listed?


```bash
# IP: 192.168.122.18

# Connect to the MSSQL database:
impacket-mssqlclient Administrator:Lab123@192.168.122.18 -windows-auth

# Enum
SELECT @@version;
SELECT name FROM sys.databases;
use master;
SELECT * FROM information_schema.tables;
select * from sysusers;
SELECT name FROM sysusers;
SELECT name, isntuser FROM sysusers; # For some reason the order of the data changes depending on the columns we want to display XD
```



3. From your Kali Linux VM, connect to the remote MySQL instance on VM 3 and explore the _users_ table present in one of the databases to get the flag.

```bash
#IP: 192.168.122.16

mysql -u root -p'root' -h 192.168.122.16 -P 3306 --skip-ssl

```
![[Pasted image 20250314173637.png]]