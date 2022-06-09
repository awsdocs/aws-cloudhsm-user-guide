# AWS CloudHSM Client SDKs<a name="use-hsm"></a>

You use a Client SDK to offload cryptographic operations to the hardware security modules \(HSM\) from various platform or language\-based applications and to perform certain management tasks on the HSM\. AWS CloudHSM offers two major versions, Client SDK 5 and Client SDK 3\. For more information, see [Choose a Client SDK version](choose-client-sdk.md)\.

**[PKCS \#11 library](pkcs11-library.md)**  
 The Organization for Advancement of Structured Information Standards \(OASIS\) owns the ongoing development of the Public Key Cryptography Standards \(PKCS\)\. The PKCS \#11 standard defines a platform\-independent API to perform cryptographic operations on hardware devices like a HSM\. Use these libraries to perform cryptographic operations on the HSM\. For more information, see [PKCS \#11 library](pkcs11-library.md)\.

**[OpenSSL Dynamic Engine](openssl-library.md)**  
OpenSSL is the default library for cryptographic operations on Linux based operating systems\. You can use OpenSSL to perform cryptographic operations to a HSM with an engine\. In open source, an engine is similar to a provider\. The CloudHSM implementation of an OpenSSL dynamic engine offloads limited operations to the HSM\. The most common use case for this is SSL/TLS offload using Apache or Nginx\. For more information, see [OpenSSL Dynamic Engine](openssl-library.md)\.

**[JCE provider](java-library.md)**  
Java Cryptography Extensions \(JCE\) provides advanced cryptographic operations like encryption, decryption, and key generation in Java\. While the Java Development Kit \(JDK\) ships with a default implementation of JCE, that implementation is not designed for use with an HSM\. However, the JCE offers an architecture that allows vendors to write providers that can easily plug into the JDK\. CloudHSM provides one such implementation of a JCE provider that allows Java applications to offload cryptographic operations to the HSM\. For more information, see [JCE provider](java-library.md)\.

**[Cryptography API: Next Generation \(CNG\) and key storage providers \(KSP\) for Microsoft Windows](ksp-library.md)**  
For Microsoft Windows, CloudHSM provides a limited implementation of the CNG and KSP providers that allow selected cryptographic operations to be offloaded to the HSM\. The most common use case for this is Active Directory Certificate Services \(AD CS\)\. For more information, see [Cryptography API: Next Generation \(CNG\) and key storage providers \(KSP\) for Microsoft Windows](ksp-library.md)\.