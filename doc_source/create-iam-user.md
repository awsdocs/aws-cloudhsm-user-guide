# Create IAM administrative groups<a name="create-iam-user"></a>

As a [best practice](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#create-iam-users), don't use your AWS account root user to interact with AWS, including AWS CloudHSM\. Instead, use AWS Identity and Access Management \(IAM\) to create an IAM user, IAM role, or federated user\. Follow the steps in the [Create an IAM user and administrator group](#create-iam-admin) section to create an administrator group and attach the **AdministratorAccess** policy to it\. Then create a new administrator user and add the user to the group\. Add additional users to the group as needed\. Each user you add inherits the **AdministratorAccess** policy from the group\. 

Another best practice is to create an AWS CloudHSM administrator group that has only the permissions required to run AWS CloudHSM\. Add individual users to this group as needed\. Each user inherits the limited permissions that are attached to the group rather than full AWS access\. The [Customer managed policies for AWS CloudHSM](identity-access-management.md#permissions-for-cloudhsm) section that follows contains the policy that you should attach to your AWS CloudHSM administrator group\. 

AWS CloudHSM defines an [service–linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-linked-role) for your AWS account\. The service–linked role currently defines permissions that allow your account to log AWS CloudHSM events\. The role can be created automatically by AWS CloudHSM or manually by you\. You cannot edit the role, but you can delete it\. For more information, see [Service\-linked roles for AWS CloudHSM](service-linked-roles.md)\.

## Create an IAM user and administrator group<a name="create-iam-admin"></a>

Start by creating an IAM user along with an administrator group for that user\.

**To create an administrator user for yourself and add the user to an administrators group \(console\)**

1. Sign in to the [IAM console](https://console.aws.amazon.com/iam/) as the account owner by choosing **Root user** and entering your AWS account email address\. On the next page, enter your password\.
**Note**  
We strongly recommend that you adhere to the best practice of using the **Administrator** IAM user that follows and securely lock away the root user credentials\. Sign in as the root user only to perform a few [account and service management tasks](https://docs.aws.amazon.com/general/latest/gr/aws_tasks-that-require-root.html)\.

1. In the navigation pane, choose **Users** and then choose **Add users**\.

1. For **User name**, enter **Administrator**\.

1. Select the check box next to **AWS Management Console access**\. Then select **Custom password**, and then enter your new password in the text box\.

1. \(Optional\) By default, AWS requires the new user to create a new password when first signing in\. You can clear the check box next to **User must create a new password at next sign\-in** to allow the new user to reset their password after they sign in\.

1. Choose **Next: Permissions**\.

1. Under **Set permissions**, choose **Add user to group**\.

1. Choose **Create group**\.

1. In the **Create group** dialog box, for **Group name** enter **Administrators**\.

1. Choose **Filter policies**, and then select **AWS managed \- job function** to filter the table contents\.

1. In the policy list, select the check box for **AdministratorAccess**\. Then choose **Create group**\.
**Note**  
You must activate IAM user and role access to Billing before you can use the `AdministratorAccess` permissions to access the AWS Billing and Cost Management console\. To do this, follow the instructions in [step 1 of the tutorial about delegating access to the billing console](https://docs.aws.amazon.com/IAM/latest/UserGuide/tutorial_billing.html)\.

1. Back in the list of groups, select the check box for your new group\. Choose **Refresh** if necessary to see the group in the list\.

1. Choose **Next: Tags**\.

1. \(Optional\) Add metadata to the user by attaching tags as key\-value pairs\. For more information about using tags in IAM, see [Tagging IAM entities](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_tags.html) in the *IAM User Guide*\.

1. Choose **Next: Review** to see the list of group memberships to be added to the new user\. When you are ready to proceed, choose **Create user**\.

You can use this same process to create more groups and users and to give your users access to your AWS account resources\. To learn about using policies that restrict user permissions to specific AWS resources, see [Access management](https://docs.aws.amazon.com/IAM/latest/UserGuide/access.html) and [Example policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_examples.html)\.

For example policies for AWS CloudHSM that you can attach to your IAM user group, see [Identity and access management for AWS CloudHSM](identity-access-management.md)\.