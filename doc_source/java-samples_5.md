# Code samples for the AWS CloudHSM software library for Java for Client SDK 5<a name="java-samples_5"></a>

## Prerequisites<a name="java-samples-prereqs_5"></a>

 Before running the samples, you must set up your environment:
+ Install and configure the [Java Cryptographic Extension \(JCE\) provider](java-library-install_5.md#install-java-library_5)\. 
+ Set up a valid [HSM user name and password](manage-hsm-users.md)\. Cryptographic user \(CU\) permissions are sufficient for these tasks\. Your application uses these credentials to log in to the HSM in each example\.
+ Decide how to provide credentials to the [JCE provider](java-library-install_5.md#java-library-credentials_5)\.

## Code samples<a name="java-samples-code_5"></a>

The following code samples show you how to use the [AWS CloudHSM JCE provider](java-library.md) to perform basic tasks\. More code samples are available on [GitHub](https://github.com/aws-samples/aws-cloudhsm-jce-examples/tree/sdk5)\.
+ [Log in to an HSM](https://github.com/aws-samples/aws-cloudhsm-jce-examples/blob/sdk5/src/main/java/com/amazonaws/cloudhsm/examples/LoginRunner.java)
+ [Manage keys](https://github.com/aws-samples/aws-cloudhsm-jce-examples/blob/sdk5/src/main/java/com/amazonaws/cloudhsm/examples/KeyUtilitiesRunner.java)
+ [Generate Symmetric Keys](https://github.com/aws-samples/aws-cloudhsm-jce-examples/blob/sdk5/src/main/java/com/amazonaws/cloudhsm/examples/SymmetricKeys.java)
+ [Generate Asymmetric Keys](https://github.com/aws-samples/aws-cloudhsm-jce-examples/blob/sdk5/src/main/java/com/amazonaws/cloudhsm/examples/AsymmetricKeys.java)
+ [Encrypt and decrypt with AES\-GCM](https://github.com/aws-samples/aws-cloudhsm-jce-examples/blob/sdk5/src/main/java/com/amazonaws/cloudhsm/examples/AESGCMEncryptDecryptRunner.java)
+ [Encrypt and decrypt with AES\-CTR](https://github.com/aws-samples/aws-cloudhsm-jce-examples/blob/sdk5/src/main/java/com/amazonaws/cloudhsm/examples/AESCTREncryptDecryptRunner.java)
+ [Encrypt and decrypt with DESede\-ECB](https://github.com/aws-samples/aws-cloudhsm-jce-examples/blob/sdk5/src/main/java/com/amazonaws/cloudhsm/examples/DESedeECBEncryptDecryptRunner.java)
+ [Sign and Verify with RSA Keys](https://github.com/aws-samples/aws-cloudhsm-jce-examples/blob/sdk5/src/main/java/com/amazonaws/cloudhsm/examples/RSAOperationsRunner.java)
+ [Sign and Verify with EC Keys](https://github.com/aws-samples/aws-cloudhsm-jce-examples/blob/sdk5/src/main/java/com/amazonaws/cloudhsm/examples/ECOperationsRunner.java)
+ [Use supported key attributes](https://github.com/aws-samples/aws-cloudhsm-jce-examples/blob/sdk5/src/main/java/com/amazonaws/cloudhsm/examples/KeyAttributesRunner.java)
+ [Use the CloudHSM key store](https://github.com/aws-samples/aws-cloudhsm-jce-examples/blob/sdk5/src/main/java/com/amazonaws/cloudhsm/examples/KeyStoreExampleRunner.java)