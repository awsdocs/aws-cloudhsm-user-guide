# Known issues for the PKCS \#11 library<a name="ki-pkcs11-sdk"></a>

**Topics**
+ [Issue: AES key wrap in version 3\.0\.0 of the PKCS \#11 library does not validate IVs before use](#ki-pkcs11-1)
+ [Issue: PKCS\#11 SDK 2\.0\.4 and earlier versions always used the default IV of `0xA6A6A6A6A6A6A6A6` for AES key wrap and unwrap](#ki-pkcs11-2)
+ [Issue: The `CKA_DERIVE` attribute was not supported and was not handled](#ki-pkcs11-3)
+ [Issue: The `CKA_SENSITIVE` attribute was not supported and was not handled](#ki-pkcs11-4)
+ [Issue: Multipart hashing and signing are not supported](#ki-pkcs11-5)
+ [Issue: `C_GenerateKeyPair` does not handle `CKA_MODULUS_BITS` or `CKA_PUBLIC_EXPONENT` in the private template in a manner that is compliant with standards](#ki-pkcs11-6)
+ [Issue: You could not hash more than 16KB of data](#ki-pkcs11-7)
+ [Issue: Buffers for the `C_Encrypt` and `C_Decrypt` API operations cannot exceed 16 KB when using the `CKM_AES_GCM` mechanism](#ki-pkcs11-8)
+ [Issue: Elliptic\-curve Diffie\-Hellman \(ECDH\) key derivation is executed partially within the HSM](#ki-pkcs11-9)
+ [Issue: Verification of secp256k1 signatures fails on EL6 platforms such as CentOS6 and RHEL6](#ki-pkcs11-10)
+ [Issue: Incorrect sequence of function calls gives undefined results instead of failing](#ki-pkcs11-11)
+ [Issue: Read Only Session is not supported in SDK 5](#ki-pkcs11-13)
+ [Issue: `cryptoki.h` header file is Windows\-only](#ki-pkcs11-14)

## Issue: AES key wrap in version 3\.0\.0 of the PKCS \#11 library does not validate IVs before use<a name="ki-pkcs11-1"></a>

If you specify an IV shorter than 8 bytes in length, it is padded with unpredictable bytes before use\. 

**Note**  
This impacts `C_WrapKey` with `CKM_AES_KEY_WRAP` mechanism only\.
+ **Impact:** If you provide an IV that is shorter than 8 bytes in version 3\.0\.0 of PKCS \#11 library, you may be unable to unwrap the key\. 
+ **Workarounds: **
  + We strongly recommend you upgrade to version 3\.0\.1 or higher of the PKCS \#11 library, which properly enforces IV length during AES key wrap\. Amend your wrapping code to pass a NULL IV, or specify the default IV of `0xA6A6A6A6A6A6A6A6`\. For more information, see [Custom IVs with Non\-Compliant Length for AES Key Wrap](troubleshooting-aes-keys.md)\.
  + If you wrapped any keys with version 3\.0\.0 of the PKCS \#11 library using an IV shorter than 8 bytes, reach out to us for [support](https://aws.amazon.com/support)\.
+ **Resolution status:** This issue has been resolved in version 3\.0\.1 of the PKCS \#11 library\. To wrap keys using AES key wrap, specify an IV that is NULL or 8 bytes long\.

## Issue: PKCS\#11 SDK 2\.0\.4 and earlier versions always used the default IV of `0xA6A6A6A6A6A6A6A6` for AES key wrap and unwrap<a name="ki-pkcs11-2"></a>

User\-provided IVs were silently ignored\.

**Note**  
This impacts `C_WrapKey` with `CKM_AES_KEY_WRAP` mechanism only\.
+ **Impact:** 
  + If you used PKCS\#11 SDK 2\.0\.4 or an earlier version and a user\-provided IV, your keys are wrapped with the default IV of `0xA6A6A6A6A6A6A6A6`\.
  + If you used PKCS\#11 SDK 3\.0\.0 or later and a user\-provided IV, your keys are wrapped with the user\-provided IV\.
+ **Workarounds:**
  + To unwrap keys wrapped with PKCS\#11 SDK 2\.0\.4 or earlier use the default IV of `0xA6A6A6A6A6A6A6A6`\. 
  + To unwrap keys wrapped with PKCS\#11 SDK 3\.0\.0 or later, use the user\-provided IV\.
+ **Resolution status:** We strongly recommend that you amend your wrapping and unwrapping code to pass a NULL IV, or specify the default IV of `0xA6A6A6A6A6A6A6A6`\.

## Issue: The `CKA_DERIVE` attribute was not supported and was not handled<a name="ki-pkcs11-3"></a>
+ **Resolution status: **We have implemented fixes to accept `CKA_DERIVE` if it is set to `FALSE`\. `CKA_DERIVE` set to `TRUE` will not be supported until we begin to add key derivation function support to AWS CloudHSM\. You must update your client and SDK\(s\) to version 1\.1\.1 or higher to benefit from the fix\.

## Issue: The `CKA_SENSITIVE` attribute was not supported and was not handled<a name="ki-pkcs11-4"></a>
+ **Resolution status: **We have implemented fixes to accept and properly honor the `CKA_SENSITIVE` attribute\. You must update your client and SDK\(s\) to version 1\.1\.1 or higher to benefit from the fix\.

## Issue: Multipart hashing and signing are not supported<a name="ki-pkcs11-5"></a>
+ **Impact: **`C_DigestUpdate` and `C_DigestFinal` are not implemented\. `C_SignFinal` is also not implemented and will fail with `CKR_ARGUMENTS_BAD` for a non\-`NULL` buffer\. 
+ **Workaround: **Hash your data within your application and use AWS CloudHSM only for signing the hash\. 
+ **Resolution status: **We are fixing the client and the SDKs to correctly implement multipart hashing\. Updates will be announced in the AWS CloudHSM forum and on the version history page\.

## Issue: `C_GenerateKeyPair` does not handle `CKA_MODULUS_BITS` or `CKA_PUBLIC_EXPONENT` in the private template in a manner that is compliant with standards<a name="ki-pkcs11-6"></a>
+ **Impact: **`C_GenerateKeyPair` should return `CKA_TEMPLATE_INCONSISTENT` when the private template contains `CKA_MODULUS_BITS` or `CKA_PUBLIC_EXPONENT`\. It instead generates a private key for which all usage fields are set to `FALSE`\. The key cannot be used\. 
+ **Workaround: **We recommend that your application check the usage field values in addition to the error code\.
+ **Resolution status: **We are implementing fixes to return the proper error message when an incorrect private key template is used\. The updated PKCS \#11 library will be announced on the version history page\. 

## Issue: You could not hash more than 16KB of data<a name="ki-pkcs11-7"></a>

 For larger buffers, only the first 16KB will be hashed and returned\. The excess data would have been silently ignored\.
+ **Resolution status: **Data less than 16KB in size continues to be sent to the HSM for hashing\. We have added capability to hash locally, in software, data between 16KB and 64KB in size\. The client and the SDKs will explicitly fail if the data buffer is larger than 64KB\. You must update your client and SDK\(s\) to version 1\.1\.1 or higher to benefit from the fix\.

## Issue: Buffers for the `C_Encrypt` and `C_Decrypt` API operations cannot exceed 16 KB when using the `CKM_AES_GCM` mechanism<a name="ki-pkcs11-8"></a>

AWS CloudHSM does not support multipart AES\-GCM encryption\.
+ **Impact: **You cannot use the `CKM_AES_GCM` mechanism to encrypt data larger than 16 KB\.
+ **Workaround: ** You can use an alternative mechanism such as `CKM_AES_CBC` or you can divide your data into pieces and encrypt each piece individually\. You must manage the division of your data and subsequent encryption\. AWS CloudHSM does not perform multipart AES\-GCM encryption for you\. Note that FIPS requires that the initialization vector \(IV\) for AES\-GCM be generated on the HSM\. Therefore, the IV for each piece of your AES\-GCM encrypted data will be different\. 
+ **Resolution status: **We are fixing the SDK to fail explicitly if the data buffer is too large\. We return `CKR_MECHANISM_INVALID` for the `C_EncryptUpdate` and `C_DecryptUpdate` API operations\. We are evaluating alternatives to support larger buffers without relying on multipart encryption\. Updates will be announced in the AWS CloudHSM forum and on the version history page\.

## Issue: Elliptic\-curve Diffie\-Hellman \(ECDH\) key derivation is executed partially within the HSM<a name="ki-pkcs11-9"></a>

Your EC private key remains within the HSM at all times, but the key derivation process is performed in multiple steps\. As a result, intermediate results from each step are available on the client\.
+ **Impact: **The key derived using the `CKM_ECDH1_DERIVE` mechanism is first available on the client and is then imported into the HSM\. A key handle is then returned to your application\.
+ **Workaround: **If you are implementing SSL/TLS Offload in AWS CloudHSM, this limitation may not be an issue\. If your application requires your key to remain within an FIPS boundary at all times, consider using an alternative protocol that does not rely on ECDH key derivation\.
+ **Resolution status: **We are developing the option to perform ECDH key derivation entirely within the HSM\. The updated implementation will be announced on the version history page once available\.

## Issue: Verification of secp256k1 signatures fails on EL6 platforms such as CentOS6 and RHEL6<a name="ki-pkcs11-10"></a>

 This happens because the CloudHSM PKCS\#11 library avoids a network call during initialization of the verification operation by using OpenSSL to verify EC curve data\. Since Secp256k1 is not supported by the default OpenSSL package on EL6 platforms, the initialization fails\.
+ **Impact: **Secp256k1 signature verification will fail on EL6 platforms\. The verify call will fail with a `CKR_HOST_MEMORY` error\.
+ **Workaround: **We recommend using either Amazon Linux 1 or any EL7 platform if your PKCS\#11 application needs to verify secp256k1 signatures\. Alternatively, upgrade to a version of the OpenSSL package that supports the secp256k1 curve\.
+ **Resolution status: **We are implementing fixes to fall back to the HSM if local curve validation is not available\. The updated PKCS\#11 library will be announced on the [version history](client-history.md) page\.

## Issue: Incorrect sequence of function calls gives undefined results instead of failing<a name="ki-pkcs11-11"></a>
+ **Impact**: If you call an incorrect sequence of functions, the final result is incorrect even though the individual function calls return success\. For instance, decrypted data may not match the original plaintext or signatures may fail to verify\. This issue affects both single part and multi\-part operations\.

  Examples of incorrect function sequences:
  + `C_EncryptInit`/`C_EncryptUpdate` followed by `C_Encrypt`
  + `C_DecryptInit`/`C_DecryptUpdate` followed by `C_Decrypt`
  + `C_SignInit`/`C_SignUpdate` followed by `C_Sign`
  + `C_VerifyInit`/`C_VerifyUpdate` followed by `C_Verify`
  + `C_FindObjectsInit` followed by `C_FindObjectsInit`
+  **Workaround**: Your application should, in compliance with the PKCS \#11 specification, use the right sequence of function calls for both single and multi\-part operations\. Your application should not rely on the CloudHSM PKCS \#11 library to return an error under this circumstance\. 

## Issue: Read Only Session is not supported in SDK 5<a name="ki-pkcs11-13"></a>
+ **Issue: **SDK 5 does not support opening Read\-Only sessions with `C_OpenSession`\.
+ **Impact: **If you attempt to call `C_OpenSession` without providing `CKF_RW_SESSION`, the call will fail with the error `CKR_FUNCTION_FAILED`\. 
+ **Workaround: **When opening a session, you must pass the `CKF_SERIAL_SESSION | CKF_RW_SESSION` flags to the `C_OpenSession` function call\. 

## Issue: `cryptoki.h` header file is Windows\-only<a name="ki-pkcs11-14"></a>
+ **Issue: **With AWS CloudHSM Client SDK 5 versions 5\.0\.0 through 5\.4\.0 on Linux, the header file `/opt/cloudhsm/include/pkcs11/cryptoki.h` is only compatible with Windows operating systems\.
+ **Impact: **You may encounter issues when trying to include this header file in your application on Linux\-based operating systems\.
+ **Resolution status: **Upgrade to AWS CloudHSM Client SDK 5 version 5\.4\.1 or above, which includes a Linux\-compatible version of this header file\.