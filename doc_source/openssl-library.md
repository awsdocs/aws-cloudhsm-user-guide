# OpenSSL Dynamic Engine<a name="openssl-library"></a>

AWS CloudHSM offers two implementations of the OpenSSL Dynamic Engine: Client SDK 3 and Client SDK 5\. 

Client SDK 3 requires a client daemon to connect to the cluster\. It supports:
+ RSA key generation for 2048, 3072, and 4096\-bit keys\.
+ RSA sign/verify\.
+ RSA encrypt/decrypt\.
+ Random number generation that is cryptographically secure and FIPS\-validated\.

Client SDK 5 doesn't require a client daemon to connect to the cluster\. It supports:
+ RSA key generation for 2048, 3072, and 4096\-bit keys\.
+ RSA sign/verify\. Verification is offloaded to OpenSSL software\.
+ ECDSA sign/verify for P\-256, P\-384, and secp256k1 key types\.

  To generate EC keys that are interoperable with the OpenSSL engine, see [getCaviumPrivKey](key_mgmt_util-getCaviumPrivKey.md)\.

For information about platform support for SDKs, see [Supported platforms for the client SDKs](client-supported-platforms.md)\.

**Topics**
+ [OpenSSL Dynamic Engine Client SDK 3](openssl3-install.md)
+ [OpenSSL Dynamic Engine Client SDK 5](openssl5-install.md)