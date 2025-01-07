# apiWithAuth
Building api with authentication in place. 

# Steps to run the application
if you have python installed in your system 
```bash
pip install -r requirements.txt && fastapi run api.py 
```
else if docker is available then run the following command 
```
docker build -t api-with-auth . && docker run -d -p 127.0.0.1:8000:8000 api-with-auth
```

# Details
The authorization will happened based on Authorization: Bearer token based method with refresh_token in the cookie.
The token revocation will happen if the user logs out of the system. 



# Tests 
The curl commands to test the application are 

to sign up for the application 


```bash
curl -X 'POST' \
  'http://localhost:8000/signup' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "username": "tanmaykhabia",
  "email": "tanmaykhabia44@gmail.com",
  "full_name": "tanmay Khabia",
  "disabled": false,
  "password": "123"
}'
```

To log into the application 
```bash
curl -X 'POST' \
  'http://localhost:8000/token' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  -d 'username=tanmaykhabia&password=123'
```
This will return the access token and the refresh token for your reference 

to refresh the token use the following command. Note update the refresh token 
```bash
curl 'http://localhost:8000/token/refresh' \
  -X 'POST' \
  -H 'Accept-Language: en-GB,en;q=0.9,en-US;q=0.8,en-IN;q=0.7' \
  -H 'Connection: keep-alive' \
  -H 'Content-Length: 0' \
  -H 'Cookie: refresh_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJ0a2hhYmlhIiwiZXhwIjoxNzM2ODcwODI4fQ.7IaY9N2UF6kstLRwYo5Nf8mlEC6XqPOdMjgcKnaNbU4' 
```

to access the application. Keep note of your application refresh token and access token as they will change for different user. 

```bash
curl 'http://localhost:8000/users/me/' \
  -H 'Accept-Language: en-GB,en;q=0.9,en-US;q=0.8,en-IN;q=0.7' \
  -H 'Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJ0YW5tYXlraGFiaWEiLCJleHAiOjE3MzYyNjg5MDh9.TfGhFC4h7JPuYg3Zld2Id-OVe0TaNFk1RiftQOIjaWc' \
  -H 'Connection: keep-alive' \
  -H 'Cookie: refresh_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJ0YW5tYXlraGFiaWEiLCJleHAiOjE3MzY4NzE5MDh9.5g9iKWmE-LZenZwWqqGZ6uHa4SzitOEMa-g2kRY57h8' \
```

to log out of the application 
```bash
curl -X 'POST' \
  'http://localhost:8000/logout' \
  -H 'accept: application/json' \
  -d ''
```
This will revoke the token and the user cannot use the same access token and refresh_token 