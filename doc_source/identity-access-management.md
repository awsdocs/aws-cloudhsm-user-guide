# Identity and access management for AWS CloudHSM<a name="identity-access-management"></a>

AWS uses security credentials to identify you and to grant you access to your AWS resources\. You can use features of AWS Identity and Access Management \(IAM\) to allow other users, services, and applications to use your AWS resources fully or in a limited way\. You can do this without sharing your security credentials\.

By default, IAM users don't have permission to create, view, or modify AWS resources\. To allow an IAM user to access resources such as a load balancer, and to perform tasks, you:

1. Create an IAM policy that grants the IAM user permission to use the specific resources and API actions they need\.

1. Attach the policy to the IAM user or the group that the IAM user belongs to\.

When you attach a policy to a user or group of users, it allows or denies the users permission to perform the specified tasks on the specified resources\.

For example, you can use IAM to create users and groups under your AWS account\. An IAM user can be a person, a system, or an application\. Then you grant permissions to the users and groups to perform specific actions on the specified resources using an IAM policy\.

## Grant permissions using IAM policies<a name="iam-policies"></a>

When you attach a policy to a user or group of users, it allows or denies the users permission to perform the specified tasks on the specified resources\.

An IAM policy is a JSON document that consists of one or more statements\. Each statement is structured as shown in the following example\.

```
{
  "Version": "2012-10-17",
  "Statement":[{
    "Effect": "effect",
    "Action": "action",
    "Resource": "resource-arn",
    "Condition": {
      "condition": {
        "key":"value"
      }
    }
  }]
}
```
+ **Effect**— The *effect* can be `Allow` or `Deny`\. By default, IAM users don't have permission to use resources and API actions, so all requests are denied\. An explicit allow overrides the default\. An explicit deny overrides any allows\.
+ **Action**— The *action* is the specific API action for which you are granting or denying permission\. For more information about specifying *action*, see [API actions for AWS CloudHSM](#api-actions)\.
+ **Resource**— The resource that's affected by the action\. AWS CloudHSM does not support resource\-level permissions\. You must use the \* wildcard to specify all AWS CloudHSM resources\.
+ **Condition**— You can optionally use conditions to control when your policy is in effect\. For more information, see [Condition keys for AWS CloudHSM](#condition-keys)\.

For more information, see the [IAM User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/)\.

## API actions for AWS CloudHSM<a name="api-actions"></a>

In the **Action** element of your IAM policy statement, you can specify any API action that AWS CloudHSM offers\. You must prefix the action name with the lowercase string `cloudhsm:`, as shown in the following example\.

```
"Action": "cloudhsm:DescribeClusters"
```

To specify multiple actions in a single statement, enclose them in square brackets and separate them with a comma, as shown in the following example\.

```
"Action": [
    "cloudhsm:DescribeClusters",
    "cloudhsm:DescribeHsm"
]
```

You can also specify multiple actions using the \* wildcard\. The following example specifies all API action names for AWS CloudHSM that start with `List`\.

```
"Action": "cloudhsm:List*"
```

To specify all API actions for AWS CloudHSM, use the \* wildcard, as shown in the following example\.

```
"Action": "cloudhsm:*"
```

For the list of API actions for AWS CloudHSM, see [AWS CloudHSM Actions](https://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_Operations.html)\.

## Condition keys for AWS CloudHSM<a name="condition-keys"></a>

When you create a policy, you can specify the conditions that control when the policy is in effect\. Each condition contains one or more key\-value pairs\. There are global condition keys and service\-specific condition keys\.

AWS CloudHSM has no service\-specific context keys\.

For more information about global condition keys, see [AWS Global Condition Context Keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html) in the *IAM User Guide*\.

## Predefined AWS managed policies for AWS CloudHSM<a name="predefined-policies"></a>

The managed policies created by AWS grant the required permissions for common use cases\. You can attach these policies to your IAM users, based on the access to AWS CloudHSM that they require:
+ **AWSCloudHSMFullAccess** — Grants full access required to use AWS CloudHSM features\.
+ **AWSCloudHSMReadOnlyAccess** — Grants read\-only access to AWS CloudHSM features\.

## Customer managed policies for AWS CloudHSM<a name="permissions-for-cloudhsm"></a>

We recommend that you create an IAM administrators group for AWS CloudHSM that contains only the permissions required to run AWS CloudHSM\. Attach the policy with the appropriate permissions to this group\. Add IAM users to the group as needed\. Each user that you add inherits the policy from the administrators group\.

Also, we recommend that you create additional user groups based on the permissions that your users need\. This ensures that only trusted users have access to critical API actions\. For example, you could create a user group that you use to grant read\-only access to clusters and HSMs\. Because this group does not allow a user to delete clusters or HSMs, an untrusted user cannot affect the availability of a production workload\.

As new AWS CloudHSM management features are added over time, you can ensure that only trusted users are given immediate access\. By assigning limited permissions to policies at creation, you can manually assign new feature permissions to them later\.

The following are example policies for AWS CloudHSM\. For information about how to create a policy and attach it to an IAM user group, see [Creating Policies on the JSON Tab](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html#access_policies_create-json-editor) in the *IAM User Guide*\.

**Examples**
+ [Read Only Permissions](#read-only-permissions)
+ [Power User Permissions](#power-user-permissions)
+ [Admin Permissions](#admin-permissions)

**Example: Read\-only permissions**  
This policy allows access to the `DescribeClusters` and `DescribeBackups` API actions\. It also includes additional permissions for specific Amazon EC2 API actions\. It does not allow the user to delete clusters or HSMs\.  

```
{
   "Version": "2012-10-17",
   "Statement": {
      "Effect": "Allow",
      "Action": [
         "cloudhsm:DescribeClusters",
         "cloudhsm:DescribeBackups",
         "cloudhsm:ListTags"
      ],
      "Resource": "*"
   }
}
```

**Example: Power user permissions**  
This policy allows access to a subset of the AWS CloudHSM API actions\. It also includes additional permissions for specific Amazon EC2 actions\. It does not allow the user to delete clusters or HSMs\. You must include the `iam:CreateServiceLinkedRole` action to allow AWS CloudHSM to automatically create the **AWSServiceRoleForCloudHSM** service\-linked role in your account\. This role allows AWS CloudHSM to log events\. For more information, see [Service\-linked roles for AWS CloudHSM](service-linked-roles.md)\.  

```
{
   "Version": "2012-10-17",
   "Statement": {
      "Effect": "Allow",
      "Action": [
         "cloudhsm:DescribeClusters",
         "cloudhsm:DescribeBackups",
         "cloudhsm:CreateCluster",
         "cloudhsm:CreateHsm",
         "cloudhsm:RestoreBackup",
         "cloudhsm:CopyBackupToRegion",
         "cloudhsm:InitializeCluster",
         "cloudhsm:ListTags",
         "cloudhsm:TagResource",
         "cloudhsm:UntagResource",
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
         "iam:CreateServiceLinkedRole"
      ],
      "Resource": "*"
   }
}
```

**Example: Admin permissions**  
This policy allows access to all AWS CloudHSM API actions, including the actions to delete HSMs and clusters\. It also includes additional permissions for specific Amazon EC2 actions\. You must include the `iam:CreateServiceLinkedRole` action to allow AWS CloudHSM to automatically create the **AWSServiceRoleForCloudHSM** service\-linked role in your account\. This role allows AWS CloudHSM to log events\. For more information, see [Service\-linked roles for AWS CloudHSM](service-linked-roles.md)\.  

```
{
   "Version":"2012-10-17",
   "Statement":{
      "Effect":"Allow",
      "Action":[
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
         "iam:CreateServiceLinkedRole"
      ],
      "Resource":"*"
   }
}
```