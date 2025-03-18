
```bash
# Send a GET request
curl -i http://192.168.50.16:5002/users/v1/login

# Send a POST request
curl -d '{"password":"fake","username":"admin"}' -H 'Content-Type: application/json'  http://192.168.50.16:5002/users/v1/login

# POST Request again
curl -X POST --data 'Archive=ipconfig' http://192.168.50.189:8000/archive

# Download a file
curl --output file.txt "google.com/something.img"
```