# What Is AWS CloudHSM?<a name="introduction"></a>

AWS CloudHSM provides hardware security modules in the AWS Cloud\. A hardware security module \(HSM\) is a computing device that processes cryptographic operations and provides secure storage for cryptographic keys\.



## Features<a name="features"></a>

When you use an HSM from AWS CloudHSM, you can perform a variety of cryptographic tasks:

+ Generate, store, import, export, and manage cryptographic keys, including symmetric keys and asymmetric key pairs\.

+ Use symmetric and asymmetric algorithms to encrypt and decrypt data\.

+ Use cryptographic hash functions to compute message digests and hash\-based message authentication codes \(HMACs\)\.

+ Cryptographically sign data \(including code signing\) and verify signatures\.

+ Generate cryptographically secure random data\.

To learn more about what you can do with AWS CloudHSM, see [Use Cases](use-cases.md)\.

To learn more about the fundamental concepts of AWS CloudHSM, see [Concepts](concepts.md)\.

When you are ready to get started with AWS CloudHSM, see [Getting Started: Create A Cluster](getting-started.md)\.

With AWS CloudHSM, you get your own HSM hosted in the AWS Cloud, giving you the control that comes with operating an HSM\. If you want a managed service for creating and controlling your data's encryption keys, but you don't want or need to operate your own HSM, consider [AWS Key Management Service](https://aws.amazon.com/kms/)\.

## Compliance<a name="compliance"></a>

AWS and AWS Marketplace partners offer many solutions for protecting data in AWS\. But some applications and data are subject to strict contractual or regulatory requirements for managing and using cryptographic keys\. Using an HSM from AWS CloudHSM can help you meet corporate, contractual, and regulatory compliance requirements for data security in the AWS Cloud\.

**FIPS 140\-2**  
The Federal Information Processing Standard \(FIPS\) Publication 140\-2 is a US government security standard that specifies security requirements for cryptographic modules that protect sensitive information\. The HSMs provided by AWS CloudHSM comply with FIPS 140\-2 level 3\. For more information, see [FIPS Compliance](https://aws.amazon.com/compliance/fips/) on the AWS website\.

**PCI DSS**  
The Payment Card Industry Data Security Standard \(PCI DSS\) is a proprietary information security standard administered by the [PCI Security Standards Council](https://www.pcisecuritystandards.org/)\. The HSMs provided by AWS CloudHSM comply with PCI DSS\. For more information, see [PCI DSS Compliance](https://aws.amazon.com/compliance/pci-dss-level-1-faqs/) on the AWS website\.

## Pricing<a name="pricing"></a>

With AWS CloudHSM, you pay by the hour with no long\-term commitments or upfront payments\. For more information, see [AWS CloudHSM Pricing](https://aws.amazon.com/cloudhsm/pricing/) on the AWS website\.