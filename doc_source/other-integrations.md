# Other third\-party vendor integrations<a name="other-integrations"></a>

Several third\-party vendors support AWS CloudHSM as a root of trust\. This means that you can utilize a software solution of your choice while creating and storing the underlying keys in your CloudHSM cluster\. As a result, your workload in AWS can rely on the latency, availability, reliability, and elasticity benefits of CloudHSM\. The following list includes third\-party vendors that support CloudHSM\.

**Note**  
AWS does not endorse or vouch for any third\-party vendor\.
+ **[Hashicorp Vault](https://www.hashicorp.com)** is a secrets management tool designed to enable collaboration and governance across organizations\. It supports AWS Key Management Service and AWS CloudHSM as roots of trust for additional protection\.
+ **[Thycotic Secrets Server](https://thycotic.com)** helps customers manage sensitive credentials across privileged accounts\. It supports AWS CloudHSM as a root of trust\. 
+ **[P6R's KMIP adapter](https://www.p6r.com/software/ksg.html)** allows you to utilize your AWS CloudHSM instances through a standard KMIP interface\. 
+ **[PrimeKey EJBCA](https://aws.amazon.com/marketplace/seller-profile?id=7edf9048-58e6-4086-9d98-b8e0c1d78fce)** is a popular open source solution for PKI\. It allows you to create and store key pairs securely with AWS CloudHSM\. 
+ **[Box KeySafe](https://blog.box.com)** provides encryption key management for cloud content to many organizations with strict security, privacy, and regulatory compliance requirements\. Customers can further secure KeySafe keys directly in AWS Key Management Service or indirectly in AWS CloudHSM via AWS KMS Custom Key Store\.
+ **[Insyde Software](https://www.insyde.com)** supports AWS CloudHSM as a root of trust for firmware signing\. 
+ **[F5 BIG\-IP LTM](https://techdocs.f5.com)** supports AWS CloudHSM as a root of trust\. 
+ **[Cloudera Navigator Key HSM](https://www.cloudera.com)** allows you to use your CloudHSM cluster to create and store keys for Cloudera Navigator Key Trustee Server\. 
+ **[Venafi Trust Protection Platform](https://marketplace.venafi.com/details/aws-cloudhsm/)** provides comprehensive machine identity management for TLS, SSH, and code signing with AWS CloudHSM key generation and protection\.