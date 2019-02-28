# Supported PKCS \#11 Key Types<a name="pkcs11-key-types"></a>

The AWS CloudHSM software library for PKCS \#11 supports the following key types\.
+ **RSA** – 2048\-bit to 4096\-bit RSA keys, in increments of 256 bits\.
+ **ECDSA** – Generate keys with the P\-224, P\-256, P\-384, P\-521, and secp256k1 curves\. Only the P\-256, P\-384, and secp256k1 curves are supported for sign/verify\.
+ **AES** – 128, 192, and 256\-bit AES keys\.
+ **Triple DES \(3DES\)** – 192\-bit keys\.
+ **GENERIC\_SECRET** – 1 to 64 bytes\.