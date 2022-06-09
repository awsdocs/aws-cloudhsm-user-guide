# AWS CloudHSM and VPC endpoints<a name="cloudhsm-vpc-endpoint"></a>

You can establish a private connection between your VPC and AWS CloudHSM by creating an *interface VPC endpoint*\. Interface endpoints are powered by [AWS PrivateLink](http://aws.amazon.com/privatelink), a technology that enables you to privately access AWS CloudHSM APIs without an internet gateway, NAT device, VPN connection, or AWS Direct Connect connection\. Instances in your VPC don't need public IP addresses to communicate with AWS CloudHSM APIs\. Traffic between your VPC and AWS CloudHSM does not leave the Amazon network\. 

Each interface endpoint is represented by one or more [Elastic Network Interfaces](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-eni.html) in your subnets\. 

For more information, see [Interface VPC endpoints \(AWS PrivateLink\)](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html) in the *Amazon VPC User Guide*\. 

## Considerations for AWS CloudHSM VPC endpoints<a name="vpc-endpoint-considerations"></a>

Before you set up an interface VPC endpoint for AWS CloudHSM, ensure that you review [Interface endpoint properties and limitations](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#vpce-interface-limitations) in the *Amazon VPC User Guide*\. 
+ AWS CloudHSM supports making calls to all of its API actions from your VPC\. 

## Creating an interface VPC endpoint for AWS CloudHSM<a name="vpc-endpoint-create"></a>

You can create a VPC endpoint for the AWS CloudHSM service using either the Amazon VPC console or the AWS Command Line Interface \(AWS CLI\)\. For more information, see [Creating an interface endpoint](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#create-interface-endpoint) in the *Amazon VPC User Guide*\.

To create a VPC endpoint for AWS CloudHSM, use the following service name: 

```
com.amazonaws.region.cloudhsmv2
```

For example, in the US West \(Oregon\) Region \(`us-west-2`\), the service name would be:

```
com.amazonaws.us-west-2.cloudhsmv2
```

To make it easier to use the VPC endpoint, you can enable a [private DNS hostname](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#vpce-private-dns) for your VPC endpoint\. If you select the **Enable Private DNS Name** option, the standard AWS CloudHSM DNS hostname \(`https://cloudhsmv2.<region>.amazonaws.com`\) resolves to your VPC endpoint\.

This option makes it easier to use the VPC endpoint\. The AWS SDKs and AWS CLI use the standard AWS CloudHSM DNS hostname by default, so you do not need to specify the VPC endpoint URL in applications and commands\.

For more information, see [Accessing a service through an interface endpoint](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#access-service-though-endpoint) in the *Amazon VPC User Guide*\.

## Creating a VPC endpoint policy for AWS CloudHSM<a name="vpc-endpoint-policy"></a>

You can attach an endpoint policy to your VPC endpoint that controls access to AWS CloudHSM\. The policy specifies the following information:
+ The principal that can perform actions\.
+ The actions that can be performed\.
+ The resources on which actions can be performed\.

For more information, see [Controlling access to services with VPC endpoints](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints-access.html) in the *Amazon VPC User Guide*\. 

**Example: VPC endpoint policy for AWS CloudHSM actions**  
The following is an example of an endpoint policy for AWS CloudHSM\. When attached to an endpoint, this policy grants access to the listed AWS CloudHSM actions for all principals on all resources\.

```
{
   "Statement":[
      {
         "Principal":"*",
         "Effect":"Allow",
         "Action":[
            "cloudhsm:CopyBackupToRegion",
            "cloudhsm:CreateCluster",
            "cloudhsm:CreateHsm",
            "cloudhsm:DeleteBackup",
            "cloudhsm:DeleteCluster",
            "cloudhsm:DeleteHsm",
            "cloudhsm:DescribeBackups",
            "cloudhsm:DescribeClusters",
            "cloudhsm:InitializeCluster",
            "cloudhsm:ListTags",
            "cloudhsm:ModifyBackupAttributes",
            "cloudhsm:ModifyCluster",
            "cloudhsm:RestoreBackup",
            "cloudhsm:TagResource",
            "cloudhsm:UntagResource"
         ],
         "Resource":"*"
      }
   ]
}
```