# Step 1: Set up the prerequisites<a name="ssl-offload-prerequisites-windows"></a>

To set up web server SSL/TLS offload with AWS CloudHSM, you need the following:
+ An active AWS CloudHSM cluster with at least one HSM\.
+ An Amazon EC2 instance running a Windows operating system with the following software installed:
  + The AWS CloudHSM client software for Windows\.
  + Internet Information Services \(IIS\) for Windows Server\.
+ A [crypto user](manage-hsm-users.md#crypto-user) \(CU\) to own and manage the web server's private key on the HSM\.

**Note**  
This tutorial uses Microsoft Windows Server 2016\. Microsoft Windows Server 2012 is also supported, but Microsoft Windows Server 2012 R2 is not\.

**To set up a Windows Server instance and create a CU on the HSM**

1. Complete the steps in [Getting started](getting-started.md)\. When you launch the Amazon EC2 client, choose a Windows Server 2016 or Windows Server 2012 AMI\. When you complete these steps, you have an active cluster with at least one HSM\. You also have an Amazon EC2 client instance running Windows Server with the AWS CloudHSM client software for Windows installed\.

1. \(Optional\) Add more HSMs to your cluster\. For more information, see [Adding an HSM](add-remove-hsm.md#add-hsm)\.

1. Connect to your Windows server\. For more information, see [Connect to Your Instance](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/EC2_GetStarted.html#ec2-connect-to-instance-windows) in the *Amazon EC2 User Guide for Windows Instances*\.

1. [Create a crypto user \(CU\)](cli-users.md#manage-users)\. Keep track of the CU user name and password\. You will need them to complete the next step\.

1. [Set the login credentials for the HSM](ksp-library-prereq.md), using the CU user name and password that you created in the previous step\.

1. In step 5, if you used Windows Credentials Manager to set HSM credentials, download [https://live.sysinternals.com/psexec.exe](https://live.sysinternals.com/psexec.exe) from SysInternals to run the following command as *NT Authority\\SYSTEM*:

   ```
   psexec.exe -s "C:\Program Files\Amazon\CloudHsm\tools\set_cloudhsm_credentials.exe" --username <username> --password <password>
   ```

   Replace *<username>* and *<password>* with the HSM credentials\.

**To install IIS on your Windows Server**

1. If you haven't already done so, connect to your Windows server\. For more information, see [Connect to Your Instance](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/EC2_GetStarted.html#ec2-connect-to-instance-windows) in the *Amazon EC2 User Guide for Windows Instances*\.

1. On your Windows server, start **Server Manager**\.

1. In the **Server Manager** dashboard, choose **Add roles and features**\.

1. Read the **Before you begin** information, and then choose **Next**\.

1. For **Installation Type**, choose **Role\-based or feature\-based installation**\. Then choose **Next**\.

1. For **Server Selection**, choose **Select a server from the server pool**\. Then choose **Next**\.

1. For **Server Roles**, do the following:

   1. Select **Web Server \(IIS\)**\.

   1. For **Add features that are required for Web Server \(IIS\)**, choose **Add Features**\.

   1. Choose **Next** to finish selecting server roles\.

1. For **Features**, accept the defaults\. Then choose **Next**\.

1. Read the **Web Server Role \(IIS\)** information\. Then choose **Next**\.

1. For **Select role services**, accept the defaults or change the settings as preferred\. Then choose **Next**\.

1. For **Confirmation**, read the confirmation information\. Then choose **Install**\.

1. After the installation is complete, choose **Close**\.

After you complete these steps, go to [Step 2: Create a certificate signing request \(CSR\) and certificate](ssl-offload-windows-create-csr-and-certificate.md)\.