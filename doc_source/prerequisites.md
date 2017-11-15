# Getting Started with AWS CloudHSM: Set up the Necessary Prerequisites<a name="prerequisites"></a>

Complete the steps in the following topics to set up your AWS environment for use with AWS CloudHSM\.


+ [Create an IAM User](#create-iam-user)
+ [Create a Virtual Private Cloud \(VPC\)](#create-vpc)
+ [Create Private Subnets](#create-subnets)

## Create an IAM User<a name="create-iam-user"></a>

As a [best practice](http://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#create-iam-users), don't use your AWS account root user to interact with AWS, including AWS CloudHSM\. Instead, use AWS Identity and Access Management \(IAM\) to create an IAM user, IAM role, or federated user\. If you don't have one already, complete the following steps to create an IAM user for yourself and give that user administrative permissions\.

**To create an IAM user for yourself and add the user to an Administrators group**

1. Use your AWS account email address and password to sign in to the [AWS Management Console](https://console.aws.amazon.com/) as the *[AWS account root user](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_root-user.html)*\.

1. In the navigation pane of the console, choose **Users**, and then choose **Add user**\.

1. For **User name**, type ** Administrator**\.

1. Select the check box next to **AWS Management Console access**, select **Custom password**, and then type the new user's password in the text box\. You can optionally select **Require password reset** to force the user to select a new password the next time the user signs in\.

1. Choose **Next: Permissions**\.

1. On the **Set permissions for user** page, choose **Add user to group**\.

1. Choose **Create group**\.

1. In the **Create group** dialog box, type ** Administrators**\.

1. For **Filter**, choose **Job function**\.

1. In the policy list, select the check box for ** AdministratorAccess**\. Then choose **Create group**\.

1. Back in the list of groups, select the check box for your new group\. Choose **Refresh** if necessary to see the group in the list\.

1. Choose **Next: Review** to see the list of group memberships to be added to the new user\. When you are ready to proceed, choose **Create user**\.

You can use this same process to create more groups and users, and to give your users access to your AWS account resources\. To learn about using policies to restrict users' permissions to specific AWS resources, go to [Access Management](http://docs.aws.amazon.com/IAM/latest/UserGuide/access.html) and [Example Policies](http://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_examples.html)\.

To sign in to the AWS Management Console with your IAM user, you need your AWS account ID or alias\. To get these items, see [Your AWS Account ID and Its Alias](http://docs.aws.amazon.com/IAM/latest/UserGuide/console_account-alias.html) in the *IAM User Guide*\.

### Restrict User Permissions to What's Necessary for AWS CloudHSM<a name="permissions-for-cloudhsm"></a>

The steps in the preceding section explain how to create an IAM user with administrative permissions in your AWS account\. To restrict the user's permissions to only those necessary for using AWS CloudHSM, remove the user from the AdministratorAccess group\. Then add the user to a group that you created for AWS CloudHSM administrators\. The following example policy contains the minimum set of permissions that AWS CloudHSM administrators need to manage AWS CloudHSM resources\.

These permissions include full access to the AWS CloudHSM API and also some additional permissions in Amazon Elastic Compute Cloud \(Amazon EC2\)\. When you perform certain actions with the AWS CloudHSM console or API, AWS CloudHSM takes additional actions on your behalf to manage certain Amazon EC2 resources\. For example, this can occur when you create and delete clusters and HSMs\.

```
{
    "Version": "2012-10-17",
    "Statement": {
        "Effect": "Allow",
        "Action": [
            "cloudhsm:*",
            "ec2:CreateNetworkInterface",
            "ec2:DescribeNetworkInterfaces",
            "ec2:DescribeNetworkInterfaceAttribute",
            "ec2:DetachNetworkInterface",
            "ec2:DeleteNetworkInterface",
            "ec2:CreateSecurityGroup",
            "ec2:AuthorizeSecurityGroupIngress",
            "ec2:AuthorizeSecurityGroupEgress",
            "ec2:RevokeSecurityGroupEgress",
            "ec2:DescribeSecurityGroups",
            "ec2:DeleteSecurityGroup",
            "ec2:CreateTags",
            "ec2:DescribeVpcs",
            "ec2:DescribeSubnets",
            "ec2:CreateServiceLinkedRole"
        ],
        "Resource": "*"
    }
}
```

## Create a Virtual Private Cloud \(VPC\)<a name="create-vpc"></a>

If you don't already have an Amazon Virtual Private Cloud \(VPC\), create one now\.

**To create a VPC**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. On the navigation bar, use the region selector to choose one of the [AWS Regions where AWS CloudHSM is currently supported](http://docs.aws.amazon.com/general/latest/gr/rande.html#cloudhsm_region)\.

1. Choose **Start VPC Wizard**\.

1. Choose the first option, **VPC with a Single Public Subnet**\. Then choose **Select**\.

1. For **VPC name:**, type an identifiable name such as **CloudHSM**\. For **Subnet name:**, type an identifiable name such as **CloudHSM public subnet**\. Leave all other options set to their defaults\. Then choose **Create VPC**\. After the VPC is created, choose **OK**\.

## Create Private Subnets<a name="create-subnets"></a>

Create a private subnet \(a subnet with no internet gateway attached\) for each Availability Zone where you want to create an HSM\. Creating a private subnet in each Availability Zone provides the most robust configuration for high availability\.

**To create the private subnets in your VPC**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Subnets**\. Then choose **Create Subnet**\.

1. In the **Create Subnet** dialog box, do the following:

   1. For **Name tag**, type an identifiable name such as **CloudHSM private subnet**\.

   1. For **VPC**, choose the VPC that you created previously\.

   1. For **Availability Zone**, choose the first Availability Zone in the list\.

   1. For **CIDR block**, type the CIDR block to use for the subnet\. If you used the default values for the VPC in the previous procedure, then type **10\.0\.1\.0/28**\.

   Choose **Yes, Create**\.

1. Repeat steps 2 and 3 to create subnets for each remaining Availability Zone in the region\. For the subnet CIDR blocks, you can use 10\.0\.2\.0/28, 10\.0\.3\.0/28, and so on\.

After you create a VPC and private subnets, proceed to [Create a Cluster and HSM](create-cluster-and-hsm.md)\.