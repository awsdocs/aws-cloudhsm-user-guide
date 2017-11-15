# AWS CloudHSM Software Library for Java<a name="java-library"></a>

The AWS CloudHSM software library for Java is a provider implementation for the Sun Java JCE \(Java Cryptography Extension\) provider framework\. It includes implementations for interfaces and engine classes in the JCA \(Java Cryptography Architecture\) standard\. For more information about the supported provider classes and interfaces, see the following topics\.


+ [Supported Keys](#java-keys)
+ [Supported Ciphers](#java-ciphers)
+ [Supported Digests](#java-digests)
+ [Supported Hash\-Based Message Authentication Code \(HMAC\) Algorithms](#java-mac)
+ [Supported Sign/Verify Mechanisms](#java-sign-verify)
+ [Install and Use the AWS CloudHSM Software Library for Java](java-library-install.md)
+ [Example Code for the AWS CloudHSM Software Library for Java](java-library-sample.md)

## Supported Keys<a name="java-keys"></a>

The AWS CloudHSM software library for Java supports the following key types\.

+ **RSA** – 2048\-bit to 4096\-bit RSA keys, in increments of 256 bits\.

+ **AES** – 128, 192, and 256\-bit AES keys\.

## Supported Ciphers<a name="java-ciphers"></a>

The AWS CloudHSM software library for Java supports the following algorithm, mode, and padding combinations\.


| Algorithm | Mode | Padding | Notes | 
| --- | --- | --- | --- | 
| AES | CBC |  `AES/CBC/NoPadding` `AES/CBC/PKCS5Padding`  | Implements Cipher\.ENCRYPT\_MODE, Cipher\.DECRYPT\_MODE, Cipher\.WRAP\_MODE, and Cipher\.UNWRAP\_MODE\. | 
| AES | GCM | AES/GCM/NoPadding | Implements Cipher\.ENCRYPT\_MODE and Cipher\.DECRYPT\_MODE\. | 
| RSA | ECB |  `RSA/ECB/PKCS1Padding` `RSA/ECB/OAEPPadding` `RSA/ECB/OAEPWithSHA-224ANDMGF1Padding` `RSA/ECB/OAEPWithSHA-256ANDMGF1Padding` `RSA/ECB/OAEPWithSHA-384ANDMGF1Padding` `RSA/ECB/OAEPWithSHA-512ANDMGF1Padding`  |  Implements `Cipher.ENCRYPT_MODE` and `Cipher.DECRYPT_MODE`\. `OAEPPadding` is `OAEP` with the `SHA-1` padding type\.  | 

## Supported Digests<a name="java-digests"></a>

The AWS CloudHSM software library for Java supports the following message digests\.

+ `MD5`

+ `SHA-1`

+ `SHA-224`

+ `SHA-256`

+ `SHA-384`

+ `SHA-512`

## Supported Hash\-Based Message Authentication Code \(HMAC\) Algorithms<a name="java-mac"></a>

The AWS CloudHSM software library for Java supports the following HMAC algorithms\.

+ `HmacSHA1`

+ `HmacSHA224`

+ `HmacSHA256`

+ `HmacSHA384`

+ `HmacSHA512`

## Supported Sign/Verify Mechanisms<a name="java-sign-verify"></a>

The AWS CloudHSM software library for Java supports the following types of RSA signature and verification\.

**PKCS \#1 version 1\.5**

+ `SHA1withRSA`

+ `SHA224withRSA`

+ `SHA256withRSA`

+ `SHA384withRSA`

+ `SHA512withRSA`

**PKCS \#1 version 2\.2**

+ `SHA1withRSA/PSS`

+ `SHA224withRSA/PSS`

+ `SHA256withRSA/PSS`

+ `SHA384withRSA/PSS`

+ `SHA512withRSA/PSS`