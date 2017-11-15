# Getting Started with AWS CloudHSM: Create a Cluster and HSM<a name="create-cluster-and-hsm"></a>

Complete the steps in the following topics to create an AWS CloudHSM cluster and the cluster's first HSM\.


+ [Create a Cluster](#create-cluster)
+ [Create an HSM](#create-hsm)

## Create a Cluster<a name="create-cluster"></a>

When you create a cluster, AWS CloudHSM creates a security group for the cluster on your behalf\. This security group controls network access to the HSMs in the cluster\. It allows inbound connections only from Amazon Elastic Compute Cloud \(Amazon EC2\) instances that are in the security group\. By default, the security group doesn't contain any instances\. Later, you launch a client instance and add it to this security group\.

**Warning**  
The cluster's security group prevents unauthorized access to your HSMs\. Anyone that can access instances in the security group can access your HSMs\. Most operations require a user to log in to the HSM, but it's possible to zeroize HSMs without authentication, which destroys the key material, certificates, and other data\. If this happens, data created or modified after the most recent backup is lost and unrecoverable\. To prevent this, ensure that only trusted administrators can access the instances in the cluster's security group or modify the security group\.

You can create a cluster from the [AWS CloudHSM console](https://console.aws.amazon.com/cloudhsm/), the [AWS Command Line Interface \(AWS CLI\)](https://aws.amazon.com/cli/), or the AWS CloudHSM API\.

**To create a cluster \(console\)**

1. Open the AWS CloudHSM console at [https://console\.aws\.amazon\.com/cloudhsm/](https://console.aws.amazon.com/cloudhsm/)\.

1. On the navigation bar, use the region selector to choose one of the [AWS Regions where AWS CloudHSM is currently supported](http://docs.aws.amazon.com/general/latest/gr/rande.html#cloudhsm_region)\.

1. Choose **Create cluster**\.

1. In the **Cluster configuration** section, do the following:

   1. For **VPC**, select the VPC that you created when you set up the prerequisites\.

   1. For **AZ\(s\)**, next to each Availability Zone, choose the private subnet that you created when you set up the prerequisites\.

1. Choose **Next: Review**\.

1. Review your cluster configuration, then choose **Create cluster**\.

**To create a cluster \(AWS CLI\)**

+ At a command prompt, issue the [create\-cluster](http://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/create-cluster.html) command\. Specify the HSM instance type and the subnet IDs of the subnets where you plan to create HSMs\. Use the subnet IDs of the private subnets that you created when you set up the prerequisites\. Specify only one subnet per Availability Zone\.

  ```
  $ aws cloudhsmv2 create-cluster --hsm-type hsm1.medium \
                                  --subnet-ids <subnet ID 1> <subnet ID 2> <subnet ID N>
  {
      "Cluster": {
          "BackupPolicy": "DEFAULT",
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

+ Send a [http://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_CreateCluster.html](http://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_CreateCluster.html) request\. Specify the HSM instance type and the subnet IDs of the subnets where you plan to create HSMs\. Use the subnet IDs of the private subnets that you created when you set up the prerequisites\. Specify only one subnet per Availability Zone\.

## Create an HSM<a name="create-hsm"></a>

Before you can create an HSM in the cluster, the cluster must be in the uninitialized state\. To determine the cluster's state, view the [clusters page in the AWS CloudHSM console](https://console.aws.amazon.com/cloudhsm/home/clusters), use the AWS CLI to issue the [describe\-clusters](http://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/describe-clusters.html) command, or send a [DescribeClusters](http://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_DescribeClusters.html) request in the AWS CloudHSM API\.

You can create an HSM from the [AWS CloudHSM console](https://console.aws.amazon.com/cloudhsm/), the [AWS CLI](https://aws.amazon.com/cli/), or the AWS CloudHSM API\.

**To create an HSM \(console\)**

1. Open the AWS CloudHSM console at [https://console\.aws\.amazon\.com/cloudhsm/](https://console.aws.amazon.com/cloudhsm/)\.

1. Choose **Initialize** next to the cluster that you created previously\.

1. Choose an Availability Zone \(AZ\) for the HSM that you are creating\. Then choose **Create**\.

**To create an HSM \(AWS CLI\)**

+ At a command prompt, issue the [create\-hsm](http://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/create-hsm.html) command\. Specify the cluster ID of the cluster that you created previously and an Availability Zone for the HSM\. Specify the Availability Zone in the form of `us-west-2a`, `us-west-2b`, etc\.

  ```
  $ aws cloudhsmv2 create-hsm --cluster-id <cluster ID> --availability-zone <Availability Zone>
  {
      "Hsm": {
          "HsmId": "hsm-ted36yp5b2x",
          "EniIp": "10.0.1.12",
          "AvailabilityZone": "us-west-2a",
          "ClusterId": "cluster-igklspoyj5v",
          "EniId": "eni-5d7ade72",
          "SubnetId": "subnet-fd54af9b",
          "State": "CREATE_IN_PROGRESS"
      }
  }
  ```

**To create an HSM \(AWS CloudHSM API\)**

+ Send a [http://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_CreateHsm.html](http://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_CreateHsm.html) request\. Specify the cluster ID of the cluster that you created previously and an Availability Zone for the HSM\.

After you create a cluster and HSM, you can optionally verify the identity of the HSM, or proceed directly to [Initialize the Cluster](initialize-cluster.md)\.