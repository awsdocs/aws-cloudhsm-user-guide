# Cryptography API: Next Generation \(CNG\) and key storage providers \(KSP\) for Microsoft Windows<a name="ksp-library"></a>

The AWS CloudHSM client for Windows includes CNG and KSP providers\. Currently, only Client SDK 3 supports CNG and KSP providers\.

*Key storage providers* \(KSPs\) enable key storage and retrieval\. For example, if you add the Microsoft Active Directory Certificate Services \(AD CS\) role to your Windows server and choose to create a new private key for your certificate authority \(CA\), you can choose the KSP that will manage key storage\. When you configure the AD CS role, you can choose this KSP\. For more information, see [Create Windows Server CA](win-ca-setup.md)\. 

*Cryptography API: Next Generation \(CNG\)* is a cryptographic API specific to the Microsoft Windows operating system\. CNG enables developers to use cryptographic techniques to secure Windows\-based applications\. At a high level, the AWS CloudHSM implementation of CNG provides the following functionality: 
+ **Cryptographic Primitives** \- enable you to perform fundamental cryptographic operations\.
+ **Key Import and Export** \- enables you to import and export asymmetric keys\.
+ **Data Protection API \(CNG DPAPI\)** \- enables you to easily encrypt and decrypt data\.
+ **Key Storage and Retrieval** \- enables you to securely store and isolate the private key of an asymmetric key pair\.

**Topics**
+ [Verifying the KSP and CNG Providers for Windows](ksp-library-install.md)
+ [Windows AWS CloudHSM prerequisites](ksp-library-prereq.md)
+ [Associate a AWS CloudHSM key with a certificate](ksp-library-associate-key-certificate.md)
+ [Code sample for CNG provider](ksp-library-sample.md)