# Service\-Linked Roles for AWS CloudHSM<a name="service-linked-roles"></a>

The IAM policy that you created previously to [Customer Managed Policies for AWS CloudHSM](identity-access-management.md#permissions-for-cloudhsm) includes the `iam:CreateServiceLinkedRole` action\. AWS CloudHSM defines a [service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-linked-role) named **AWSServiceRoleForCloudHSM**\. The role is predefined by AWS CloudHSM and includes permissions that AWS CloudHSM requires to call other AWS services on your behalf\. The role makes setting up your service easier because you don’t need to manually add the role policy and trust policy permissions\.

The role policy allows AWS CloudHSM to create Amazon CloudWatch Logs log groups and log streams and write log events on your behalf\. You can view it below and in the IAM console\. 

```
{
    "Version": "2018-06-12",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "logs:CreateLogGroup",
                "logs:CreateLogStream",
                "logs:PutLogEvents",
                "logs:DescribeLogStreams"
            ],
            "Resource": [
                "arn:aws:logs:*:*:*"
            ]
        }
    ]
}
```

The trust policy for the **AWSServiceRoleForCloudHSM** role allows AWS CloudHSM to assume the role\.

```
{
  "Version": "2018-06-12",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "cloudhsm.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

## Creating a Service\-Linked Role \(Automatic\)<a name="create-slr-auto"></a>

AWS CloudHSM creates the **AWSServiceRoleForCloudHSM** role when you create a cluster if you include the `iam:CreateServiceLinkedRole` action in the permissions that you defined when you created the AWS CloudHSM administrators group\. See [Customer Managed Policies for AWS CloudHSM](identity-access-management.md#permissions-for-cloudhsm)\. 

If you already have one or more clusters and just want to add the **AWSServiceRoleForCloudHSM** role, you can use the console, the [create\-cluster](https://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/create-cluster.html) command, or the [CreateCluster](AWS CloudHSM API ReferenceAPI_CreateCluster.html) API operation to create a cluster\. Then use the console, the [delete\-cluster](https://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/delete-cluster.html) command, or the [DeleteCluster](AWS CloudHSM API ReferenceAPI_DeleteCluster.html) API operation to delete it\. Creating the new cluster creates the service\-linked role and applies it to all clusters in your account\. Alternatively, you can create the role manually\. See the following section for more information\. 

**Note**  
You do not need to perform all of the steps outlined in [Getting Started with AWS CloudHSM](getting-started.md) to create a cluster if you are only creating it to add the **AWSServiceRoleForCloudHSM** role\.

## Creating a Service\-Linked Role \(Manual\)<a name="create-slr-manual"></a>

You can use the IAM console, AWS CLI, or API to create the **AWSServiceRoleForCloudHSM** role\. For more information, see [Creating a Service\-Linked Role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#create-service-linked-role) in the *IAM User Guide*\. 

## Editing the Service\-Linked Role<a name="edit-slr"></a>

AWS CloudHSM does not allow you to edit the **AWSServiceRoleForCloudHSM** role\. After the role is created, for example, you cannot change its name because various entities might reference the role by name\. Also, you cannot change the role policy\. You can, however, use IAM to edit the role description\. For more information, see [Editing a Service–Linked Role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#edit-service-linked-role) in the *IAM User Guide*\.

## Deleting the Service\-Linked Role<a name="delete-slr"></a>

You cannot delete a service\-linked role as long as a cluster to which it has been applied still exists\. To delete the role, you must first delete each HSM in your cluster and then delete the cluster\. Every cluster in your account must be deleted\. You can then use the IAM console, AWS CLI, or API to delete the role\. For more information about deleting a cluster, see [Deleting an AWS CloudHSM Cluster](delete-cluster.md)\. For more information, see [Deleting a Service\-Linked Role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#delete-service-linked-role) in the *IAM User Guide*\.