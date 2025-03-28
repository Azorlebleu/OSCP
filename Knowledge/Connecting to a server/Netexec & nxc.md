
https://www.netexec.wiki/?q=

```bash
# SMB enum
nxc smb 192.168.249.10 -u "guest" -p "" --shares

# SMB vulnerabilities
nxc smb 192.168.249.10 -u 'g' -p '' -M zerologon -M printnightmare -M smbghost -M  ms17-010
```