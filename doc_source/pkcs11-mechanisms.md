# Supported mechanisms<a name="pkcs11-mechanisms"></a>

The PKCS \#11 library is compliant with version 2\.40 of the PKCS \#11 specification\. To invoke a cryptographic feature using PKCS \#11, call a function with a given mechanism\. The following sections summarize the combinations of functions and mechanisms supported by AWS CloudHSM\.

The PKCS \#11 library supports the following algorithms:
+ **Encryption and decryption** – AES\-CBC, AES\-CTR, AES\-ECB, AES\-GCM, DES3\-CBC, DES3\-ECB, RSA\-OAEP, and RSA\-PKCS
+ **Sign and verify** – RSA, HMAC, and ECDSA; with and without hashing
+ **Hash/digest** – SHA1, SHA224, SHA256, SHA384, and SHA512
+ **Key wrap** – AES Key Wrap[1](#mech1), AES\-GCM, RSA\-AES, and RSA\-OAEP

**Topics**
+ [Generate Key and Key Pair functions](#pkcs11-mech-function-genkey)
+ [Sign and Verify functions](#pkcs11-mech-function-signverify)
+ [Sign Recover and Verify Recover](#pkcs11-mech-function-sr-vr)
+ [Digest](#pkcs11-mech-function-digest)
+ [Encrypt and Decrypt](#pkcs11-mech-function-enc-dec)
+ [Derive Key](#pkcs11-mech-function-derive-key)
+ [Wrap and Unwrap](#pkcs11-mech-function-wrap-unwrap)
+ [Mechanism annotations](#pkcs11-mech-annotations)

## Generate Key and Key Pair functions<a name="pkcs11-mech-function-genkey"></a>

The AWS CloudHSM software library for PKCS \#11 library allows you to use the following mechanisms for Generate Key and Key Pair functions\.
+ `CKM_RSA_PKCS_KEY_PAIR_GEN`
+ `CKM_RSA_X9_31_KEY_PAIR_GEN` – This mechanism is functionally identical to the `CKM_RSA_PKCS_KEY_PAIR_GEN` mechanism, but offers stronger guarantees for `p` and `q` generation\.
+ `CKM_EC_KEY_PAIR_GEN`
+ `CKM_GENERIC_SECRET_KEY_GEN`
+ `CKM_AES_KEY_GEN`
+ `CKM_DES3_KEY_GEN`

## Sign and Verify functions<a name="pkcs11-mech-function-signverify"></a>

The AWS CloudHSM software library for PKCS \#11 library allows you to use the following mechanisms for Sign and Verify functions\. With Client SDK 5, the data is hashed locally in software\. This means there is no limit on the size of the data that can be hashed by the SDK\.

With Client SDK 5 RSA and ECDSA hashing is done locally so there is no data limit\. With HMAC, there is a data limit\. See footnote [3](#mech3) for more info\.

**RSA**
+ `CKM_RSA_X_509`
+ `CKM_RSA_PKCS` – single\-part operations only\.
+ `CKM_RSA_PKCS_PSS` – single\-part operations only\.
+ `CKM_SHA1_RSA_PKCS`
+ `CKM_SHA224_RSA_PKCS`
+ `CKM_SHA256_RSA_PKCS`
+ `CKM_SHA384_RSA_PKCS`
+ `CKM_SHA512_RSA_PKCS`
+ `CKM_SHA512_RSA_PKCS`
+ `CKM_SHA1_RSA_PKCS_PSS`
+ `CKM_SHA224_RSA_PKCS_PSS`
+ `CKM_SHA256_RSA_PKCS_PSS`
+ `CKM_SHA384_RSA_PKCS_PSS`
+ `CKM_SHA512_RSA_PKCS_PSS`

**ECDSA**
+ `CKM_ECDSA` – single\-part operations only\.
+ `CKM_ECDSA_SHA1`
+ `CKM_ECDSA_SHA224`
+ `CKM_ECDSA_SHA256`
+ `CKM_ECDSA_SHA384`
+ `CKM_ECDSA_SHA512`

**HMAC**
+ `CKM_SHA_1_HMAC`[2](#mech2)
+ `CKM_SHA224_HMAC`[2](#mech2)
+ `CKM_SHA256_HMAC`[2](#mech2)
+ `CKM_SHA384_HMAC`[2](#mech2)
+ `CKM_SHA512_HMAC`[2](#mech2)

**CMAC**
+ `CKM_AES_CMAC`

## Sign Recover and Verify Recover<a name="pkcs11-mech-function-sr-vr"></a>

Client SDK 5 does not support Sign Recover and Verify Recover functions\.

## Digest<a name="pkcs11-mech-function-digest"></a>

The AWS CloudHSM software library for PKCS \#11 library allows you to use the following mechanisms for Digest functions\. With Client SDK 5, the data is hashed locally in software\. This means there is no limit on the size of the data that can be hashed by the SDK\.
+ `CKM_SHA_1`
+ `CKM_SHA224`
+ `CKM_SHA256`
+ `CKM_SHA384`
+ `CKM_SHA512`

## Encrypt and Decrypt<a name="pkcs11-mech-function-enc-dec"></a>

The AWS CloudHSM software library for PKCS \#11 library allows you to use the following mechanisms for Encrypt and Decrypt functions\.
+ `CKM_RSA_X_509`
+ `CKM_RSA_PKCS` – single\-part operations only\.
+ `CKM_RSA_PKCS_OAEP` – single\-part operations only\.
+ `CKM_AES_ECB`
+ `CKM_AES_CTR`
+ `CKM_AES_CBC`
+ `CKM_AES_CBC_PAD`
+ `CKM_DES3_CBC`
+ `CKM_DES3_CBC_PAD`
+  `CKM_AES_GCM` [1](#mech1), [2](#mech2)
+ `CKM_CLOUDHSM_AES_GCM`[3](#mech3)

## Derive Key<a name="pkcs11-mech-function-derive-key"></a>

The AWS CloudHSM software library for PKCS \#11 library allows you to use the following mechanisms for Derive functions\.
+ `CKM_SP800_108_COUNTER_KDF`

## Wrap and Unwrap<a name="pkcs11-mech-function-wrap-unwrap"></a>

The AWS CloudHSM software library for PKCS \#11 library allows you to use the following mechanisms for Wrap and Unwrap functions\.

For additional information regarding AES key wrapping, see [AES Key Wrapping](manage-aes-key-wrapping.md)\. 
+ `CKM_RSA_PKCS` – single\-part operations only\.
+ `CKM_RSA_PKCS_OAEP`[4](#mech4)
+ `CKM_AES_GCM`[1](#mech1), [3](#mech3)
+ `CKM_CLOUDHSM_AES_GCM`[3](#mech3)
+ `CKM_RSA_AES_KEY_WRAP`
+ `CKM_CLOUDHSM_AES_KEY_WRAP_NO_PAD`[3](#mech3)
+ `CKM_CLOUDHSM_AES_KEY_WRAP_PKCS5_PAD`[3](#mech3)
+ `CKM_CLOUDHSM_AES_KEY_WRAP_ZERO_PAD`[3](#mech3)

## Mechanism annotations<a name="pkcs11-mech-annotations"></a>
+ \[1\] When performing AES\-GCM encryption, the HSM does not accept initialization vector \(IV\) data from the application\. You must use an IV that it generates\. The 12\-byte IV provided by the HSM is written into the memory reference pointed to by the pIV element of the `CK_GCM_PARAMS` parameters structure that you supply\. To prevent user confusion, PKCS \#11 SDK in version 1\.1\.1 and later ensures that pIV points to a zeroized buffer when AES\-GCM encryption is initialized\.
+ \[2\] When operating on data by using any of the following mechanisms, if the data buffer exceeds the maximum data size, the operation results in an error\. For these mechanisms, all the data processing must occur inside the HSM\. The following table lists maximum data size set for each mechanism:  
**Maximum data set size**    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/pkcs11-mechanisms.html)
+ \[3\] Vendor\-defined mechanism\. In order to use the CloudHSM vendor defined mechanisms, PKCS\#11 applications must include `/opt/cloudhsm/include/pkcs11t.h` during compilation\.

  `CKM_CLOUDHSM_AES_GCM`: This proprietary mechanism is a programmatically safer alternative to the standard `CKM_AES_GCM`\. It prepends the IV generated by the HSM to the ciphertext instead of writing it back into the `CK_GCM_PARAMS` structure that is provided during cipher initialization\. You can use this mechanism with `C_Encrypt`, `C_WrapKey`, `C_Decrypt`, and `C_UnwrapKey` functions\. When using this mechanism, the pIV variable in the `CK_GCM_PARAMS` struct must be set to `NULL`\. When using this mechanism with `C_Decrypt` and `C_UnwrapKey`, the IV is expected to be prepended to the ciphertext that is being unwrapped\.

  `CKM_CLOUDHSM_AES_KEY_WRAP_PKCS5_PAD`: AES Key Wrap with PKCS \#5 Padding\.

  `CKM_CLOUDHSM_AES_KEY_WRAP_ZERO_PAD`: AES Key Wrap with Zero Padding\.
+ \[4\] The following `CK_MECHANISM_TYPE` and `CK_RSA_PKCS_MGF_TYPE` are supported as `CK_RSA_PKCS_OAEP_PARAMS` for `CKM_RSA_PKCS_OAEP`:
  + `CKM_SHA_1` using `CKG_MGF1_SHA1`
  + `CKM_SHA224` using `CKG_MGF1_SHA224`
  + `CKM_SHA256` using `CKG_MGF1_SHA256`
  + `CKM_SHA384` using `CKM_MGF1_SHA384`
  + `CKM_SHA512` using `CKM_MGF1_SHA512`