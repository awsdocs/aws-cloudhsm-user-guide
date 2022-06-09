# Infrastructure security in AWS CloudHSM<a name="infrastructure-security"></a>

As a managed service, AWS CloudHSM is protected by the AWS global network security procedures that are described in the [Amazon Web Services: Overview of Security Processes](https://d0.awsstatic.com/whitepapers/Security/AWS_Security_Whitepaper.pdf) whitepaper\.

You use AWS published API calls to access AWS CloudHSM through the network\. Clients must support Transport Layer Security \(TLS\) 1\.0 or later\. We recommend TLS 1\.2 or later\. Clients must also support cipher suites with perfect forward secrecy \(PFS\) such as Ephemeral Diffie\-Hellman \(DHE\) or Elliptic Curve Ephemeral Diffie\-Hellman \(ECDHE\)\. Most modern systems such as Java 7 and later support these modes\.

Additionally, requests must be signed by using an access key ID and a secret access key that is associated with an IAM principal\. Or you can use the [AWS Security Token Service](https://docs.aws.amazon.com/STS/latest/APIReference/Welcome.html) \(AWS STS\) to generate temporary security credentials to sign requests\.

## Network isolation<a name="network-isolation"></a>

A virtual private cloud \(VPC\) is a virtual network in your own logically isolated area in the AWS cloud\. You can create a cluster in a private subnet in your VPC\. For more information, see [Create a private subnet](create-subnets.md)\.

When you create an HSM, AWS CloudHSM put an elastic network interface \(ENI\) in your subnet so that you can interact with your HSMs\. For more information, see [Cluster architecture](clusters.md#cluster-architecture)\.

AWS CloudHSM creates a security group that allows inbound and outbound communication between HSMs in your cluster\. You can use this security group to enable your EC2 instances to communicate with the HSMs in your cluster\. For more information, see [Connect Amazon EC2 instance to AWS CloudHSM cluster](configure-sg-client-instance.md)\.

## Authorization of users<a name="authorization"></a>

With AWS CloudHSM, operations performed on the HSM require the credentials of an authenticated HSM user\. For more information, see [Understanding HSM users](manage-hsm-users.md#understanding-users)\.