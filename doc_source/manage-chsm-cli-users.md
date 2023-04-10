# Using CloudHSM CLI to manage users<a name="manage-chsm-cli-users"></a>

This topic provides step\-by\-step instruction on managing hardware security module \(HSM\) users with CloudHSM CLI\. For more information about CloudHSM CLI or HSM users, see [CloudHSM CLI](cloudhsm_cli.md) and [Using CloudHSM CLI](manage-hsm-users-chsm-cli.md)\.

**Topics**
+ [Understanding user management](#understand-cloudhsm-cli-users)
+ [Download CloudHSM CLI](#get-cli-users-cloudhsm-cli)
+ [How to](manage-users-cloudhsm-cli.md)

## Understanding HSM user management with CloudHSM CLI<a name="understand-cloudhsm-cli-users"></a>

 To manage HSM users, you must log in to the HSM with the user name and password of an [admin](manage-hsm-users-chsm-cli.md#admin)\. Only admins can manage users\. The HSM contains a default admin named admin\. You set the password for admin when you [activated the cluster](activate-cluster.md)\. 

 To use CloudHSM CLI, you must use the configure tool to update the local configuration\. For instructions on running the configure tool with CloudHSM CLI, see [Getting started with CloudHSM Command Line Interface \(CLI\)](cloudhsm_cli-getting-started.md)\. The `-a` parameter requires you to add the IP address of an HSM in your cluster\. If you have multiple HSMs, you can use any IP address\. This ensures CloudHSM CLI can propagate any changes you make across the entire cluster\. Remember that CloudHSM CLI uses its local file to track cluster information\. If the cluster has changed since the last time you used CloudHSM CLI from a particular host, you must add those changes to the local configuration file stored on that host\. Never remove a HSM while you're using CloudHSM CLI\. 

**To get an IP address for a HSM \(console\)**

1. Open the AWS CloudHSM console at [https://console\.aws\.amazon\.com/cloudhsm/home](https://console.aws.amazon.com/cloudhsm/home)\.

1. To change the AWS Region, use the Region selector in the upper\-right corner of the page\.

1. To open the cluster detail page, in the cluster table, choose the cluster ID\.

1. To get the IP address, on the HSMs tab, choose one of the IP addresses listed under **ENI IP address**\. 

**To get an IP address for a HSM \(AWS CLI\)**
+ Get the IP address of an HSM by using the [describe\-clusters](https://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/describe-clusters.html) command from the AWS CLI\. In the output from the command, the IP address of the HSMs are the values of `EniIp`\. 

  ```
  $  aws cloudhsmv2 describe-clusters
  	
  
  {
      "Clusters": [
          { ... }
              "Hsms": [
                  {
  ...
                      "EniIp": "10.0.0.9",
  ...
                  },
                  {
  ...
                      "EniIp": "10.0.1.6",
  ...
  ```

## Download CloudHSM CLI<a name="get-cli-users-cloudhsm-cli"></a>

The latest version of CloudHSM CLI is available for HSM user management tasks for Client SDK 5\. To download and install CloudHSM CLI, follow the instructions in [Install and configure CloudHSM CLI](gs_cloudhsm_cli-install.md)\.