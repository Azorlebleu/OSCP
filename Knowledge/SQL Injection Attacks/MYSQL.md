
```bash
# Connect to the database
mysql -u root -p'root' -h 192.168.122.16 -P 3306 --ssl=False
mysql -u root -p'root' -h 192.168.122.16 -P 3306 --skip-ssl

# Once connected to the database:

select version();
select system_user();
show databases;
use database_name;
show tables;
describe table; # Show schema



```