# AWS CloudHSM Client SDKs<a name="use-hsm"></a>

Use a Client SDK to offload cryptographic operations from platform or language\-based applications to hardware security modules \(HSMs\)\. 

AWS CloudHSM offers two major versions, and Client SDK 5 is the latest\. It offers a variety of advantages over Client SDK 3 \(the previous series\)\. For more information, see [ Benefits of Client SDK 5](client-sdk-5-benefits.md)\. For information about platform support, see [Client SDK 5 supported platforms](client-supported-platforms.md)\. 

For information on using Client SDK 3, see [Previous Client SDK versions](choose-client-sdk.md)\.

**[PKCS \#11 library](pkcs11-library.md)**  
 PKCS \#11 is a standard for performing cryptographic operations on hardware security modules \(HSMs\)\. AWS CloudHSM offers implementations of the PKCS \#11 library that are compliant with PKCS \#11 version 2\.40\.

**[OpenSSL Dynamic Engine](openssl-library.md)**  
The AWS CloudHSM OpenSSL Dynamic Engine allows you to offload cryptographic operations to your CloudHSM cluster through the OpenSSL API\.

**[JCE provider](java-library.md)**  
The AWS CloudHSM JCE provider is compliant with the Java Cryptographic Architecture \(JCA\)\. The provider allows you to perform cryptographic operations on the HSM\.

**[Cryptography API: Next Generation \(CNG\) and key storage providers \(KSP\) for Microsoft Windows](ksp-library.md)**  
The AWS CloudHSM client for Windows includes CNG and KSP providers\. Currently, only Client SDK 3 supports CNG and KSP providers\.