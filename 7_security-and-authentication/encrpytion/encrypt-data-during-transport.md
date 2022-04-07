# Basics

Transport encryption ensures that

- transport of the data
- from the client (e.g an React app via the MongoDB driver)
- to the MongoDB server
- is secure
- so that nobody nobody can
  - access or manipulate data
  - on the database

MongoDB uses TLS/SLL

- to encrypt data
- during the transport

Services like OpenSSL (self-signed certificate) can be used to

- generate a public key and private key pair to
- encrypt data before it is send (client) AND
- decrypt it after it reached its destination (MongoDB server)

Officially-signed certificates

- should be used
- for production

# How to use SSL to encrypt data during its transport **FOR WINDOWS**

Ref: https://stackoverflow.com/questions/10175812/how-to-generate-a-self-signed-ssl-certificate-using-openssl

1. download and install OpenSSL (e.g. `Wind64 OpenSSL v.3.0.2` https://slproweb.com/products/Win32OpenSSL.html)
2. open the terminal as administrator
3. navigate into the OpenSSL install folder (e.g. C:\Program Files\OpenSSL-Win64\bin)
4. **generate a self-signed SSL certificate** using OpenSSL via `openssl req -newkey rsa:2048 -new -x509 -days 365 -nodes -out mongodb-cert.crt -keyout mongodb-cert.key`
   - fill in all information
   - BUT type in `localhost` or the `server ip` for the `common name`
5. **create a .pem** file using the .key and .crt file `type mongodb-cert.key mongodb-cert.crt > mongodb.pem`
6. move the .pem file whereever you need it
7. navigate to the folder via the terminal
8. **start the MongoDB server** by running mongod with SSL encryption via these two options:
   1. only SSL `mongod --dbpath C:\mongodb\data --sslMode requireSSL --sslPEMKeyFile mongodb.pem`
   2. SSL and authentication `mongod --dbpath C:\mongodb\data --auth --sslMode requireSSL --sslPEMKeyFile mongodb.pem`
9. open a new terminal as administrator
10. navigate into the folder with the -pem file
11. **start the MongoDB client** `mongo --ssl --sslCAFile mongodb.pem --host localhost`
