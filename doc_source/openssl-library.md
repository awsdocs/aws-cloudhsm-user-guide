# OpenSSL Dynamic Engine<a name="openssl-library"></a>

The AWS CloudHSM OpenSSL Dynamic Engine allows you to offload cryptographic operations to your CloudHSM cluster through the OpenSSL API\. OpenSSL Dynamic Engine supports the following operations:
+ RSA key generation for 2048, 3072, and 4096\-bit keys\.
+ RSA sign/verify\. Verification is offloaded to OpenSSL software\.
+ ECDSA sign/verify for P\-256, P\-384, and secp256k1 key types\.

  To generate EC keys that are interoperable with the OpenSSL engine, see [getCaviumPrivKey](key_mgmt_util-getCaviumPrivKey.md)\.

For information about platform support for SDKs, see [Client SDK 5 supported platforms](client-supported-platforms.md)\.

For information on using Client SDK 3, see [Previous Client SDK versions](choose-client-sdk.md)\.

**Topics**
+ [Installing the OpenSSL Dynamic Engine](openssl5-install.md)
+ [Advanced configurations for OpenSSL](openssl-library-configs.md)