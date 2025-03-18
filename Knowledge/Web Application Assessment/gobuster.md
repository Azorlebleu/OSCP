*go* must be installed.
https://github.com/OJ/gobuster
# Discovery

```bash
# Classic scan  using a wordlist
gobuster dir -u 192.168.1.1 -w /usr/share/wordlists/dirb/common.txt
	# dir: Enumerate files and directories
	# -u: Target
	# -w: Worlist to use
	# -t: Number of threads (default is 10)

# Using a pattern

cat pattern
{GOBUSTER}/v1
{GOBUSTER}/v2

gobuster dir -u 192.168.1.1 -w /usr/share/wordlists/dirb/common.txt -p pattern

# The GOBUSTER elements will be replaced by words in the wordlist

# DNS enumeration
gobuster dns -d megacorpone.com -w wordlist.txt -t 10

# Look for specific file extension
gobuster dir -u megacorpone.com -w /usr/share/wordlists/dirb/common.txt -t 10 -x pdf

# If desperate
gobuster dir -u megacorpone.com -w /usr/share/wordlists/dirb/big.txt -t 10 -x txt,php,aspx,pdf,config,py

```