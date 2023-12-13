### We can build TLS from any protocol from Layer 7 down to TLS Layer that can be encrypted

## HTTPs
- we do a handshake
- we share the same encryption key with the client and the server
- once we have these keys we use them to encrypt the request
- We encrypt with symmetric key algorithm, where a key is need to be exchanged between the client and the server
- Key uses asymmetric key (PKI), where there are two different keys meant for encryption and decryption
- authenticate the server