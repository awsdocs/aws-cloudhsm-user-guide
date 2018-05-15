# Supported PKCS \#11 Mechanisms<a name="pkcs11-mechanisms"></a>

The AWS CloudHSM software library for PKCS \#11 supports the following PKCS \#11 mechanisms\.

**Generate, Create, Import Keys**
+ `CKM_AES_KEY_GEN`
+ `CKM_DES3_KEY_GEN`
+ `CKM_EC_KEY_PAIR_GEN`
+ `CKM_GENERIC_SECRET_KEY_GEN`
+ `CKM_RSA_X9_31_KEY_PAIR_GEN`
**Note**  
This mechanism is functionally identical to the `CKM_RSA_PKCS_KEY_PAIR_GEN` mechanism, but offers stronger guarantees for `p` and `q` generation\. If you need the `CKM_RSA_PKCS_KEY_PAIR_GEN` mechanism, use `CKM_RSA_X9_31_KEY_PAIR_GEN`\.

**Sign/Verify**
+ `CKM_RSA_PKCS`
+ `CKM_RSA_PKCS_PSS`
+ `CKM_SHA256_RSA_PKCS`
+ `CKM_SHA224_RSA_PKCS`
+ `CKM_SHA384_RSA_PKCS`
+ `CKM_SHA512_RSA_PKCS`
+ `CKM_SHA1_RSA_PKCS_PSS`
+ `CKM_SHA256_RSA_PKCS_PSS`
+ `CKM_SHA224_RSA_PKCS_PSS`
+ `CKM_SHA384_RSA_PKCS_PSS`
+ `CKM_SHA512_RSA_PKCS_PSS`
+ `CKM_MD5_HMAC`
+ `CKM_SHA_1_HMAC`
+ `CKM_SHA224_HMAC`
+ `CKM_SHA256_HMAC`
+ `CKM_SHA384_HMAC`
+ `CKM_SHA512_HMAC`
+ `CKM_ECDSA`
+ `CKM_ECDSA_SHA1`
+ `CKM_ECDSA_SHA224`
+ `CKM_ECDSA_SHA256`
+ `CKM_ECDSA_SHA384`
+ `CKM_ECDSA_SHA512`

**Digest**
+ `CKM_SHA1`
+ `CKM_SHA224`
+ `CKM_SHA256`
+ `CKM_SHA384`
+ `CKM_SHA512`

**Encrypt/Decrypt**
+ `CKM_AES_CBC`
+ `CKM_AES_CBC_PAD`
+ `CKM_AES_GCM`
**Note**  
When performing AES\-GCM encryption, the HSM ignores the initialization vector \(IV\) in the request and uses an IV that it generates\. The HSM writes the generated IV to the memory reference pointed to by the `pIV` element of the `CK_GCM_PARAMS` parameters structure that you supply\.
+ `CKM_DES3_CBC`
+ `CKM_DES3_CBC_PAD`
+ `CKM_RSA_OAEP_PAD`
+ `CKM_RSA_PKCS`

**Key Derive**
+ `CKM_ECDH1_DERIVE`

**Key Wrap**
+ `CKM_AES_KEY_WRAP`