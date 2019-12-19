# Compliance Validation for AWS CloudHSM<a name="compliance"></a>

Third\-party auditors assess the security and compliance of AWS CloudHSM as part of multiple AWS compliance programs\. These include SOC, PCI, FedRAMP, HIPAA, and others\.

For a list of AWS services in scope of specific compliance programs, see [AWS Services in Scope by Compliance Program](http://aws.amazon.com/compliance/services-in-scope/)\. For general information, see [AWS Compliance Programs](http://aws.amazon.com/compliance/programs/)\.

You can download third\-party audit reports using AWS Artifact\. For more information, see [Downloading Reports in AWS Artifact](https://docs.aws.amazon.com/artifact/latest/ug/downloading-documents.html)\.

Your compliance responsibility when using AWS CloudHSM is determined by the sensitivity of your data, your company's compliance objectives, and applicable laws and regulations\. AWS provides the following resources to help with compliance:
+ [Security and Compliance Quick Start Guides](http://aws.amazon.com/quickstart/?awsf.quickstart-homepage-filter=categories%23security-identity-compliance) – These deployment guides discuss architectural considerations and provide steps for deploying security\- and compliance\-focused baseline environments on AWS\.
+ [Architecting for HIPAA Security and Compliance Whitepaper ](https://d0.awsstatic.com/whitepapers/compliance/AWS_HIPAA_Compliance_Whitepaper.pdf) – This whitepaper describes how companies can use AWS to create HIPAA\-compliant applications\.
+ [AWS Compliance Resources](http://aws.amazon.com/compliance/resources/) – This collection of workbooks and guides might apply to your industry and location\.
+ [Evaluating Resources with Rules](https://docs.aws.amazon.com/config/latest/developerguide/evaluate-config.html) in the *AWS Config Developer Guide* – The AWS Config service assesses how well your resource configurations comply with internal practices, industry guidelines, and regulations\.
+ [AWS Security Hub](https://docs.aws.amazon.com/securityhub/latest/userguide/what-is-securityhub.html) – This AWS service provides a comprehensive view of your security state within AWS that helps you check your compliance with security industry standards and best practices\.

## FIPS Validation<a name="fips-validation"></a>

Relying on a FIPS\-validated HSM can help you meet corporate, contractual, and regulatory compliance requirements for data security in the AWS Cloud\. You can review the FIPS\-approved security policies for the HSMs provided by AWS CloudHSM below\.

**[FIPS Validation for Hardware Used by CloudHSM](https://csrc.nist.gov/Projects/Cryptographic-Module-Validation-Program)**  
+ [Certificate \#3254](https://csrc.nist.gov/Projects/Cryptographic-Module-Validation-Program/Certificate/3254) was issued on August 2, 2018
+ [Certificate \#2850](https://csrc.nist.gov/projects/cryptographic-module-validation-program/Certificate/2850) was issued on February 27, 2017

**[FIPS 140\-2 Compliance](https://aws.amazon.com/compliance/fips/)**  
The Federal Information Processing Standard \(FIPS\) Publication 140\-2 is a US government security standard that specifies security requirements for cryptographic modules that protect sensitive information\. The HSMs provided by AWS CloudHSM comply with FIPS 140\-2 level 3\.

**[PCI DSS Compliance](https://aws.amazon.com/compliance/pci-dss-level-1-faqs/)**  
The Payment Card Industry Data Security Standard \(PCI DSS\) is a proprietary information security standard administered by the [PCI Security Standards Council](https://www.pcisecuritystandards.org/)\. The HSMs provided by AWS CloudHSM comply with PCI DSS\.