1. Start up the _Walkthrough VM 1_ and modify the Kali **/etc/hosts** file to reflect the provided dynamically-allocated IP address that has been assigned to the _offsecwp_ instance. Use Firefox to get familiar with the Developer Debugging Tools by navigating to the _offsecwp_ site and replicate the steps shown in this Learning Unit. Explore the entire WordPress website and inspect its HTML source code in order to find the flag.

> Open the About Us page, and control F for OS{


2. Start _Walkthrough VM 2_ and replicate the curl command we learned in this section in order to map and exploit the vulnerable APIs. Next, perform a brute force attack to discover another API that has a same pattern as **/users/v1**. Then, perform a query against the base path of the new API: what's the name of the item belonging to the _admin_ user?

```bash

# Enumerating APIs
gobuster dir -u http://192.168.105.16:5002 -w /usr/share/wordlists/dirb/common.txt -p pattern

# Found something: /console

```

1. This website running on the Exercise VM 1 is dedicated to all things maps! Follow the maps to get the flag.

![[Pasted image 20250307000747.png]]

Navigating through the files, we find the password split into 2 parts.