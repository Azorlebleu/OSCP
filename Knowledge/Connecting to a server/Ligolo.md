```bash
sudo ip tuntap add user kali mode tun ligolo

sudo ip link set ligolo up

sudo ip route add 10.10.100.0/24 dev ligolo

# Proxy:
./proxy -selfcert

# Agent:
./agent.exe -connect 192.168.45.208:11601 -ignore-cert

# Commands
session
# Select one command
start
```