# Supported key types<a name="pkcs11-key-types"></a>

The PKCS \#11 library supports the following key types\.


****  

| Key Type | Description | 
| --- | --- | 
| RSA | Generate 2048\-bit to 4096\-bit RSA keys, in increments of 256 bits\. | 
| EC | Generate keys with the secp224r1 \(P\-224\), secp256r1 \(P\-256\), secp256k1 \(Blockchain\), secp384r1 \(P\-384\), and secp521r1 \(P\-521\) curves\. | 
| AES | Generate 128, 192, and 256\-bit AES keys\.  | 
| Triple DES \(3DES\) | Generate 192\-bit Triple DES keys\. | 
| GENERIC\_SECRET | Generate 1 to 800 bytes generic secrets\. | 