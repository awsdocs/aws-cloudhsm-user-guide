# Oracle TDE with AWS CloudHSM: Set Up the Prerequisites<a name="oracle-tde-prerequisites"></a>

To accomplish Oracle TDE integration with AWS CloudHSM, you need the following:

+ An active AWS CloudHSM cluster with at least one HSM\.

+ An Amazon EC2 instance running the Amazon Linux operating system with the following software installed:

  + The AWS CloudHSM client and command line tools\.

  + The AWS CloudHSM software library for PKCS \#11\.

  + Oracle Database\. AWS CloudHSM supports Oracle TDE integration with Oracle Database versions 11 and 12\.

+ A crypto user \(CU\) to own and manage the TDE master encryption key on the HSMs in your cluster\.

Complete the following steps to set up all of the prerequisites\.

**To set up the prerequisites for Oracle TDE integration with AWS CloudHSM**

1. Complete the steps in [Getting Started: Create A Cluster](getting-started.md)\. After you complete these steps, you have an active cluster with one HSM\. You also have an Amazon EC2 instance, known as a *client instance*, running the Amazon Linux operating system and with the AWS CloudHSM client and command line tools installed and configured\.

1. \(Optional\) Add more HSMs to your cluster\. For more information, see [Adding an HSM](add-remove-hsm.md#add-hsm)\.

1. Connect to the client instance that you created previously\. On the client instance, do the following:

   1. Install the AWS CloudHSM software library for PKCS \#11\.

   1. Install Oracle Database\. For more information, see the [Oracle Database documentation](https://docs.oracle.com/en/database/)\. AWS CloudHSM supports Oracle TDE integration with Oracle Database versions 11 and 12\.

   1. Start the AWS CloudHSM client\.

   1. Update the configuration file for the command line tool known as cloudhsm\_mgmt\_util\.

   1. Use the command line tool known as cloudhsm\_mgmt\_util to create a crypto user \(CU\) on your cluster\. For more information, see [Managing HSM Users](manage-hsm-users.md)\.

After you complete these steps, you can [Configure the Database and Generate the Master Encryption Key](oracle-tde-configure-database-and-generate-master-key.md)\.