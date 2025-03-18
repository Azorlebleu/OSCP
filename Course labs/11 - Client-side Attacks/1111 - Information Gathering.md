1. Download **old.pdf** from the _Mountain Vegetables_ website on VM #1 by clicking on the **OLD** button. Use _exiftool_ to review the file's metadata. Enter the value of the _Author_ tag.

```bash
# IP: 192.168.202.197

# Running exiftool
exiftool -a -u old.pdf
```

![[Pasted image 20250316124257.png]]

2. Start VM #2 and use _gobuster_ to bruteforce the contents of the web server. Specify "pdf" as the filetype and find a document other than **old.pdf** and **brochure.pdf**. After you identify the file, download it and extract the flag in the metadata.

```bash
# IP: 192.168.202.197

# Running gobuster with -x extension:
gobuster dir -u 192.168.202.197 -w /usr/share/wordlists/dirb/common.txt -t 10 -x .pdf
	# Found info.pdf

# Running exiftool
exiftool -a -u info.pdf
```

![[Pasted image 20250316124543.png]]
