# Known Issues<a name="KnownIssues"></a>

The following issues are currently known for AWS CloudHSM\.

**Topics**
+ [Known Issues for all HSM instances](#ki-all)
+ [Known Issues for Amazon EC2 Instances Running Amazon Linux 2](#ki-al2)
+ [Known Issues for the PKCS \#11 SDK](#ki-pkcs11-sdk)
+ [Known Issues for the JCE SDK](#ki-jce-sdk)
+ [Known Issues for the OpenSSL SDK](#ki-openssl-sdk)

## Known Issues for all HSM instances<a name="ki-all"></a>

The following issues impact all AWS CloudHSM users regardless of whether they use the key\_mgmt\_util command line tool, the PKCS \#11 SDK, the JCE SDK, or the OpenSSL SDK\. 

**Issue: **AES key wrapping uses PKCS\#5 padding instead of providing a standards\-compliant implementation of key wrap with zero padding\. Additionally, key wrap with no padding is not supported\.
+ **Impact: **There is no impact if you wrap and unwrap within AWS CloudHSM\. Keys wrapped with AWS CloudHSM cannot be unwrapped within other HSMs or software that expects compliance to the no\-padding specification\. This is because 8 bytes of padding data might be suffixed to your key data following a standards\-compliant unwrap\. Externally wrapped keys cannot be properly unwrapped into an AWS CloudHSM instance\. 
+ **Workaround: **To externally unwrap a key that was wrapped with AES Key Wrapping on a AWS CloudHSM instance, strip the extra padding before you attemp to use the key\. You can do this by trimming the extra bytes in a file editor or copying only the key bytes into a new buffer in your code\. 
+ **Resolution status: **We are fixing the client and SDKs to provide SP800\-38F compliant AES key wrapping\. Updates will be announced in the AWS CloudHSM forum and on the version history page\. The update will include mechanisms to assist you in rewrapping any existing wrapped keys in a standards\-compliant way\. 

**Issue: **The client daemon requires at least one valid IP address in its configuration file to successfully connect to the cluster\. 
+ **Impact: **If you delete every HSM in your cluster and then add another HSM, which gets a new IP address, the client daemon continues to search for your HSMs at their original IP addresses\. 
+ **Workaround: **If you run an intermittent workload, we recommend that you use the `IpAddress` argument in the [CreateHsm](https://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_CreateHsm.html) function to set the elastic network interface \(ENI\) to its original value\. Note than an ENI is specific to an Availability Zone \(AZ\)\. The alternative is to delete the `/opt/cloudhsm/daemon/1/cluster.info` file and then reset the client configuration to the IP address of your new HSM\. You can use the `client -a <IP address>` command\. For more information, For more information, see [Install and Configure the AWS CloudHSM Client \(Linux\)](install-and-configure-client-linux.md) or [Install and Configure the AWS CloudHSM Client \(Windows\)](install-and-configure-client-win.md)\.

**Issue: **There was an upper limit of 16 KB on data that can be hashed and signed by AWS CloudHSM\. 
+ **Resolution status: **Data less than 16KB in size continues to be sent to the HSM for hashing\. We have added capability to hash locally, in software, data between 16KB and 64KB in size\. The client and the SDKs will explicitly fail if the data buffer is larger than 64KB\. You must update your client and SDK\(s\) to version 1\.1\.1 or higher to benefit from the fix\. 

**Issue: **Imported keys could not be specified as nonexportable\.
+ **Resolution Status: **This issue is fixed\. No action is required on your part to benefit from the fix\.

**Issue: **If you have a single HSM in your cluster, HSM failover does not work correctly\.
+ **Impact: **If the single HSM instance in your cluster loses connectivity, the client will not reconnect with it even if the HSM instance is later restored\.
+ **Workaround: **We recommend at least two HSM instances in any production cluster\. If you use this configuration, you will not be impacted by this issue\. For single\-HSM clusters, bounce the client daemon to restore connectivity\.
+ **Resolution status: ** This issue has been resolved in the [AWS CloudHSM client 1\.1\.2](client-history.md#client-version-1-1-2) release\. You must upgrade to this client to benefit from the fix\.

**Issue: **If you exceed the key capacity of the HSMs in your cluster within a short period of time, the client enters an unhandled error state\.
+ **Impact: ** When the client encounters the unhandled error state, it freezes and must be restarted\.
+ **Workaround: **Test your throughput to ensure you are not creating session keys at a rate that the client is unable to handle\. You can lower your rate by adding an HSM to the cluster or slowing down the session key creation\.
+ **Resolution status: ** This issue has been resolved in the [AWS CloudHSM client 1\.1\.2](client-history.md#client-version-1-1-2) release\. You must upgrade to this client to benefit from the fix\.

**Issue: **Digest operations with HMAC keys of size greater than 800 bytes are not supported\.
+ **Impact: **HMAC keys larger than 800 bytes can be generated on or imported into the HSM\. However, if you use this larger key in a digest operation via the JCE or key\_mgmt\_util, the operation will fail\. Note that if you are using PKCS11, HMAC keys are limited to a size of 64 bytes\.
+ **Workaround: **If you will be using HMAC keys for digest operations on the HSM, ensure the size is smaller than 800 bytes\.
+ **Resolution status: **None at this time\.

## Known Issues for Amazon EC2 Instances Running Amazon Linux 2<a name="ki-al2"></a>

**Issue: **Amazon Linux 2 version 2018\.07 uses an updated `ncurses` package \(version 6\) that is currently incompatible with the AWS CloudHSM SDKs\. The following error will be returned upon running the AWS CloudHSM [cloudhsm\_mgmt\_util](cloudhsm_mgmt_util.md) or [key\_mgmt\_util](key_mgmt_util.md):

```
/opt/cloudhsm/bin/cloudhsm_mgmt_util: error while loading shared libraries: libncurses.so.5: cannot open shared object file: No such file or directory
```
+ **Impact: **Instances running on Amazon Linux 2 version 2018\.07 will be unable to use *all* AWS CloudHSM utilities\. 
+ **Workaround: ** Issue the following command on your Amazon Linux 2 EC2 instances to install the supported `ncurses` package \(version 5\):

  ```
  sudo yum install ncurses-compat-libs
  ```
+ **Resolution status:** This issue has been resolved in the [AWS CloudHSM client 1\.1\.2](client-history.md#client-version-1-1-2) release\. You must upgrade to this client to benefit from the fix\.

## Known Issues for the PKCS \#11 SDK<a name="ki-pkcs11-sdk"></a>

**Issue: **PKCS \#11–compliant error messages are not directly available from the HSM instance\.
+ **Impact: **The PKCS \#11 library makes two round trips to the HSM per call: The first verifies whether the requested operation is permitted\. The second executes the operation if it is permitted\. This results in slower performance\. 
+ **Workaround: **We provide an alternative PKCS \#11 library with a local Redis cache so permissions for the requested operation can be checked locally\. For details, including information about the limitations of this solution, see [Install the PKCS \#11 Library with Redis \(Optional\)](pkcs11-library-install.md#install-pkcs11-redis)\.
+ **Resolution status: **We are implementing fixes to directly provide PKCS \#11–compliant error messages, removing the need for the Redis workaround\. The updated PKCS \#11 library will be announced on the version history page\.

**Issue: **The `CKA_DERIVE` attribute was not supported and was not handled\.
+ **Resolution status: **We have implemented fixes to accept `CKA_DERIVE` if it is set to `FALSE`\. `CKA_DERIVE` set to `TRUE` will not be supported until we begin to add key derivation function support to AWS CloudHSM\. You must update your client and SDK\(s\) to version 1\.1\.1 or higher to benefit from the fix\.

**Issue:** The `CKA_SENSITIVE` attribute was not supported and was not handled\.
+ **Resolution status: **We have implemented fixes to accept and properly honor the `CKA_SENSITIVE` attribute\. You must update your client and SDK\(s\) to version 1\.1\.1 or higher to benefit from the fix\.

**Issue: **Multipart hashing and signing are not supported\.
+ **Impact: **`C_DigestUpdate` and `C_DigestFinal` are not implemented\. `C_SignFinal` is also not implemented and will fail with `CKR_ARGUMENTS_BAD` for a non\-`NULL` buffer\. 
+ **Workaround: **Hash your data within your application and use AWS CloudHSM only for signing the hash\. 
+ **Resolution status: **We are fixing the client and the SDKs to correctly implement multipart hashing\. Updates will be announced in the AWS CloudHSM forum and on the version history page\.

**Issue: **`C_GenerateKeyPair` does not handle `CKA_MODULUS_BITS` or `CKA_PUBLIC_EXPONENT` in the private template in a manner that is compliant with standards\.
+ **Impact: **`C_GenerateKeyPair` should return `CKA_TEMPLATE_INCONSISTENT` when the private template contains `CKA_MODULUS_BITS` or `CKA_PUBLIC_EXPONENT`\. It instead generates a private key for which all usage fields are set to `FALSE`\. The key cannot be used\. 
+ **Workaround: **We recommend that your application check the usage field values in addition to the error code\.
+ **Resolution status: **We are implementing fixes to return the proper error message when an incorrect private key template is used\. The updated PKCS \#11 library will be announced on the version history page\. 

**Issue: **You could not hash more than 16KB of data\. For larger buffers, only the first 16KB will be hashed and returned\. The excess data would have been silently ignored\.
+ **Resolution status: **Data less than 16KB in size continues to be sent to the HSM for hashing\. We have added capability to hash locally, in software, data between 16KB and 64KB in size\. The client and the SDKs will explicitly fail if the data buffer is larger than 64KB\. You must update your client and SDK\(s\) to version 1\.1\.1 or higher to benefit from the fix\.

**Issue: **Buffers for the `C_Encrypt` and `C_Decrypt` API operations cannot exceed 16 KB when using the `CKM_AES_GCM` mechanism\. Also, AWS CloudHSM does not support multipart AES\-GCM encryption\.
+ **Impact: **You cannot use the `CKM_AES_GCM` mechanism to encrypt data larger than 16 KB\.
+ **Workaround: ** You can use an alternative mechanism such as `CKM_AES_CBC` or you can divide your data into pieces and encrypt each piece individually\. You must manage the division of your data and subsequent encryption\. AWS CloudHSM does not perform multipart AES\-GCM encryption for you\. Note that FIPS requires that the initialization vector \(IV\) for AES\-GCM be generated on the HSM\. Therefore, the IV for each piece of your AES\-GCM encrypted data will be different\. 
+ **Resolution status: **We are fixing the SDK to fail explicitly if the data buffer is too large\. We return `CKR_MECHANISM_INVALID` for the `C_EncryptUpdate` and `C_DecryptUpdate` API operations\. We are evaluating alternatives to support larger buffers without relying on multipart encryption\. Updates will be announced in the AWS CloudHSM forum and on the version history page\.

**Issue: **ECDH key derivation is executed partially within the HSM\. Your EC private key remains within the HSM at all times, but the key derivation process is performed in multiple steps\. As a result, intermediate results from each step are available on the client\.
+ **Impact: **The key derived using the `CKM_ECDH1_DERIVE` mechanism is first available on the client and is then imported into the HSM\. A key handle is then returned to your application\.
+ **Workaround: **If you are implementing SLL/TLS Offload in AWS CloudHSM, this limitation may not be an issue\. If your application requires your key to remain within an FIPS boundary at all times, consider using an alternative protocol that does not rely on ECDH key derivation\.
+ **Resolution status: **We are developing the option to perform ECDH key derivation entirely within the HSM\. The updated implementation will be announced on the version history page once available\.

## Known Issues for the JCE SDK<a name="ki-jce-sdk"></a>

**Issue: **You cannot specify attributes when unwrapping keys\.
+ **Impact:** All keys are unwrapped as exportable session keys\.
+ **Workaround: **You can script key\_mgmt\_util to unwrap keys with limited attribute customization, or use the PKCS \#11 library to unwrap keys with full template support\.
+ **Resolution status: **We are planning to add full key parameter specification for the JCE SDK's unwrap command in a future release\. The update will be announced on the version history page once available\.

**Issue: **The JCE KeyStore is read only\.
+ **Impact: **You cannot store an object type that is not supported by the HSM in the JCE keystore today\. Specifically, you cannot store certificates in the keystore\. This precludes interoperability with tools like jarsigner, which expect to find the certificate in the keystore\. 
+ **Workaround: **You can rework your code to load certificates from local files or from an S3 bucket location instead of from the keystore\. 
+ **Resolution status: **We are adding support for certificate storage in the keystore\. The feature will be announced on the version history page once available\.

**Issue: **Buffers for AES\-GCM encryption cannot exceed 16,000 bytes\. Also, multi\-part AES\-GCM encryption is not supported\.
+ **Impact: **You cannot use AES\-GCM to encrypt data larger than 16,000 bytes\.
+ **Workaround: ** You can use an alternative mechanism, such as AES\-CBC, or you can divide your data into pieces and encrypt each piece individually\. If you divide the data, you must manage the divided ciphertext and its decryption\. Because FIPS requires that the initialization vector \(IV\) for AES\-GCM be generated on the HSM, the IV for each AES\-GCM\-encrypted piece of data will be different\.
+ **Resolution status: **We are fixing the SDK to fail explicitly if the data buffer is too large\. We are evaluating alternatives that support larger buffers without relying on multi\-part encryption\. Updates will be announced in the AWS CloudHSM forum and on the version history page\. 

## Known Issues for the OpenSSL SDK<a name="ki-openssl-sdk"></a>

**Issue:** Only RSA offload to the HSM is supported by default\.
+ **Impact: **To maximize performance, the SDK is not configured to offload additional functions such as random number generation or EC\-DH operations\.
+ **Workaround: **Please contact us through a support case if you need to offload additional operations\.
+ **Resolution status: **We are adding support to the SDK to configure offload options through a configuration file\. The update will be announced on the version history page once available\.

**Issue:** RSA encryption and decryption with OAEP padding using a key on the HSM is not supported\.
+ **Impact: **Any call to RSA encryption and decryption with OAEP padding fails with a divide\-by\-zero error\. This occurs because the OpenSSL dynamic engine calls the operation locally using the fake PEM file instead of offloading the operation to the HSM\. 
+ **Workaround: ** You can perform this procedure by using either the [AWS CloudHSM Software Library for PKCS \#11](pkcs11-library.md) or the [AWS CloudHSM Software Library for Java](java-library.md)\. 
+ **Resolution status: **We are adding support to the SDK to correctly offload this operation\. The update will be announced on the version history page once available\. 

**Issue: **Only private key generation of RSA and ECC keys is offloaded to the HSM\. For any other key type, the OpenSSL AWS CloudHSM engine is not used for call processing\. The local OpenSSL engine is used instead\. This generates a key locally in software\.
+ **Impact: **Because the failover is silent, there is no indication that you have not received a key that was securely generated on the HSM\. You will see an output trace that contains the string `"...........++++++"` if the key is locally generated by OpenSSL in software\. This trace is absent when the operation is offloaded to the HSM\. Because the key is not generated or stored on the HSM, it will be unavailable for future use\.
+ **Workaround: **Only use the OpenSSL engine for key types it supports\. For all other key types, use PKCS \#11 or JCE in applications, or use `key_mgmt_util` in the AWS CLI\. 