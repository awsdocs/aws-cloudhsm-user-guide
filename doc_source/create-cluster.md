# Create a cluster<a name="create-cluster"></a>

A cluster is a collection of individual HSMs\. AWS CloudHSM synchronizes the HSMs in each cluster so that they function as a logical unit\.

When you create a cluster, AWS CloudHSM creates a security group for the cluster on your behalf\. This security group controls network access to the HSMs in the cluster\. It allows inbound connections only from Amazon Elastic Compute Cloud \(Amazon EC2\) instances that are in the security group\. By default, the security group doesn't contain any instances\. Later, you [launch a client instance](launch-client-instance.md) and [configure the cluster's security group](configure-sg.md) to allow communication and connections with the HSM\.

**Important**  
When you create a cluster, AWS CloudHSM creates a [service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html) named AWSServiceRoleForCloudHSM\. If AWS CloudHSM cannot create the role or the role does not already exist, you may not be able to create a cluster\. For more information, see [Resolving cluster creation failures](troubleshooting-create-cluster.md)\. For more information about service–linked roles, see [Service\-linked roles for AWS CloudHSM](service-linked-roles.md)\. 

You can create a cluster from the [AWS CloudHSM console](https://console.aws.amazon.com/cloudhsm/), the [AWS Command Line Interface \(AWS CLI\)](https://aws.amazon.com/cli/), or the AWS CloudHSM API\. 

**To create a cluster \(console\)**

1. Open the AWS CloudHSM console at [https://console\.aws\.amazon\.com/cloudhsm/home](https://console.aws.amazon.com/cloudhsm/home)\.

1. On the navigation bar, use the region selector to choose one of the [AWS Regions where AWS CloudHSM is currently supported](https://docs.aws.amazon.com/general/latest/gr/rande.html#cloudhsm_region)\. 

1. Choose **Create cluster**\.

1. In the **Cluster configuration** section, do the following:

   1. For **VPC**, select the VPC that you created\.

   1. For **AZ\(s\)**, next to each Availability Zone, choose the private subnet that you created\. 
**Note**  
Even if AWS CloudHSM is not supported in a given Availability Zone, performance should not be affected, as AWS CloudHSM automatically load balances across all HSMs in a cluster\. See [AWS CloudHSM Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#cloudhsm_region) in the *AWS General Reference* to see Availability Zone support for AWS CloudHSM\.

1. Choose **Next**\.

1. Specify how long the service should retain backups\.
**Note**  
Accept the default retention period of 90 days or type a new value between 7 and 379 days\. The service will automatically delete backups in this cluster older than the value you specify here\. You can change this later\. For more information, see [Configuring backup retention](manage-backup-retention.md)\.

1. Choose **Next**\.

1. \(Optional\) Type a tag key and an optional tag value\. To add more than one tag to the cluster, choose **Add tag**\.

1. Choose **Review**\.

1. Review your cluster configuration, and then choose **Create cluster**\.

**To create a cluster \([AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/)\)**
+ At a command prompt, run the [create\-cluster](https://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/create-cluster.html) command\. Specify the HSM instance type, the backup retention period, and the subnet IDs of the subnets where you plan to create HSMs\. Use the subnet IDs of the private subnets that you created\. Specify only one subnet per Availability Zone\. 

  ```
  $ aws cloudhsmv2 create-cluster --hsm-type hsm1.medium \
    				--backup-retention-policy Type=DAYS,Value=<number of days> \
    				--subnet-ids <subnet ID>
  
  {
      "Cluster": {
          "BackupPolicy": "DEFAULT",
          "BackupRetentionPolicy": {
              "Type": "DAYS",
              "Value": 90
           },
          "VpcId": "vpc-50ae0636",
          "SubnetMapping": {
              "us-west-2b": "subnet-49a1bc00",
              "us-west-2c": "subnet-6f950334",
              "us-west-2a": "subnet-fd54af9b"
          },
          "SecurityGroup": "sg-6cb2c216",
          "HsmType": "hsm1.medium",
          "Certificates": {},
          "State": "CREATE_IN_PROGRESS",
          "Hsms": [],
          "ClusterId": "cluster-igklspoyj5v",
          "CreateTimestamp": 1502423370.069
      }
  }
  ```

**To create a cluster \(AWS CloudHSM API\)**
+ Send a [https://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_CreateCluster.html](https://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_CreateCluster.html) request\. Specify the HSM instance type, the backup retention policy, and the subnet IDs of the subnets where you plan to create HSMs\. Use the subnet IDs of the private subnets that you created\. Specify only one subnet per Availability Zone\.

If your attempts to create a cluster fail, it might be related to problems with the AWS CloudHSM service\-linked roles\. For help on resolving the failure, see [Resolving cluster creation failures](troubleshooting-create-cluster.md)\.