# Chapitre 0 - The Beginning
https://notagoodbye.unit41.fr/TheBeginning

![[Pasted image 20250327205631.png]]
Looking for things online. Google dorks
https://www.google.com/search?q=%22newbiecenter%22&num=10&sca_esv=e1bc306dcb0911c4&biw=1362&bih=998&sxsrf=AHTn8zqnl577OiC7mN1W4dYkQPFZ5MfDsw%3A1743105582660&ei=Lq7lZ-v6J-XKwPAPx7rU-Qw&ved=0ahUKEwjryb2ohquMAxVlJRAIHUcdNc8Q4dUDCBA&uact=5&oq=%22newbiecenter%22&gs_lp=Egxnd3Mtd2l6LXNlcnAiDiJuZXdiaWVjZW50ZXIiMgQQIxgnMgUQABjvBTIIEAAYgAQYogQyCBAAGIAEGKIESNkTUOwRWOwRcAF4AJABAJgBVaABVaoBATG4AQPIAQD4AQGYAgKgAlnCAgoQIxiwAxgnGK4CwgILEAAYsAMYogQYiQXCAgsQABiABBiwAxiiBMICCBAAGLADGO8FmAMAiAYBkAYGkgcBMqAHtgOyBwExuAdX&sclient=gws-wiz-serp

Found a page with different users.
Finding user 2:
https://www.newbiecontest.org/index.php?page=info_membre&id=2

![[Pasted image 20250327210111.png]]
Found the Romain :)

Looking for messages from him

Dork:
https://www.google.com/search?q=site%3Anewbiecontest.org+%22Folcan%22+%22message%22+%22message%22&num=10&sca_esv=e1bc306dcb0911c4&sxsrf=AHTn8zrsCyYjjvax5ODTcCJrWk3Lo7_zWA%3A1743106524815&ei=3LHlZ7y_MZO-wPAP1NqqkQU&ved=0ahUKEwi8nd7piauMAxUTHxAIHVStKlIQ4dUDCBA&uact=5&oq=site%3Anewbiecontest.org+%22Folcan%22+%22message%22+%22message%22&gs_lp=Egxnd3Mtd2l6LXNlcnAiM3NpdGU6bmV3YmllY29udGVzdC5vcmcgIkZvbGNhbiIgIm1lc3NhZ2UiICJtZXNzYWdlIkizGFDwB1ixF3ACeACQAQCYAS6gAe0CqgEBObgBA8gBAPgBAZgCAKACAJgDAIgGAZIHAKAHlQOyBwC4BwA&sclient=gws-wiz-serp

Finally found: https://www.newbiecontest.org/forums/index.php?topic=3.msg25;topicseen#msg25
![[Pasted image 20250327212353.png]]
**MA{NC_never_die}**

# Chapitre 1 - NetStaff
https://notagoodbye.unit41.fr/N3tSt4ff

![[Pasted image 20250327213845.png]]
Downloading a .doc. Opening oletools on Kali

![[Pasted image 20250327213343.png]]

Sacrée gueule le PV de recette ptn
![[Pasted image 20250327213550.png]]
Ez :D

# Chapitre 2 - Orange Cyberdefense
https://notagoodbye.unit41.fr/OCD4EVER
![[Pasted image 20250327213828.png]]

```bash
# Mdp: N3tSt4ff! "N3tSt4ff!" 
# Downloaded .zip

# Files are plain text, and KeePass database
	# KeePass password is not N3tSt4ff!

# Found host + port: 90.84.174.151 1194
# Found certificates & config file for OpenVPN.
# Building the different files: ca.crt, client.crt, client.key, client.ovpn
sudo openvpn --config client.ovpn --tls-client --ca ca.crt --cert client.crt --key client.key
	# Connection is successful, but the connection is reset instantly

# Using directly the labo file as openvpn config - works!

# Creating a wordlist from the challenge text

keepass2john Extract/Database | grep -o "$keepass$.*" >  CrackThis.hash
hashcat -a 0 -m 13400 -o output.txt --outfile-format 2 CrackThis.hash wordlist
	# The password was indeed N3tSt4ff! ...

# Opening the kdbx file properly (1st version)
# Works!

# Adding cops.local to /etc/hosts
```
![[Pasted image 20250327223045.png]]
**MA{Mam_was_the_first_dev}**

# Chapitre 2.2 - Interlude

> Qu'est ce que la UNIT41 ? Pourquoi ce domaine ? Que cherche t'il a nous dire ? Un texte sur le domaine nous donnera la suite. Chaque mot semble avoir son importance...

```bash
# Look at the TXT records for notagoodbye.unit41.fr

```
![[Pasted image 20250327223354.png]]
**MA{DNS_Can_Hide_Me}**

# Chapitre 3 
![[Pasted image 20250327224713.png]]

Testing various payloads - it mentions a "serpent", and says that Python may be able to break the AI.


![[Pasted image 20250327225640.png]]![[Pasted image 20250327225653.png]]

**MA{LLM_are_hackable}**


# Chapitre 4 - I'm an analyst

![[Pasted image 20250327231521.png]]

Can't find the last one - will brute force lmao
Getting all the 208 techniques from https://attack.mitre.org/techniques/enterprise/

```bash
# Using Hydra
hydra  -P techniques -f -V notagoodbye.unit41.fr http-post-form "/analystSOC:answer%5B0%5D=03-22+00%3A10%3A57&answer%5B1%5D=42420&answer%5B2%5D=45.63.89.234&answer%5B3%5D=34.98.123.45&answer%5B4%5D=81929ee32ef3abdae9c5f78b94ac3d7d&answer%5B5%5D=^PASS^&answer%5B6%5D=%2Fetc%2Fcron.d%2Fsystem-update&answer%5B7%5D=cdn-storage.googledriveuser.com"
```
