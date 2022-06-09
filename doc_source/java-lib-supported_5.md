# Supported mechanisms for Client SDK 5<a name="java-lib-supported_5"></a>

For information about the Java Cryptography Architecture \(JCA\) interfaces and engine classes supported by AWS CloudHSM, see the following topics\. 

**Topics**
+ [Supported keys](#java-keys_5)
+ [Supported ciphers](#java-ciphers_5)
+ [Supported digests](#java-digests_5)
+ [Supported hash\-based message authentication code \(HMAC\) algorithms](#java-mac_5)
+ [Supported sign/verify mechanisms](#java-sign-verify_5)

## Supported keys<a name="java-keys_5"></a>

The AWS CloudHSM software library for Java enables you to generate the following key types\.
+ RSA – 2048\-bit to 4096\-bit RSA keys, in increments of 256 bits\.
+ AES – 128, 192, and 256\-bit AES keys\.
+ ECC key pairs for NIST curves secp256r1 \(P\-256\), secp384r1 \(P\-384\), and secp256k1 \(Blockchain\)\.
+ DESede \(Triple DES\)
+ HMAC – with SHA1, SHA224, SHA256, SHA384, SHA512 hash support\.

## Supported ciphers<a name="java-ciphers_5"></a>

The AWS CloudHSM software library for Java supports the following algorithm, mode, and padding combinations\.


| Algorithm | Mode | Padding | Notes | 
| --- | --- | --- | --- | 
| AES | CBC |  `AES/CBC/NoPadding` `AES/CBC/PKCS5Padding`  | Implements `Cipher.ENCRYPT_MODE` and `Cipher.DECRYPT_MODE`\.  | 
| AES | ECB |  `AES/ECB/PKCS5Padding` `AES/ECB/NoPadding`  | Implements `Cipher.ENCRYPT_MODE` and `Cipher.DECRYPT_MODE`\.  | 
| AES | CTR |  `AES/CTR/NoPadding`  |  Implements `Cipher.ENCRYPT_MODE` and `Cipher.DECRYPT_MODE`\.  | 
| AES | GCM | `AES/GCM/NoPadding` | Implements `Cipher.WRAP_MODE`, `Cipher.UNWRAP_MODE`, `Cipher.ENCRYPT_MODE`, and `Cipher.DECRYPT_MODE`\.When performing AES\-GCM encryption, the HSM ignores the initialization vector \(IV\) in the request and uses an IV that it generates\. When the operation completes, you must call `Cipher.getIV()` to get the IV\. | 
| AESWrap | ECB |  `AESWrap/ECB/NoPadding` `AESWrap/ECB/PKCS5Padding` `AESWrap/ECB/ZeroPadding`  | Implements `Cipher.WRAP_MODE` and `Cipher.UNWRAP_MODE`\.  | 
| DESede \(Triple DES\) | CBC |  `DESede/CBC/PKCS5Padding` `DESede/CBC/NoPadding`  |  Implements `Cipher.ENCRYPT_MODE` and `Cipher.DECRYPT_MODE`\.  | 
| DESede \(Triple DES\) | ECB |  `DESede/ECB/NoPadding` `DESede/ECB/PKCS5Padding`  | Implements `Cipher.ENCRYPT_MODE` and `Cipher.DECRYPT_MODE`\.  | 
| RSA | ECB | `RSA/ECB/PKCS1Padding` `RSA/ECB/OAEPPadding` `RSA/ECB/OAEPWithSHA-1ANDMGF1Padding` `RSA/ECB/OAEPWithSHA-224ANDMGF1Padding` `RSA/ECB/OAEPWithSHA-256ANDMGF1Padding` `RSA/ECB/OAEPWithSHA-384ANDMGF1Padding` `RSA/ECB/OAEPWithSHA-512ANDMGF1Padding`  |  Implements `Cipher.WRAP_MODE`, `Cipher.UNWRAP_MODE`, `Cipher.ENCRYPT_MODE`, and `Cipher.DECRYPT_MODE`\.  | 
| RSAAESWrap | ECB |  `RSAAESWrap/ECB/OAEPPadding` `RSAAESWrap/ECB/OAEPWithSHA-1ANDMGF1Padding` `RSAAESWrap/ECB/OAEPWithSHA-224ANDMGF1Padding` `RSAAESWrap/ECB/OAEPWithSHA-256ANDMGF1Padding` `RSAAESWrap/ECB/OAEPWithSHA-384ANDMGF1Padding` `RSAAESWrap/ECB/OAEPWithSHA-512ANDMGF1Padding`  | Implements `Cipher.WRAP_MODE` and `Cipher.UNWRAP_MODE`\.  | 

## Supported digests<a name="java-digests_5"></a>

The AWS CloudHSM software library for Java supports the following message digests\. With Client SDK 5, the data is hashed locally in software\. This means there is no limit on the size of the data that can be hashed by the SDK\.
+ `SHA-1`
+ `SHA-224`
+ `SHA-256`
+ `SHA-384`
+ `SHA-512`

## Supported hash\-based message authentication code \(HMAC\) algorithms<a name="java-mac_5"></a>

The AWS CloudHSM software library for Java supports the following HMAC algorithms\.
+ `HmacSHA1`
+ `HmacSHA224`
+ `HmacSHA256`
+ `HmacSHA384`
+ `HmacSHA512`

## Supported sign/verify mechanisms<a name="java-sign-verify_5"></a>

The AWS CloudHSM software library for Java supports the following types of signature and verification\. With Client SDK 5 and signature algorithms with hashing, the data is hashed locally in software before being sent to the HSM for the signature/verification\. This means there is no limit on the size of the data that can be hashed by the SDK\.

**RSA Signature Types**
+ `NONEwithRSA`
+ `RSASSA-PSS`
+ `SHA1withRSA`
+ `SHA1withRSA/PSS`
+ `SHA1withRSAandMGF1`
+ `SHA224withRSA`
+ `SHA224withRSAandMGF1`
+ `SHA224withRSA/PSS`
+ `SHA256withRSA`
+ `SHA256withRSAandMGF1`
+ `SHA256withRSA/PSS`
+ `SHA384withRSA`
+ `SHA384withRSAandMGF1`
+ `SHA384withRSA/PSS`
+ `SHA512withRSA`
+ `SHA512withRSAandMGF1`
+ `SHA512withRSA/PSS`

**ECDSA Signature Types**
+ `NONEwithECDSA`
+ `SHA1withECDSA`
+ `SHA224withECDSA`
+ `SHA256withECDSA`
+ `SHA384withECDSA`
+ `SHA512withECDSA`