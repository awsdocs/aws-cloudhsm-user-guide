# Configure Windows Server as a Certificate Authority \(CA\) with AWS CloudHSM<a name="win-ca-overview"></a>

In a public key infrastructure \(PKI\), a certificate authority \(CA\) is a trusted entity that issues digital certificates\. These digital certificates bind a public key to an identity \(a person or organization\) by means of public key cryptography and digital signatures\. To operate a CA, you must maintain trust by protecting the private key that signs the certificates issued by your CA\. You can store the private key in the HSM in your AWS CloudHSM cluster, and use the HSM to perform the cryptographic signing operations\.

In this tutorial, you use Windows Server and AWS CloudHSM to configure a CA\. You install the AWS CloudHSM client software for Windows on your Windows server, then add the Active Directory Certificate Services \(AD CS\) role to your Windows Server\. When you configure this role, you use an AWS CloudHSM key storage provider \(KSP\) to create and store the CA's private key on your AWS CloudHSM cluster\. The KSP is the bridge that connects your Windows server to your AWS CloudHSM cluster\. In the last step, you sign a certificate signing request \(CSR\) with your Windows Server CA\.

For more information, see the following topics:

**Topics**
+ [Windows Server CA Step 1: Set Up the Prerequisites](win-ca-prerequisites.md)
+ [Windows Server CA Step 2: Create a Windows Server CA with AWS CloudHSM](win-ca-setup.md)
+ [Windows Server CA Step 3: Sign a Certificate Signing Request \(CSR\) with Your Windows Server CA with AWS CloudHSM](win-ca-sign-csr.md)