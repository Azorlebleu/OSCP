
1. We have a lot of mess on our hands, and the new _DIRTBUSTER_ cleaning service is just what we need to help with the cleanup! You can visit their new site on the Module Exercise VM #1, but it is still under development. We wonder where they hid their admin portal. Once found the admin portal, log-in with the provided credentials to obtain the flag.


```bash
gobuster dir -u 192.168.105.52 -w /usr/share/wordlists/dirb/common.txt
```

![[Pasted image 20250306230226.png]]

1. The DIRTBUSTER team finally changed their default credentials, but they are not very original. We complied at _http://target_vm/passwords.txt_ of potential passwords from the DIRTBUSTER employee contact info - I am confident the password is in there somewhere. The username is still _admin_, and the new login portal is available at the web server root folder on the Module Exercise VM #2.

> Configured Burp to use the Intruder section of it. Described in the course: 8.2.4 :)

