# AWS CloudHSM Software Library for PKCS \#11<a name="pkcs11-library"></a>

The AWS CloudHSM software library for PKCS \#11 is a PKCS \#11 standard implementation that communicates with the HSMs in your AWS CloudHSM cluster\. The library supports PKCS \#11 version 2\.40, including the following key types, mechanisms, and APIs\.


+ [Supported PKCS \#11 Key Types](#pkcs11-key-types)
+ [Supported PKCS \#11 Mechanisms](#pkcs11-mechanisms)
+ [Supported PKCS \#11 APIs](#pkcs11-apis)
+ [Install and Use the AWS CloudHSM Software Library for PKCS \#11](pkcs11-library-install.md)

## Supported PKCS \#11 Key Types<a name="pkcs11-key-types"></a>

The AWS CloudHSM software library for PKCS \#11 supports the following key types\.

+ **RSA** – 2048\-bit to 4096\-bit RSA keys, in increments of 256 bits\.

+ **ECDSA** – Generate keys with the P\-224, P\-256, P\-384, and P\-521 curves\. Only the P\-256 and P\-384 curves are supported for sign/verify\.

+ **AES** – 128, 192, and 256\-bit AES keys\.

+ **Triple DES \(3DES\)** – 192\-bit keys\.

+ **GENERIC\_SECRET** – 1 to 64 bytes\.

## Supported PKCS \#11 Mechanisms<a name="pkcs11-mechanisms"></a>

The AWS CloudHSM software library for PKCS \#11 supports the following PKCS \#11 mechanisms\.

**Generate, Create, Import Keys**

+ `CKM_RSA_X9_31_KEY_PAIR_GEN`
**Note**  
This mechanism is functionally identical to the `CKM_RSA_PKCS_KEY_PAIR_GEN` mechanism, but offers stronger guarantees for `p` and `q` generation\. If you need the `CKM_RSA_PKCS_KEY_PAIR_GEN` mechanism, you can use `CKM_RSA_X9_31_KEY_PAIR_GEN` instead\.

+ `CKM_EC_KEY_PAIR_GEN`

+ `CKM_AES_KEY_GEN`

+ `CKM_DES3_KEY_GEN`

+ `CKM_GENERIC_SECRET_KEY_GEN`

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

+ `CKM_ECDSA_SHA1`

**Digest**

+ `CKM_SHA1`

+ `CKM_SHA224`

+ `CKM_SHA256`

+ `CKM_SHA384`

+ `CKM_SHA512`

**Encrypt/Decrypt**

+ `CKM_DES3_CBC`

+ `CKM_DES3_CBC_PAD`

+ `CKM_AES_CBC`

+ `CKM_AES_CBC_PAD`

+ `CKM_AES_GCM`

+ `CKM_RSA_PKCS`

+ `CKM_RSA_OAEP_PAD`

**Key Derive**

+ `CKM_ECDH1_DERIVE`

**Key Wrap**

+ `CKM_AES_KEY_WRAP`

## Supported PKCS \#11 APIs<a name="pkcs11-apis"></a>

The AWS CloudHSM software library for PKCS \#11 supports the following PKCS \#11 APIs\.

+ `C_Initialize`

+ `C_Finalize`

+ `C_GetFunctionList`

+ `C_OpenSession`

+ `C_GetSessionInfo`

+ `C_Login`

+ `C_Logout`

+ `C_GetInfo`

+ `C_GetTokenInfo`

+ `C_GenerateKey`

+ `C_GenerateKeyPair`

+ `C_GetAttributeValue`

+ `C_EncryptInit`

+ `C_Encrypt`

+ `C_EncryptUpdate`

+ `C_EncryptFinal`

+ `C_DecryptInit`

+ `C_Decrypt`

+ `C_DecryptUpdate`

+ `C_DecryptFinal`

+ `C_DigestInit`

+ `C_Digest`

+ `C_SignInit`

+ `C_Sign`

+ `C_SignUpdate`

+ `C_SignFinal`

+ `C_SignRecoverInit`

+ `C_SignRecover`

+ `C_VerifyInit`

+ `C_Verify`

+ `C_VerifyUpdate`

+ `C_VerifyFinal`

+ `C_VerifyRecoverInit`

+ `C_VerifyRecover`

+ `C_GenerateRandom`

+ `C_WrapKey`

+ `C_UnWrapKey`

+ `C_DestroyObject`

+ `C_GetSlotList`

+ `C_GetSlotInfo`

+ `C_GetMechanismInfo`

+ `C_GetMechanismList`

+ `C_GetOperationState`

+ `C_CreateObject`

+ `C_FindObjectsInit`

+ `C_FindObjects`

+ `C_FindObjectsFinal`