# Getting started with CloudHSM Management Utility \(CMU\)<a name="cloudhsm_mgmt_util-getting-started"></a>

CloudHSM Management Utility \(CMU\) enables you to manage hardware security module \(HSM\) users\. Use this topic to get started with basic HSM user management tasks, such as creating users, listing users, and connecting CMU to the cluster\.

1. To use CMU, you must first use the configure tool to update the local CMU configuration with the `--cmu` parameter and an IP address from one of the HSMs in your cluster\. Do this each time you use CMU to ensure you're managing HSM users on every HSM in the cluster\.

------
#### [ Linux ]

   ```
   $ sudo /opt/cloudhsm/bin/configure --cmu <IP address>
   ```

------
#### [ Windows ]

   ```
   C:\Program Files\Amazon\CloudHSM\bin\ configure.exe --cmu <IP address>
   ```

------

1. Use the following command to start the CLI in interactive mode\.

------
#### [ Linux ]

   ```
   $ /opt/cloudhsm/bin/cloudhsm_mgmt_util /opt/cloudhsm/etc/cloudhsm_mgmt_util.cfg
   ```

------
#### [ Windows ]

   ```
   C:\Program Files\Amazon\CloudHSM> .\cloudhsm_mgmt_util.exe C:\ProgramData\Amazon\CloudHSM\data\cloudhsm_mgmt_util.cfg
   ```

------

   Output should be similar to the following depending on how many HSMs you have\.

   ```
   Connecting to the server(s), it may take time
   depending on the server(s) load, please wait...
   
   Connecting to server '10.0.2.9': hostname '10.0.2.9', port 2225...
   Connected to server '10.0.2.9': hostname '10.0.2.9', port 2225.
   
   Connecting to server '10.0.3.11': hostname '10.0.3.11', port 2225...
   Connected to server '10.0.3.11': hostname '10.0.3.11', port 2225.
   
   Connecting to server '10.0.1.12': hostname '10.0.1.12', port 2225...
   Connected to server '10.0.1.12': hostname '10.0.1.12', port 2225.
   ```

   The prompt changes to aws\-cloudhsm> when cloudhsm\_mgmt\_util is running\.

1. Use the loginHSM command to log in to the cluster\. Any type user can use this command to log in to the cluster\. 

   The command in the following example logs in *admin*, which is the default [crypto officer \(CO\)](manage-hsm-users-cmu.md#understanding-users-cmu)\. You set this user's password when you activated the cluster\. You can use the `-hpswd` parameter to hide your password\.

   ```
   aws-cloudhsm>loginHSM CO admin -hpswd
   ```

   The system prompts you for your password\. You enter the password, the system hides the password, and the output shows that the command was successful and that the you have connected to all the HSMs on the cluster\.

   ```
   Enter password:
   
   
   loginHSM success on server 0(10.0.2.9)
   loginHSM success on server 1(10.0.3.11)
   loginHSM success on server 2(10.0.1.12)
   ```

1.  Use listUsers to list all the users on the cluster\.

   ```
   aws-cloudhsm>listUsers
   ```

   CMU lists all the users on the cluster\.

   ```
   Users on server 0(10.0.2.9):
   Number of users found:2
   
       User Id             User Type       User Name                          MofnPubKey    LoginFailureCnt         2FA
            1              CO              admin                                    NO               0               NO
            2              AU              app_user                                 NO               0               NO
   Users on server 1(10.0.3.11):
   Number of users found:2
   
       User Id             User Type       User Name                          MofnPubKey    LoginFailureCnt         2FA
            1              CO              admin                                    NO               0               NO
            2              AU              app_user                                 NO               0               NO
   Users on server 2(10.0.1.12):
   Number of users found:2
   
       User Id             User Type       User Name                          MofnPubKey    LoginFailureCnt         2FA
            1              CO              admin                                    NO               0               NO
            2              AU              app_user                                 NO               0               NO
   ```

1.  Use createUser to create a CU user named **example\_user** with a password of **password1**\. 

   You use CU users in your applications to perform cryptographic and key management operations\. You can create CU users because in step 3 you logged in as a CO user\. Only CO users can perform user management tasks with CMU, such as creating and deleting users and changing the passwords of other users\.

   ```
   aws-cloudhsm>createUser CU example_user password1
   ```

   CMU prompts you about the create user operation\.

   ```
   *************************CAUTION********************************
   This is a CRITICAL operation, should be done on all nodes in the
   cluster. AWS does NOT synchronize these changes automatically with the
   nodes on which this operation is not executed or failed, please
   ensure this operation is executed on all nodes in the cluster.
   ****************************************************************
   
   Do you want to continue(y/n)?
   ```

1. To create the CU user **example\_user**, type **y**\.

1.  Use listUsers to list all the users on the cluster\. 

   ```
   aws-cloudhsm>listUsers
   ```

   CMU lists all the users on the cluster, including the new CU user you just created\.

   ```
   Users on server 0(10.0.2.9):
   Number of users found:3
   
       User Id             User Type       User Name                          MofnPubKey    LoginFailureCnt         2FA
            1              CO              admin                                    NO               0               NO
            2              AU              app_user                                 NO               0               NO
            3              CU              example_user                             NO               0               NO
   Users on server 1(10.0.3.11):
   Number of users found:3
   
       User Id             User Type       User Name                          MofnPubKey    LoginFailureCnt         2FA
            1              CO              admin                                    NO               0               NO
            2              AU              app_user                                 NO               0               NO
            3              CU              example_user                             NO               0               NO
   Users on server 2(10.0.1.12):
   Number of users found:3
   
       User Id             User Type       User Name                          MofnPubKey    LoginFailureCnt         2FA
            1              CO              admin                                    NO               0               NO
            2              AU              app_user                                 NO               0               NO
            3              CU              example_user                             NO               0               NO
   ```

1. Use the logoutHSM command to log out of the HSMs\.

   ```
   aws-cloudhsm>logoutHSM
   
   
   logoutHSM success on server 0(10.0.2.9)
   logoutHSM success on server 1(10.0.3.11)
   logoutHSM success on server 2(10.0.1.12)
   ```

1. Use the quit command to stop cloudhsm\_mgmt\_util\.

   ```
   aws-cloudhsm>quit
   
   
   disconnecting from servers, please wait...
   ```