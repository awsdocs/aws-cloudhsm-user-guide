# AWS CloudHSM client end\-to\-end encryption<a name="client-end-to-end-encryption"></a>

Communication between the client instance and the HSMs in your cluster is encrypted from end to end\. Only your client and your HSMs can decrypt the communication\.

The following process explains how the client establishes end\-to\-end encrypted communication with an HSM\.

1. Your client establishes a Transport Layer Security \(TLS\) connection with the server that hosts your HSM hardware\. Your cluster's security group allows inbound traffic to the server only from client instances in the security group\. The client also checks the server's certificate to ensure that it's a trusted server\.  
![\[A TLS connection between the client and the server.\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/end-to-end-encryption-part-1.png)

1. Next, the client establishes an encrypted connection with the HSM hardware\. The HSM has the cluster certificate that you signed with your own certificate authority \(CA\), and the client has the CA's root certificate\. Before the client–HSM encrypted connection is established, the client verifies the HSM's cluster certificate against its root certificate\. The connection is established only when the client successfully verifies that the HSM is trusted\.  
![\[A secure, end-to-end encrypted connection between the client and the HSM.\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/end-to-end-encryption-part-2.png)