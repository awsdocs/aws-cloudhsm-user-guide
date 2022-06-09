# Known issues for the JCE SDK<a name="ki-jce-sdk"></a>

**Topics**
+ [Issue: When working with asymmetric key pairs, you see occupied key capacity even when you are not explicitly creating or importing keys](#ki-jce-1)
+ [Issue: You cannot specify attributes when unwrapping keys](#ki-jce-2)
+ [Issue: The JCE KeyStore is read only](#ki-jce-3)
+ [Issue: Buffers for AES\-GCM encryption cannot exceed 16,000 bytes](#ki-jce-4)
+ [Issue: Elliptic\-curve Diffie\-Hellman \(ECDH\) key derivation is executed partially within the HSM](#ki-jce-5)
+ [Issue: KeyGenerator and KeyAttribute incorrectly interprets key size parameter as number of bytes instead of bits](#ki-jce-6)

## Issue: When working with asymmetric key pairs, you see occupied key capacity even when you are not explicitly creating or importing keys<a name="ki-jce-1"></a>
+ **Impact:** This issue can cause your HSMs to unexpectedly run out of key space and occurs when your application uses a standard JCE key object for crypto operations instead of a `CaviumKey` object\. When you use a standard JCE key object, `CaviumProvider` implicitly imports that key into the HSM as a session key and does not delete this key until the application exits\. As a result, keys build up while the application is running and can cause your HSMs to run out of free key space, thus freezing your application\. 
+ **Workaround: **When using the `CaviumSignature` class, `CaviumCipher` class, `CaviumMac` class, or the `CaviumKeyAgreement` class, you should supply the key as a `CaviumKey` instead of a standard JCE key object\.

  You can manually convert a normal key to a `CaviumKey` using the [https://github.com/aws-samples/aws-cloudhsm-jce-examples/blob/master/src/main/java/com/amazonaws/cloudhsm/examples/KeyUtilitiesRunner.java](https://github.com/aws-samples/aws-cloudhsm-jce-examples/blob/master/src/main/java/com/amazonaws/cloudhsm/examples/KeyUtilitiesRunner.java) class, and can then manually delete the key after the operation is complete\.
+ **Resolution status: **We are updating the `CaviumProvider` to properly manage implicit imports\. The fix will be announced on the version history page once available\.

## Issue: You cannot specify attributes when unwrapping keys<a name="ki-jce-2"></a>
+ **Impact:** All keys are unwrapped as exportable session keys\.
+ **Workaround: **You can script key\_mgmt\_util to unwrap keys with limited attribute customization, or use the PKCS \#11 library to unwrap keys with full template support\.
+ **Resolution status: **We are planning to add full key parameter specification for the JCE SDK's unwrap command in a future release\. The update will be announced on the version history page once available\.

## Issue: The JCE KeyStore is read only<a name="ki-jce-3"></a>
+ **Impact: **You cannot store an object type that is not supported by the HSM in the JCE keystore today\. Specifically, you cannot store certificates in the keystore\. This precludes interoperability with tools like jarsigner, which expect to find the certificate in the keystore\. 
+ **Workaround: **You can rework your code to load certificates from local files or from an S3 bucket location instead of from the keystore\. 
+ **Resolution status: **We are adding support for certificate storage in the keystore\. The feature will be announced on the version history page once available\.

## Issue: Buffers for AES\-GCM encryption cannot exceed 16,000 bytes<a name="ki-jce-4"></a>

Multi\-part AES\-GCM encryption is not supported\.
+ **Impact: **You cannot use AES\-GCM to encrypt data larger than 16,000 bytes\.
+ **Workaround: ** You can use an alternative mechanism, such as AES\-CBC, or you can divide your data into pieces and encrypt each piece individually\. If you divide the data, you must manage the divided ciphertext and its decryption\. Because FIPS requires that the initialization vector \(IV\) for AES\-GCM be generated on the HSM, the IV for each AES\-GCM\-encrypted piece of data will be different\.
+ **Resolution status: **We are fixing the SDK to fail explicitly if the data buffer is too large\. We are evaluating alternatives that support larger buffers without relying on multi\-part encryption\. Updates will be announced in the AWS CloudHSM forum and on the version history page\. 

## Issue: Elliptic\-curve Diffie\-Hellman \(ECDH\) key derivation is executed partially within the HSM<a name="ki-jce-5"></a>

Your EC private key remains within the HSM at all times, but the key derivation process is performed in multiple steps\. As a result, intermediate results from each step are available on the client\. An ECDH key derivation sample is available in the [Java code samples](java-samples.md)\.
+ **Impact: **Software version 3\.0 adds ECDH functionality to the JCE\. When you use the CKM\_ECDH1\_DERIVE mechanism to derive the key, it is first available on the client and is then imported into the HSM\. A key handle is then returned to your application\.
+ **Workaround: **If you are implementing SSL/TLS Offload in AWS CloudHSM, this limitation may not be an issue\. If your application requires your key to remain within an FIPS boundary at all times, consider using an alternative protocol that does not rely on ECDH key derivation\.
+ **Resolution status: **We are developing the option to perform ECDH key derivation entirely within the HSM\. When available, we'll announce the updated implementation on the version history page\.

## Issue: KeyGenerator and KeyAttribute incorrectly interprets key size parameter as number of bytes instead of bits<a name="ki-jce-6"></a>

When generating a key using the `init` function of the [KeyGenerator class](https://docs.oracle.com/javase/8/docs/api/javax/crypto/KeyGenerator.html#init-int-) or the `SIZE` attribute of the [AWS CloudHSM KeyAttribute enum](java-lib-attributes_5.md), the API incorrectly expects the argument to be the number of key bytes, when it should instead be the number of key bits\. 
+ **Impact: **Client SDK versions 5\.4\.0 through 5\.4\.2 incorrectly expects the key size to be provided to the specified APIs as bytes\.
+ **Workaround: **Convert the key size from bits to bytes before using the KeyGenerator class or KeyAttribute enum to generate keys using the AWS CloudHSM JCE provider if using Client SDK versions 5\.4\.0 through 5\.4\.2\.
+ **Resolution status: **Upgrade your client SDK version to 5\.5\.0 or later, which includes a fix to correctly expect key sizes in bits when using the KeyGenerator class or KeyAttribute enum to generate keys\.