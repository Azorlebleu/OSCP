https://github.com/edermi/skweez
```bash
# Common
skweez http://192.168.249.11/ -d 3 --min-word-length 2 --max-word-length 30 --onlyascii > passwords.txt
# From website with depth 3 & Output to file
skweez http://192.168.249.11/ -d 3 > passwords.txt

# Specify word size
skweez http://192.168.249.11/ -d 3 --min-word-length 5 --max-word-length 30

# Only ASCII
skweez http://192.168.249.11/ -d 3 --onlyascii


# Install
git clone https://github.com/edermi/skweez.git
cd skweez
go build
```