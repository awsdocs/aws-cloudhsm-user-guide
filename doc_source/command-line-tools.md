# AWS CloudHSM command line tools<a name="command-line-tools"></a>

This topic describes the command line tools available for managing and using AWS CloudHSM\.

**Topics**
+ [Understanding command line tools](#command-line-tools-intro)
+ [Configure tool](configure-tool.md)
+ [CloudHSM Command Line Interface \(CLI\)](cloudhsm_cli.md)
+ [CloudHSM Management Utility \(CMU\)](cloudhsm_mgmt_util.md)
+ [Key Management Utility \(KMU\)](key_mgmt_util.md)

## Understanding command line tools<a name="command-line-tools-intro"></a>

In addition to the AWS command\-line interface \(AWS CLI\) that you use for managing your AWS resources, AWS CloudHSM offers command\-line tools for managing HSM users and creating and managing keys on the HSM\. In AWS CloudHSM you use the familiar AWS CLI to manage your cluster, and the CloudHSM command line tools to manage your HSM\.

These are the various command\-line tools:

**To manage HSMs and clusters**  
[CloudHSMv2 commands in AWS CLI](https://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/index.html) and [HSM2 PowerShell cmdlets in the AWSPowerShell module](https://aws.amazon.com/powershell/)  
+ These tools get, create, delete, and tag AWS CloudHSM clusters and HSMs:
+ To use the commands in [CloudHSMv2 commands in AWS CLI](https://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/index.html), you need to [install](https://docs.aws.amazon.com/cli/latest/userguide/installing.html) and [configure](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html#cli-quick-configuration) AWS CLI\.
+ [HSM2 PowerShell cmdlets in the AWSPowerShell module](https://aws.amazon.com/powershell/) are available in a Windows PowerShell module and a cross\-platform PowerShell Core module\.

   

**To manage HSM users**  
[CloudHSM CLI](cloudhsm_cli.md)  
+ Use [this tool](cloudhsm_cli.md) to create users, delete users, list users, change user passwords, and update user multifactor authentication \(MFA\)\. It is not included in the AWS CloudHSM client software\. For guidance on installing this tool, see [Install and configure CloudHSM CLI](gs_cloudhsm_cli-install.md)\.

   

**Helper Tools**  
Two tools help you to use AWS CloudHSM tools and software libraries:  
+ The [configure tool](configure-tool.md) updates your CloudHSM client configuration files\. This enables the AWS CloudHSM to synchronize the HSMs in a cluster\.

  AWS CloudHSM offers two major versions, and Client SDK 5 is the latest\. It offers a variety of advantages over Client SDK 3 \(the previous series\)\./>\. 
+ [pkpspeed](troubleshooting-verify-hsm-performance.md) measures the performance of your HSM hardware independent of software libraries\. 

   

**Tools for previous SDKs**  
Use the key management tool \(KMU\) create, delete, import, and export symmetric keys and asymmetric key pairs:  
+ [key\_mgmt\_util](key_mgmt_util.md)\. This tool is included in the AWS CloudHSM client software\.

   
Use the CloudHSM management tool \(CMU\) to create and delete HSM users, including implementing quorum authentication of user management tasks  
+ [cloudhsm\_mgmt\_util](cloudhsm_mgmt_util.md)\. This tool is included in the AWS CloudHSM client software\.

   