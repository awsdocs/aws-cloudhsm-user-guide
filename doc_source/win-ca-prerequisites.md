# Windows Server CA step 1: Set up the prerequisites<a name="win-ca-prerequisites"></a>

To set up Windows Server as a certificate authority \(CA\) with AWS CloudHSM, you need the following:
+ An active AWS CloudHSM cluster with at least one HSM\.
+ An Amazon EC2 instance running a Windows Server operating system with the AWS CloudHSM client software for Windows installed\. This tutorial uses Microsoft Windows Server 2016\.
+ A cryptographic user \(CU\) to own and manage the CA's private key on the HSM\.

**To set up the prerequisites for a Windows Server CA with AWS CloudHSM**

1. Complete the steps in [Getting started](getting-started.md)\. When you launch the Amazon EC2 client, choose a Windows Server AMI\. This tutorial uses Microsoft Windows Server 2016\. When you complete these steps, you have an active cluster with at least one HSM\. You also have an Amazon EC2 client instance running Windows Server with the AWS CloudHSM client software for Windows installed\.

1. \(Optional\) Add more HSMs to your cluster\. For more information, see [Adding an HSM](add-remove-hsm.md#add-hsm)\.

1. Connect to your client instance\. For more information, see [Connect to Your Instance](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/EC2_GetStarted.html#ec2-connect-to-instance-windows) in the *Amazon EC2 User Guide for Windows Instances*\.

1. Create a crypto user \(CU\)using [Managing HSM users with CloudHSM CLI](manage-hsm-users-chsm-cli.md) or [Managing HSM users with CloudHSM Management Utility \(CMU\)](manage-hsm-users-cmu.md)\. Keep track of the CU user name and password\. You will need them to complete the next step\.

1. [Set the login credentials for the HSM](ksp-library-prereq.md), using the CU user name and password that you created in the previous step\.

1. In step 5, if you used Windows Credentials Manager to set HSM credentials, download [https://live.sysinternals.com/psexec.exe](https://live.sysinternals.com/psexec.exe) from SysInternals to run the following command as *NT Authority\\SYSTEM*:

   ```
   psexec.exe -s "C:\Program Files\Amazon\CloudHsm\tools\set_cloudhsm_credentials.exe" --username <username> --password <password>
   ```

   Replace *<username>* and *<password>* with the HSM credentials\.

To create a Windows Server CA with AWS CloudHSM, go to [Create Windows Server CA](win-ca-setup.md)\.