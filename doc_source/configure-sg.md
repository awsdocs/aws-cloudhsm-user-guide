# Review Cluster Security Group<a name="configure-sg"></a>

 When you create a cluster, AWS CloudHSM creates a security group with the name `cloudhsm-cluster-clusterID-sg`\. This security group contains a preconfigured TCP rule that allows inbound and outbound communication within the cluster security group over ports 2223\-2225\. This rule allows HSMs in your cluster to communicate with each other\. 

**Warning**  
Note the following:  
 Do not delete or modify the preconfigured TCP rule, which is populated in the cluster security group\. This rule can prevent connectivity issues and unauthorized access to your HSMs\. 
 The cluster security group prevents unauthorized access to your HSMs\. Anyone that can access instances in the security group can access your HSMs\. Most operations require a user to log in to the HSM\. However, it's possible to zeroize HSMs without authentication, which destroys the key material, certificates, and other data\. If this happens, data created or modified after the most recent backup is lost and unrecoverable\. To prevent the unauthorized access, ensure that only trusted administrators can modify or access the instances in the default security group\. 

 In the next step, you can [launch an Amazon EC2 instance](launch-client-instance.md) and connect it to your HSMs by [attaching the cluster security group](configure-sg-client-instance.md) to it\.