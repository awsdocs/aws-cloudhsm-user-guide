# Code samples for the PKCS \#11 library<a name="pkcs11-samples"></a>

The code samples on GitHub show you how to accomplish basic tasks using the PKCS \#11 library\. 

## Sample code prerequisites<a name="pkcs11-samples-prereqs"></a>

Before running the samples, perform the following steps to set up your environment:
+ Install and configure the [PKCS \#11 library](pkcs11-library-install.md) for Client SDK 5\.
+ Set up a [cryptographic user \(CU\)](manage-hsm-users.md)\. Your application uses this HSM account to run the code samples on the HSM\.

## Code samples<a name="pkcs11-samples-code"></a>

Code Samples for the AWS CloudHSM Software Library for PKCS\#11 are available on [GitHub](https://github.com/aws-samples/aws-cloudhsm-pkcs11-examples)\. This repository includes examples on how to do common operations using PKCS\#11 including encryption, decryption, signing and verifying\.
+ [Generate keys \(AES, RSA, EC\)](https://github.com/aws-samples/aws-cloudhsm-pkcs11-examples/tree/master/src/generate)
+ [List key attributes](https://github.com/aws-samples/aws-cloudhsm-pkcs11-examples/blob/master/src/attributes/)
+ [Encrypt and decrypt data with AES GCM](https://github.com/aws-samples/aws-cloudhsm-pkcs11-examples/blob/master/src/encrypt/aes_gcm.c)
+ [Encrypt and decrypt data with AES\_CTR](https://github.com/aws-samples/aws-cloudhsm-pkcs11-examples/tree/master/src/encrypt/aes_ctr.c) 
+ [Encrypt and decrypt data with 3DES](https://github.com/aws-samples/aws-cloudhsm-pkcs11-examples/tree/master/src/encrypt/des_ecb.c) 
+ [Sign and verify data with RSA](https://github.com/aws-samples/aws-cloudhsm-pkcs11-examples/blob/master/src/sign/rsa_sign.c)
+ [Derive keys using HMAC KDF](https://github.com/aws-samples/aws-cloudhsm-pkcs11-examples/blob/master/src/derivation/hmac_kdf.c)
+ [Wrap and unwrap keys with AES using PKCS \#5 padding](https://github.com/aws-samples/aws-cloudhsm-pkcs11-examples/blob/master/src/wrapping/aes_wrapping.c)
+ [Wrap and unwrap keys with AES using no padding](https://github.com/aws-samples/aws-cloudhsm-pkcs11-examples/blob/master/src/wrapping/aes_no_padding_wrapping.c)
+ [Wrap and unwrap keys with AES using zero padding](https://github.com/aws-samples/aws-cloudhsm-pkcs11-examples/blob/master/src/wrapping/aes_zero_padding_wrapping.c)
+ [Wrap and unwrap keys with AES\-GCM](https://github.com/aws-samples/aws-cloudhsm-pkcs11-examples/tree/master/src/wrapping/aes_gcm_wrapping.c)
+ [Wrap and unwrap keys with RSA](https://github.com/aws-samples/aws-cloudhsm-pkcs11-examples/blob/master/src/wrapping/rsa_wrapping.c)