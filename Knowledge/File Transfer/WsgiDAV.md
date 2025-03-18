This must be installed using [[Virtual Environments]].

```bash
# Install
pip3 install wsgidav

# Run it
wsgidav --host=0.0.0.0 --port=80 --auth=anonymous --root /home/kali/webdav/

# Access it
curl http://127.0.0.1:80
```