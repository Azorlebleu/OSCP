```powershell
# Test if a port is open
Test-NetConnection -Port 445 192.168.163.151

# Enumerate all ports
1..1024 | % {echo ((New-Object Net.Sockets.TcpClient).Connect("192.168.163.151", $_)) "TCP port $_ is open"} 2>$null
```
