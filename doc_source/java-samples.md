# Code Samples for the AWS CloudHSM Software Library for Java<a name="java-samples"></a>

## Sample Code Prerequisites<a name="w24aac15c14c12b3"></a>

 Before running the samples, use the following steps to set up your environment:
+ Install and configure the [AWS CloudHSM software library for Java](java-library-install.md) and the [AWS CloudHSM client package](install-and-configure-client-linux.md)\. 
+ Set up a valid [HSM user name and password](manage-hsm-users.md)\. Cryptographic user \(CU\) permissions are sufficient for these tasks\. Your application uses these credentials to log in to the HSM in each example\.
+ Decide how to specify the Cavium provider\.

## Code Samples<a name="w24aac15c14c12b5"></a>

The following Java library code samples show you how to use the [AWS CloudHSM software library for Java](java-library.md) to perform basic tasks in AWS CloudHSM\. More code samples are available on [GitHub](https://github.com/aws-samples/aws-cloudhsm-jce-examples/)\.
+ [Login to an HSM](https://github.com/aws-samples/aws-cloudhsm-jce-examples/blob/master/src/main/java/com/amazonaws/cloudhsm/examples/LoginRunner.java)
+ [Manage keys](https://github.com/aws-samples/aws-cloudhsm-jce-examples/blob/master/src/main/java/com/amazonaws/cloudhsm/examples/KeyUtilitiesRunner.java)
+ [Generate an AES key](https://github.com/aws-samples/aws-cloudhsm-jce-examples/blob/master/src/main/java/com/amazonaws/cloudhsm/examples/SymmetricKeys.java)
+ [Encrypt and decrypt data with AES GCM](https://github.com/aws-samples/aws-cloudhsm-jce-examples/blob/master/src/main/java/com/amazonaws/cloudhsm/examples/AESGCMEncryptDecryptRunner.java)
+ [Wrap and unwrap keys with AES](https://github.com/aws-samples/aws-cloudhsm-jce-examples/blob/master/src/main/java/com/amazonaws/cloudhsm/examples/AESWrappingRunner.java)
+ [Wrap and unwrap keys with RSA](https://github.com/aws-samples/aws-cloudhsm-jce-examples/blob/master/src/main/java/com/amazonaws/cloudhsm/examples/RSAWrappingRunner.java)
+ [Enumerate through the KeyStore](https://github.com/aws-samples/aws-cloudhsm-jce-examples/blob/master/src/main/java/com/amazonaws/cloudhsm/examples/KeyStoreExampleRunner.java)
+ [Sign messages in a multi\-threaded sample](https://github.com/aws-samples/aws-cloudhsm-jce-examples/blob/master/src/main/java/com/amazonaws/cloudhsm/examples/SignThreadedRunner.java)
+ [Use JCE instead of OpenSSL to perform ECDH key derivation](https://github.com/aws-samples/aws-cloudhsm-jce-examples/blob/master/src/main/java/com/amazonaws/cloudhsm/examples/SignThreadedRunner.java)