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

A public key and private key pair can be used to

- to decrypt the encrypted data
- using OpenSSL
