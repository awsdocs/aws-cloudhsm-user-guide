# Supported key types<a name="pkcs11-key-types"></a>

The PKCS \#11 library supports the following key types\.


****  

| Key Type | Description | 
| --- | --- | 
| RSA | Generate 2048\-bit to 4096\-bit RSA keys, in increments of 256 bits\. | 
| ECDSA | Generate keys with the P\-224, P\-256, P\-384, P\-521, and secp256k1 curves\. Only the P\-256, P\-384, and secp256k1 curves are supported for sign and verify\. | 
| AES | Generate 128, 192, and 256\-bit AES keys\.  | 
| Triple DES \(3DES\) | Generate 192\-bit Triple DES keys\. | 
| GENERIC\_SECRET | Generate 1 to 64 bytes generic secrets\. | 