# Hashid
Used to identify the hash.

```bash
hashid '$P$BINTaLa8QLMqeXbQtzT2Qfizm2P/nI0'
```
![[Pasted image 20250315005732.png]]


# Hashcat

```bash
# Find the mode
hashcat -h | grep -i 'PHPass'
```
![[Pasted image 20250315005938.png]]

```bash
# Dictionnary attack
hashcat -m <mode> -a 0 hash.txt wordlist.txt
hashcat -m 400 -a 0 hash.txt /usr/share/wordlists/rockyou.txt
```