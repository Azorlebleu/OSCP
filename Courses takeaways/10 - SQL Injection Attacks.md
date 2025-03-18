Lucas ref: https://red.infiltr8.io/web-pentesting/web-vulnerabilities/server-side/sql-injection/union-attacks

```bash
# SQL example query
$sql_query = "SELECT * FROM users WHERE user_name= '$uname' AND password='$passwd'";

# Basic SQL injection example
offsec' OR 1=1 -- //                       '

# Count the amount of columns 
' ORDER BY 1-- //                          '
' ORDER BY 5-- //                          '
	# Breaking at N means there are N-1 columns

```