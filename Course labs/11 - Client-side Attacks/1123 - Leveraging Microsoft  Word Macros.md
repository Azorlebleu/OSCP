1. Perform the steps from this section to create a malicious Word document containing a macro with the name **MyMacro** on the _OFFICE_ (VM #1) machine. For this, you have to install Microsoft Office on VM #1 again as outlined in the section "Installing Microsoft Office". Confirm that the macro works as expected by obtaining a reverse shell from the _OFFICE_ machine. We can log in with offsec:lab. What keyword is used to declare a variable in VBA?

```bash
# Dim
```

2. Once you have confirmed that the macro from the previous exercise works, upload the document containing the macro _MyMacro_ in the file upload form (port 8000) of the _TICKETS_ (VM #2) machine with the name **ticket.doc**. A script on the machine, simulating a user, checks for this file and executes it. After receiving a reverse shell, enter the flag from the **flag.txt** file on the desktop for the _Administrator_ user. For the file upload functionality, add **tickets.com** with the corresponding IP address in **/etc/hosts**. Please note that it can take up to three minutes after uploading the document for the macro to get executed.


```bash
# Office: 192.168.202.198
# Tickets: 192.168.202.196

# Installing Office
xfreerdp /v:192.168.202.196 /u:offsec /p:lab /dynamic-resolution /drive:/usr/share/Kali,.

# Creating a macro as described in the section
```
```python

# As Macros can only take 50 characters at once, we need to split the line
str = "powershell -e yourcommandhere"

n = 50

for i in range(0, len(str), n):
        print("Str = Str + " + '"' + str[i:i+n] + '"')
```
![[Pasted image 20250316135250.png]]
```bash
# Sharing file
impacket-smbserver pedro . -smb2support -username p -password p # Kali
net use X: \\192.168.45.250\pedro # Windows
	# You can then transfer files :)

# Can also be done via freerdp - just check the files :D

# Uploading the file through the ticketing system

# A lot of issues... had to download VPN again for it to work

# 


```