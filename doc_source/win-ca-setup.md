# Windows Server CA Step 2: Create a Windows Server CA with AWS CloudHSM<a name="win-ca-setup"></a>

To create a Windows Server CA, you add the Active Directory Certificate Services \(AD CS\) role to your Windows Server\. When you add this role, you use an AWS CloudHSM key storage provider \(KSP\) to create and store the CA's private key on your AWS CloudHSM cluster\.

**Note**  
When you create your Windows Server CA, you can choose to create a root CA or a subordinate CA\. You typically make this decision based on the design of your public key infrastructure and the security policies of your organization\. This tutorial explains how to create a root CA for simplicity\.

**To add the AD CS role to your Windows Server and create the CA's private key**

1. If you haven't already done so, connect to your Windows server\. For more information, see [Connect to Your Instance](http://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/EC2_GetStarted.html#ec2-connect-to-instance-windows) in the *Amazon EC2 User Guide for Windows Instances*\.

1. On your Windows server, start **Server Manager**\.

1. In the **Server Manager** dashboard, choose **Add roles and features**\.

1. Read the **Before you begin** information, and then choose **Next**\.

1. For **Installation Type**, choose **Role\-based or feature\-based installation**\. Then choose **Next**\.

1. For **Server Selection**, choose **Select a server from the server pool**\. Then choose **Next**\.

1. For **Server Roles**, do the following:

   1. Select **Active Directory Certificate Services**\.

   1. For **Add features that are required for Active Directory Certificate Services**, choose **Add Features**\.

   1. Choose **Next** to finish selecting server roles\.

1. For **Features**, accept the defaults, and then choose **Next**\.

1. For **AD CS**, do the following:

   1. Choose **Next**\.

   1. Select **Certification Authority**, and then choose **Next**\.

1. For **Confirmation**, read the confirmation information, and then choose **Install**\. Do not close the window\.

1. Choose the highlighted **Configure Active Directory Certificate Services on the destination server** link\.

1. For **Credentials**, verify or change the credentials displayed\. Then choose **Next**\.

1. For **Role Services**, select **Certification Authority**\. Then choose **Next**\.

1. For **Setup Type**, select **Standalone CA**\. Then choose **Next**\.

1. For **CA Type**, select **Root CA**\. Then choose **Next**\.
**Note**  
You can choose to create a root CA or a subordinate CA based on the design of your public key infrastructure and the security policies of your organization\. This tutorial explains how to create a root CA for simplicity\.

1. For **Private Key**, select **Create a new private key**\. Then choose **Next**\.

1. For **Cryptography**, do the following:

   1. For **Select a cryptographic provider**, choose one of the **Cavium Key Storage Provider** options from the menu\. These are the AWS CloudHSM key storage providers\. For example, you can choose **RSA\#Cavium Key Storage Provider**\.

   1. For **Key length**, choose one of the key length options\.

   1. For **Select the hash algorithm for signing certificates issued by this CA**, choose one of the hash algorithm options\.

   Choose **Next**\.

1. For **CA Name**, do the following:

   1. \(Optional\) Edit the common name\.

   1. \(Optional\) Type a distinguished name suffix\.

   Choose **Next**\.

1. For **Validity Period**, specify a time period in years, months, weeks, or days\. Then choose **Next**\.

1. For **Certificate Database**, you can accept the default values, or optionally change the location for the database and the database log\. Then choose **Next**\.

1. For **Confirmation**, review the information about your CA; Then choose **Configure**\.

1. Choose **Close**, and then choose **Close** again\.

You now have a Windows Server CA with AWS CloudHSM\. To learn how to sign a certificate signing request \(CSR\) with your CA, go to [Sign a CSR](win-ca-sign-csr.md)\.