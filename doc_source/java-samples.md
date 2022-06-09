# Code samples for the AWS CloudHSM software library for Java for Client SDK 3<a name="java-samples"></a>

## Prerequisites<a name="java-samples-prereqs"></a>

 Before running the samples, you must set up your environment:
+ Install and configure the [Java Cryptographic Extension \(JCE\) provider](java-library-install.md#install-java-library) and the [AWS CloudHSM client package](install-and-configure-client-linux.md)\. 
+ Set up a valid [HSM user name and password](manage-hsm-users.md)\. Cryptographic user \(CU\) permissions are sufficient for these tasks\. Your application uses these credentials to log in to the HSM in each example\.
+ Decide how to provide credentials to the [JCE provider](java-library-install.md#java-library-credentials)\.

## Code samples<a name="java-samples-code"></a>

The following code samples show you how to use the [AWS CloudHSM JCE provider](java-library.md) to perform basic tasks\. More code samples are available on [GitHub](https://github.com/aws-samples/aws-cloudhsm-jce-examples/)\.
+ [Log in to an HSM](https://github.com/aws-samples/aws-cloudhsm-jce-examples/blob/master/src/main/java/com/amazonaws/cloudhsm/examples/LoginRunner.java)
+ [Manage keys](https://github.com/aws-samples/aws-cloudhsm-jce-examples/blob/master/src/main/java/com/amazonaws/cloudhsm/examples/KeyUtilitiesRunner.java)
+ [Generate an AES key](https://github.com/aws-samples/aws-cloudhsm-jce-examples/blob/master/src/main/java/com/amazonaws/cloudhsm/examples/SymmetricKeys.java)
+ [Encrypt and decrypt with AES\-GCM](https://github.com/aws-samples/aws-cloudhsm-jce-examples/blob/master/src/main/java/com/amazonaws/cloudhsm/examples/AESGCMEncryptDecryptRunner.java)
+ [Encrypt and decrypt with AES\-CTR]( https://github.com/aws-samples/aws-cloudhsm-jce-examples/blob/master/src/main/java/com/amazonaws/cloudhsm/examples/AESCTREncryptDecryptRunner.java)
+ [Encrypt and decrypt with D3DES\-ECB]( https://github.com/aws-samples/aws-cloudhsm-jce-examples/blob/master/src/main/java/com/amazonaws/cloudhsm/examples/DESedeECBEncryptDecryptRunner.java)
+ [Wrap and unwrap keys with AES\-GCM](https://github.com/aws-samples/aws-cloudhsm-jce-examples/blob/master/src/main/java/com/amazonaws/cloudhsm/examples/AESGCMWrappingRunner.java)
+ [Wrap and unwrap keys with AES](https://github.com/aws-samples/aws-cloudhsm-jce-examples/blob/master/src/main/java/com/amazonaws/cloudhsm/examples/AESWrappingRunner.java)
+ [Wrap and unwrap keys with RSA](https://github.com/aws-samples/aws-cloudhsm-jce-examples/blob/master/src/main/java/com/amazonaws/cloudhsm/examples/RSAWrappingRunner.java)
+ [Use supported key attributes](https://github.com/aws-samples/aws-cloudhsm-jce-examples/blob/master/src/main/java/com/amazonaws/cloudhsm/examples/CustomKeyAttributesRunner.java)
+ [Enumerate keys in the key store](https://github.com/aws-samples/aws-cloudhsm-jce-examples/blob/master/src/main/java/com/amazonaws/cloudhsm/examples/KeyStoreExampleRunner.java)
+ [ Use the CloudHSM key store](https://github.com/aws-samples/aws-cloudhsm-jce-examples/blob/master/src/main/java/com/amazonaws/cloudhsm/examples/CloudHSMKeyStoreExampleRunner.java)
+ [Sign messages in a multi\-threaded sample](https://github.com/aws-samples/aws-cloudhsm-jce-examples/blob/master/src/main/java/com/amazonaws/cloudhsm/examples/SignThreadedRunner.java)
+ [Sign and Verify with EC Keys](https://github.com/aws-samples/aws-cloudhsm-jce-examples/blob/master/src/main/java/com/amazonaws/cloudhsm/examples/ECOperationsRunner.java)