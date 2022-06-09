# Activate the cluster<a name="activate-cluster"></a>

When you activate an AWS CloudHSM cluster, the cluster's state changes from initialized to active\. You can then [manage the hardware security module \(HSM\) users](manage-hsm-users.md) and [use the HSM](use-hsm.md)\. 

To activate the cluster, log in to the HSM with the credentials of the [precrypto officer \(PRECO\)](manage-hsm-users.md#understanding-users)\. This a temporary user that exists only on the first HSM in an AWS CloudHSM cluster\. The first HSM in a new cluster contains a PRECO user with a default user name and password\. When you change the password, the PRECO user becomes a crypto officer \(CO\)\.

**To activate a cluster**

1. Connect to the client instance that you launched in previously\. For more information, see [Launch an Amazon EC2 client instance](launch-client-instance.md)\. You can launch a Linux instance or a Windows Server\. 

1. Use the following command to start the CloudHSM Management Utility \(CMU\) command line utility\.

------
#### [ Linux ]

   ```
   $ /opt/cloudhsm/bin/cloudhsm_mgmt_util /opt/cloudhsm/etc/cloudhsm_mgmt_util.cfg
   ```

------
#### [ Windows ]

   ```
   C:\Program Files\Amazon\CloudHSM>cloudhsm_mgmt_util.exe C:\ProgramData\Amazon\CloudHSM\data\cloudhsm_mgmt_util.cfg
   ```

------

1. \(Optional\) Use the listUsers command to display the existing users\.

   ```
   aws-cloudhsm>listUsers
   Users on server 0(server1):
   Number of users found:2
   
       User Id             User Type       User Name                          MofnPubKey    LoginFailureCnt         2FA
            1              PRECO           admin                                    NO               0               NO
            2              AU              app_user                                 NO               0               NO
   ```

1. Use the loginHSM command to log in to the HSM as the PRECO user\. This is a temporary user that exists on the first HSM in your cluster\. 

   ```
   aws-cloudhsm>loginHSM PRECO admin password
   
   loginHSM success on server 0(server1)
   ```

1. Use the changePswd command to change the password for the PRECO user\. When you change the password, the PRECO user becomes a crypto officer \(CO\)\. 

   ```
   aws-cloudhsm>changePswd PRECO admin <NewPassword>
   
   *************************CAUTION********************************
   This is a CRITICAL operation, should be done on all nodes in the
   cluster. AWS does NOT synchronize these changes automatically with the
   nodes on which this operation is not executed or failed, please
   ensure this operation is executed on all nodes in the cluster.
   ****************************************************************
   
   Do you want to continue(y/n)?y
   Changing password for admin(PRECO) on 1 nodes
   ```

   We recommend that you write down the new password on a password worksheet\. Do not lose the worksheet\. We recommend that you print a copy of the password worksheet, use it to record your critical HSM passwords, and then store it in a secure place\. We also recommended that you store a copy of this worksheet in secure off\-site storage\. 

1. \(Optional\) Use the listUsers command to verify that the user's type changed to [crypto officer \(CO\)](manage-hsm-users.md#crypto-officer)\. 

   ```
   aws-cloudhsm>listUsers
   Users on server 0(server1):
   Number of users found:2
   
       User Id             User Type       User Name                          MofnPubKey    LoginFailureCnt         2FA
            1              CO              admin                                    NO               0               NO
            2              AU              app_user                                 NO               0               NO
   ```

1. Use the quit command to stop the cloudhsm\_mgmt\_util tool\.

   ```
   aws-cloudhsm>quit
   ```

For more information about working with CMU, see [Understanding HSM Users](manage-hsm-users.md#understanding-users) and [Understanding HSM User Management with CMU](cli-users.md#understand-users)\.