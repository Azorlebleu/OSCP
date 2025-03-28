```bash
# Classic with user
smbmap -u guest -H 192.168.249.10

# Examples
smbmap -u jsmith -p password1 -d workgroup -H 192.168.0.1
smbmap -u jsmith -p 'aad3b435b51404eeaad3b435b51404ee:da76f2c4c96028b7a6111aef4a50a94d' -H 172.16.0.20
smbmap -u 'apadmin' -p 'asdf1234!' -d ACME -Hh 10.1.3.30 -x 'net group "Domain Admins" /domain'
```