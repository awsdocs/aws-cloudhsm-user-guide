# Supported mechanisms for Client SDK 5<a name="java-lib-supported_5"></a>

For information about the Java Cryptography Architecture \(JCA\) interfaces and engine classes supported by AWS CloudHSM, see the following topics\. 

**Topics**
+ [Supported ciphers](#java-ciphers_5)
+ [Supported sign/verify mechanisms](#java-sign-verify_5)
+ [Supported digests](#java-digests_5)
+ [Supported hash\-based message authentication code \(HMAC\) algorithms](#java-mac_5)
+ [Supported cipher\-based message authentication code \(CMAC\) algorithms](#java-cmac_5)
+ [Supported key factories](#java-key-factories)

## Supported ciphers<a name="java-ciphers_5"></a>

The AWS CloudHSM software library for Java supports the following algorithm, mode, and padding combinations\.


| Algorithm | Mode | Padding | Notes | 
| --- | --- | --- | --- | 
| AES | CBC |  `AES/CBC/NoPadding` `AES/CBC/PKCS5Padding`  |  Implements `Cipher.ENCRYPT_MODE` and `Cipher.DECRYPT_MODE`\. Implements `Cipher.UNWRAP_MODE for AES/CBC NoPadding`  | 
| AES | ECB |  `AES/ECB/PKCS5Padding` `AES/ECB/NoPadding`  | Implements `Cipher.ENCRYPT_MODE` and `Cipher.DECRYPT_MODE`\.  | 
| AES | CTR |  `AES/CTR/NoPadding`  |  Implements `Cipher.ENCRYPT_MODE` and `Cipher.DECRYPT_MODE`\.  | 
| AES | GCM | `AES/GCM/NoPadding` | Implements `Cipher.WRAP_MODE`, `Cipher.UNWRAP_MODE`, `Cipher.ENCRYPT_MODE`, and `Cipher.DECRYPT_MODE`\.When performing AES\-GCM encryption, the HSM ignores the initialization vector \(IV\) in the request and uses an IV that it generates\. When the operation completes, you must call `Cipher.getIV()` to get the IV\. | 
| AESWrap | ECB |  `AESWrap/ECB/NoPadding` `AESWrap/ECB/PKCS5Padding` `AESWrap/ECB/ZeroPadding`  | Implements `Cipher.WRAP_MODE` and `Cipher.UNWRAP_MODE`\.  | 
| DESede \(Triple DES\) | CBC |  `DESede/CBC/PKCS5Padding` `DESede/CBC/NoPadding`  |  Implements `Cipher.ENCRYPT_MODE` and `Cipher.DECRYPT_MODE`\.  | 
| DESede \(Triple DES\) | ECB |  `DESede/ECB/NoPadding` `DESede/ECB/PKCS5Padding`  | Implements `Cipher.ENCRYPT_MODE` and `Cipher.DECRYPT_MODE`\.  | 
| RSA | ECB | `RSA/ECB/PKCS1Padding` `RSA/ECB/OAEPPadding` `RSA/ECB/OAEPWithSHA-1ANDMGF1Padding` `RSA/ECB/OAEPWithSHA-224ANDMGF1Padding` `RSA/ECB/OAEPWithSHA-256ANDMGF1Padding` `RSA/ECB/OAEPWithSHA-384ANDMGF1Padding` `RSA/ECB/OAEPWithSHA-512ANDMGF1Padding`  |  Implements `Cipher.WRAP_MODE`, `Cipher.UNWRAP_MODE`, `Cipher.ENCRYPT_MODE`, and `Cipher.DECRYPT_MODE`\.  | 
| RSA | ECB | `RSA/ECB/NoPadding` |  Implements `Cipher.ENCRYPT_MODE` and `Cipher.DECRYPT_MODE`\.  | 
| RSAAESWrap | ECB |  `RSAAESWrap/ECB/OAEPPadding` `RSAAESWrap/ECB/OAEPWithSHA-1ANDMGF1Padding` `RSAAESWrap/ECB/OAEPWithSHA-224ANDMGF1Padding` `RSAAESWrap/ECB/OAEPWithSHA-256ANDMGF1Padding` `RSAAESWrap/ECB/OAEPWithSHA-384ANDMGF1Padding` `RSAAESWrap/ECB/OAEPWithSHA-512ANDMGF1Padding`  | Implements `Cipher.WRAP_MODE` and `Cipher.UNWRAP_MODE`\.  | 

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

## Supported cipher\-based message authentication code \(CMAC\) algorithms<a name="java-cmac_5"></a>

CMACs \(Cipher\-based message authentication codes\) create message authentication codes \(MACs\) using a block cipher and a secret key\. They differ from HMACs in that they use a block symmetric key method for the MACs rather than a hashing method\.

The AWS CloudHSM software library for Java supports the following CMAC algorithms\.
+ `AESCMAC`

## Supported key factories<a name="java-key-factories"></a>

You can use key factories to convert keys to key specifications\. AWS CloudHSM has two types of key factories for JCE:

**SecretKeyFactory:** Used to import or derive symmetric keys\. Using SecretKeyFactory, you can pass a supported Key or a supported KeySpec to import or derive symmetric keys into AWS CloudHSM\. Following are the supported specs for KeyFactory:
+ For SecretKeyFactory's `generateSecret` method following [KeySpec](https://docs.oracle.com/javase/8/docs/api/java/security/spec/KeySpec.html) classes are supported:
  + **KeyAttributesMap**can be used to import a key bytes with addtional attributes as a CloudHSM Key\. An example can be found here [here](https://github.com/aws-samples/aws-cloudhsm-jce-examples/blob/sdk5/src/main/java/com/amazonaws/cloudhsm/examples/KeyUtilitiesRunner.java)\.
  + **[SecretKeySpec](https://docs.oracle.com/javase/8/docs/api/javax/crypto/spec/SecretKeySpec.html)**can be used to import a symmetric key spec as a CloudHSM Key\.
  + **AesCmacKdfParameterSpec**can be used to derive symmetric keys using another CloudHSM AES Key\.

**Note**  
SecretKeyFactory's `translateKey` method takes any key that implements the [key](https://docs.oracle.com/javase/8/docs/api/java/security/Key.html) interface\.

**KeyFactory:** Used for importing asymmetric keys\. Using KeyFactory, you can pass a supported Key or supported KeySpec to import an asymmetric key into AWS CloudHSM\. For more information, refer to the following resources:
+ For KeyFactory's `generatePublic` method, following [KeySpec](https://docs.oracle.com/javase/8/docs/api/java/security/spec/KeySpec.html) classes are supported:
+ CloudHSM KeyAttributesMap for both RSA and EC KeyTypes, including:
  + CloudHSM KeyAttributesMap for both RSA and EC public KeyTypes\. An example can be found [here](https://github.com/aws-samples/aws-cloudhsm-jce-examples/blob/sdk5/src/main/java/com/amazonaws/cloudhsm/examples/KeyUtilitiesRunner.java)
  + [X509EncodedKeySpec](https://docs.oracle.com/javase/8/docs/api/java/security/spec/X509EncodedKeySpec.html) for both RSA and EC Public Key
  + [RSAPublicKeySpec](https://docs.oracle.com/javase/8/docs/api/java/security/spec/RSAPublicKeySpec.html) for RSA Public Key
  + [ECPublicKeySpec](https://docs.oracle.com/javase/8/docs/api/java/security/spec/ECPublicKeySpec.html) for EC Public Key
+ For KeyFactory's `generatePrivate` method, following [KeySpec](https://docs.oracle.com/javase/8/docs/api/java/security/spec/KeySpec.html) classes are supported:
+ CloudHSM KeyAttributesMap for both RSA and EC KeyTypes, including:
  + CloudHSM KeyAttributesMap for both RSA and EC public KeyTypes\. An example can be found [here](https://github.com/aws-samples/aws-cloudhsm-jce-examples/blob/sdk5/src/main/java/com/amazonaws/cloudhsm/examples/KeyUtilitiesRunner.java)
  + [PKCS8EncodedKeySpec](https://docs.oracle.com/javase/8/docs/api/java/security/spec/PKCS8EncodedKeySpec.html) for both EC and RSA Private Key
  + [RSAPrivateCrtKeySpec](https://docs.oracle.com/javase/8/docs/api/java/security/spec/RSAPrivateCrtKeySpec.html) for RSA Private Key
  + [ECPrivateKeySpec](https://docs.oracle.com/javase/8/docs/api/java/security/spec/ECPrivateKeySpec.html) for EC Private Key

For KeyFactory's `translateKey` method, it takes in any Key that implements the [Key Interface](https://docs.oracle.com/javase/8/docs/api/java/security/Key.html)\.