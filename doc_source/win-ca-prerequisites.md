# Windows Server CA Step 1: Set Up the Prerequisites<a name="win-ca-prerequisites"></a>

To set up Windows Server as a certificate authority \(CA\) with AWS CloudHSM, you need the following:
+ An active AWS CloudHSM cluster with at least one HSM\.
+ An Amazon EC2 instance running a Windows Server operating system with the AWS CloudHSM client software for Windows installed\. This tutorial uses Microsoft Windows Server 2016\.
+ A cryptographic user \(CU\) to own and manage the CA's private key on the HSM\.

**To set up the prerequisites for a Windows Server CA with AWS CloudHSM**

1. Complete the steps in [Getting Started](getting-started.md)\. When you launch the Amazon EC2 client, choose a Windows Server AMI\. This tutorial uses Microsoft Windows Server 2016\. When you complete these steps, you have an active cluster with at least one HSM\. You also have an Amazon EC2 client instance running Windows Server with the AWS CloudHSM client software for Windows installed\.

1. \(Optional\) Add more HSMs to your cluster\. For more information, see [Adding an HSM](add-remove-hsm.md#add-hsm)\.

1. Connect to your client instance\. For more information, see [Connect to Your Instance](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/EC2_GetStarted.html#ec2-connect-to-instance-windows) in the *Amazon EC2 User Guide for Windows Instances*\.

1. To create a cryptographic user \(CU\) on your HSM, do the following:

   1. [Start the AWS CloudHSM client](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-start-cloudhsm-client)\.

   1. [Update the cloudhsm\_mgmt\_util configuration file](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-update-configuration)\.

   1. [Start cloudhsm\_mgmt\_util](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-start)\.

   1. [Enable end\-to\-end encryption](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-enable_e2e)\.

   1. [Log in to the HSMs](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-log-in) with the user name and password of a crypto officer \(CO\)\.

   1. [Create a crypto user \(CU\)](manage-hsm-users.md#create-user)\. Keep track of the CU user name and password\. You will need them to complete the next step\.

1. [Set the Windows system environment variables](ksp-library-prereq.md), using the CU user name and password that you created in the previous step\.

To create a Windows Server CA with AWS CloudHSM, go to [Create Windows Server CA](win-ca-setup.md)\.