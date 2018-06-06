# KSP and CNG Providers for Windows<a name="ksp-library"></a>

Cryptography API: Next Generation \(CNG\) is a cryptographic API specific to the Microsoft Windows operating system\. CNG enables developers to use cryptographic techniques to secure Windows\-based applications\. At a high level, CNG provides the following functionality\. 
+ **Cryptographic Primitives** \- enable you to perform fundamental cryptographic operations\.
+ **Key Import and Export** \- enables you to import and export symmetric and asymmetric keys\.
+ **Data Protection API \(CNG DPAPI\)** \- enables you to easily encrypt and decrypt data\.
+ **Key Storage and Retrieval** \- enables you to securely store and isolate the private key of an asymmetric key pair\.

Key storage providers \(KSPs\) enable key storage and retrieval\. For example, if you add the Microsoft Active Directory Certificate Services \(AD CS\) role to your Windows server and you choose to create a new private key for your certificate authority \(CA\), you can choose the KSP that will manage key storage\. The Windows CloudHSM client includes KSPs created by Cavium specifically for AWS CloudHSM\. When you configure the AD CS role, you can choose a Cavium KSP\. For more information, see [Create Windows Server CA](win-ca-setup.md)\. The Windows CloudHSM client also installs a Cavium CNG provider\. 

**Topics**
+ [Install the KSP and CNG Providers for Windows](ksp-library-install.md)
+ [Windows AWS CloudHSM Prerequisites](ksp-library-prereq.md)
+ [Code Sample for Cavium CNG Provider](ksp-library-sample.md)