# AWS CloudHSM Command Line Tools<a name="command-line-tools"></a>

AWS CloudHSM provides command line tools for managing and using AWS CloudHSM\.

**Manage Clusters and HSMs**  
These tools get, create, delete, and tag CloudHSM clusters and HSMs\.  

+ [CloudHSMv2 commands in AWS Command Line Interface \(AWS CLI\)](http://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/index.html)\. To use these commands, you need to [install](http://docs.aws.amazon.com/cli/latest/userguide/installing.html) and [configure](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html#cli-quick-configuration) AWS CLI\.

+ HSM2 PowerShell cmdlets in the [AWSPowerShell module](https://aws.amazon.com/powershell/)\. These cmdlets are available in a Windows PowerShell module and a cross\-platform PowerShell Core module\.

   

**Manage Users**  
These tools create and delete HSM users, including implementing quorum authentication of user management tasks\.  

+ cloudhsm\_mgmt\_util\. This tool is included in the AWS CloudHSM client software\.

   

**Manage Keys**  
These tools create, delete, import, and export symmetric keys and asymmetric key pairs\.  

+ key\_mgmt\_util\. This tool is included in the AWS CloudHSM client software\.

   

**Helper Tools**  
These tools help you to use the tools and software libraries\.  

+ configure updates your CloudHSM client configuration files\.

+ pkpspeed measures the performance of your HSM hardware independent of software libraries\. 

   


+ [Getting Started with cloudhsm\_mgmt\_util](cloudhsm_mgmt_util.md)
+ [key\_mgmt\_util](key_mgmt_util.md)