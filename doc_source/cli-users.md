# Using CloudHSM Management Utility \(CMU\) to manage users<a name="cli-users"></a>

This topic provides step\-by\-step instruction on managing hardware security module \(HSM\) users with CloudHSM Management Utility \(CMU\), a command line tool that comes with the Client SDK\. For more information about CMU or HSM users, see [CloudHSM Management Utility](cloudhsm_mgmt_util.md) and [Understanding HSM users](manage-hsm-users.md#understanding-users)\.

**Topics**
+ [Understanding user management](#understand-users)
+ [Download CloudHSM Management Utility](#get-cli-users)
+ [How to manage users](#manage-users)

## Understanding HSM user management with CMU<a name="understand-users"></a>

 To manage HSM users, you must log in to the HSM with the user name and password of a [cryptographic officer](manage-hsm-users.md#crypto-officer) \(CO\)\. Only COs can manage users\. The HSM contains a default CO named admin\. You set the password for admin when you [activated the cluster](activate-cluster.md)\. 

 To use CMU, you must use the configure tool to update the local configuration\. CMU creates its own connection to the cluster and this connection is *not* cluster aware\. To track cluster information, CMU maintains a local configuration file\. This means that *each time* you use CMU, you should first update the configuration file by running the [configure](configure-tool.md) command line tool with the `--cmu` parameter\. If you are using Client SDK 3\.2\.1 or earlier, you must use a different parameter than `--cmu`\. For more information, see [Using CMU with Client SDK 3\.2\.1 and earlier](#downlevel-cmu)\. 

 The `--cmu` parameter requires you to add the IP address of an HSM in your cluster\. If you have multiple HSMs, you can use any IP address\. This ensures CMU can propagate any changes you make across the entire cluster\. Remember that CMU uses its local file to track cluster information\. If the cluster has changed since the last time you used CMU from a particular host, you must add those changes to the local configuration file stored on that host\. Never add or remove a HSM while you're using CMU\. 

**To get an IP address for a HSM \(console\)**

1. Open the AWS CloudHSM console at [https://console\.aws\.amazon\.com/cloudhsm/](https://console.aws.amazon.com/cloudhsm/)\.

1. To change the AWS Region, use the Region selector in the upper\-right corner of the page\.

1. In the navigation pane, choose **Clusters**\.

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

### Using CMU with Client SDK 3\.2\.1 and earlier<a name="downlevel-cmu"></a>

With Client SDK 3\.3\.0, AWS CloudHSM added support for the `--cmu` parameter, which simplifies the process of updating the configuration file for CMU\. If you're using a version of CMU from Client SDK 3\.2\.1 or earlier, you must continue to use the `-a` and `-m` parameters to update the configuration file\. For more information about these parameters, see [Configure Tool](configure-tool.md)\.

## Download CloudHSM Management Utility<a name="get-cli-users"></a>

The latest version of CMU is available for HSM user management tasks whether you are using Client SDK 5 and Client SDK 3\. 

**To download and install CMU**
+ Download and install CMU\.

------
#### [ Amazon Linux ]

  ```
  $ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL6/cloudhsm-mgmt-util-latest.el6.x86_64.rpm
  ```

  ```
  $ sudo yum install ./cloudhsm-mgmt-util-latest.el6.x86_64.rpm
  ```

------
#### [ Amazon Linux 2 ]

  ```
  $ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL7/cloudhsm-mgmt-util-latest.el7.x86_64.rpm
  ```

  ```
  $ sudo yum install ./cloudhsm-mgmt-util-latest.el7.x86_64.rpm
  ```

------
#### [ CentOS 7\.8\+ ]

  ```
  $ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL7/cloudhsm-mgmt-util-latest.el7.x86_64.rpm
  ```

  ```
  $ sudo yum install ./cloudhsm-mgmt-util-latest.el7.x86_64.rpm
  ```

------
#### [ CentOS 8\.3\+ ]

  ```
  $ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL8/cloudhsm-mgmt-util-latest.el8.x86_64.rpm
  ```

  ```
  $ sudo yum install ./cloudhsm-mgmt-util-latest.el8.x86_64.rpm
  ```

------
#### [ RHEL 7\.8\+ ]

  ```
  $ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL7/cloudhsm-mgmt-util-latest.el7.x86_64.rpm
  ```

  ```
  $ sudo yum install ./cloudhsm-mgmt-util-latest.el7.x86_64.rpm
  ```

------
#### [ RHEL 8\.3\+ ]

  ```
  $ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL8/cloudhsm-mgmt-util-latest.el8.x86_64.rpm
  ```

  ```
  $ sudo yum install ./cloudhsm-mgmt-util-latest.el8.x86_64.rpm
  ```

------
#### [ Ubuntu 16\.04 LTS ]

  ```
  $ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/Xenial/cloudhsm-mgmt-util_latest_amd64.deb
  ```

  ```
  $ sudo apt install ./cloudhsm-mgmt-util_latest_amd64.deb
  ```

------
#### [ Ubuntu 18\.04 LTS ]

  ```
  $ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/Bionic/cloudhsm-mgmt-util_latest_u18.04_amd64.deb
  ```

  ```
  $ sudo apt install ./cloudhsm-mgmt-util_latest_u18.04_amd64.deb
  ```

------
#### [ Windows Server 2012 ]

  1. Download [CloudHSM Management Utility](https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/Windows/AWSCloudHSMManagementUtil-latest.msi)\.

  1. Run the CMU installer \(**AWSCloudHSMManagementUtil\-latest\.msi**\) with Windows administrative privilege\.

------
#### [ Windows Server 2012 R2 ]

  1. Download [CloudHSM Management Utility](https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/Windows/AWSCloudHSMManagementUtil-latest.msi)\.

  1. Run the CMU installer \(**AWSCloudHSMManagementUtil\-latest\.msi**\) with Windows administrative privilege\.

------
#### [ Windows Server 2016 ]

  1. Download [CloudHSM Management Utility](https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/Windows/AWSCloudHSMManagementUtil-latest.msi)\.

  1. Run the CMU installer \(**AWSCloudHSMManagementUtil\-latest\.msi**\) with Windows administrative privilege\.

------

## How to manage HSM users with CMU<a name="manage-users"></a>

This section includes basic commands to manage HSM users with CMU\. 

### To create HSM users<a name="create-users"></a>

Use createUser to create new users on the HSM\. You must log in as a CO to create a user\.

**To create a new CO user**

1. Use the configure tool to update the CMU configuration\.

------
#### [ Linux ]

   ```
   $ sudo /opt/cloudhsm/bin/configure --cmu <IP address>
   ```

------
#### [ Windows ]

   ```
   C:\Program Files\Amazon\CloudHSM>configure.exe --cmu <IP address>
   ```

------

1. Start CMU\.

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

1. Log in to the HSM as a CO user\.

   ```
   aws-cloudhsm>loginHSM CO admin co12345
   ```

   Make sure the number of connections CMU lists match the number of HSMs in the cluster\. If not, log out and start over\.

1. Use createUser to create a CO user named **example\_officer** with a password of **password1**\.

   ```
   aws-cloudhsm>createUser CO example_officer password1
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

1. Type **y**\.

**To create a new CU user**

1. Use the configure tool to update the CMU configuration\.

------
#### [ Linux ]

   ```
   $ sudo /opt/cloudhsm/bin/configure --cmu <IP address>
   ```

------
#### [ Windows ]

   ```
   C:\Program Files\Amazon\CloudHSM>configure.exe --cmu <IP address>
   ```

------

1. Start CMU\.

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

1. Log in to the HSM as a CO user\.

   ```
   aws-cloudhsm>loginHSM CO admin co12345
   ```

   Make sure the number of connections CMU lists match the number of HSMs in the cluster\. If not, log out and start over\.

1. Use createUser to create a CU user named **example\_user** with a password of **password1**\.

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

1. Type **y**\.

For more information about createUser, see [createUser](cloudhsm_mgmt_util-createUser.md)\.

### To list all HSM users on the cluster<a name="list-users"></a>

 Use listUsers command to list all the users on the cluster\. You do not have to log in to run listUsers and all user types can list users\. 

**To list all users on the cluster**

1. Use the configure tool to update the CMU configuration\.

------
#### [ Linux ]

   ```
   $ sudo /opt/cloudhsm/bin/configure --cmu <IP address>
   ```

------
#### [ Windows ]

   ```
   C:\Program Files\Amazon\CloudHSM>configure.exe --cmu <IP address>
   ```

------

1. Start CMU\.

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

1.  Use listUsers to list all the users on the cluster\. 

   ```
   aws-cloudhsm>listUsers
   ```

   CMU lists all the users on the cluster\.

   ```
   Users on server 0(10.0.2.9):
   Number of users found:4
   
       User Id             User Type       User Name                          MofnPubKey    LoginFailureCnt         2FA
            1              PCO             admin                                    NO               0               NO
            2              AU              app_user                                 NO               0               NO
            3              CO              example_officer                          NO               0               NO
            4              CU              example_user                             NO               0               NO
   Users on server 1(10.0.3.11):
   Number of users found:4
   
       User Id             User Type       User Name                          MofnPubKey    LoginFailureCnt         2FA
            1              PCO             admin                                    NO               0               NO
            2              AU              app_user                                 NO               0               NO
            3              CO              example_officer                          NO               0               NO
            4              CU              example_user                             NO               0               NO
   Users on server 2(10.0.1.12):
   Number of users found:4
   
       User Id             User Type       User Name                          MofnPubKey    LoginFailureCnt         2FA
            1              PCO             admin                                    NO               0               NO
            2              AU              app_user                                 NO               0               NO
            3              CO              example_officer                          NO               0               NO
            4              CU              example_user                             NO               0               NO
   ```

   The PCO is the first CO created on each HSM\. The PCO is known as the *primary* CO and this CO has the same permissions as any other CO\.

For more information about listUsers, see [listUsers](cloudhsm_mgmt_util-listUsers.md)\.

### To change HSM user passwords<a name="change-user-password"></a>

 Use changePswd to change a password\. 

 User types and passwords are case sensitive, but user names are not case sensitive\.

 CO, Crypto user \(CU\), and appliance user \(AU\) can change their own password\. To change the passowrd of another user, you must log in as a CO\. You cannot change the password of a user who is currently logged in\. 

**To change your own password**

1. Use the configure tool to update the CMU configuration\.

------
#### [ Linux ]

   ```
   $ sudo /opt/cloudhsm/bin/configure --cmu <IP address>
   ```

------
#### [ Windows ]

   ```
   C:\Program Files\Amazon\CloudHSM>configure.exe --cmu <IP address>
   ```

------

1. Start CMU\.

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

1. Log in to the HSM\.

   ```
   aws-cloudhsm>loginHSM CO admin co12345
   ```

   Make sure the number of connections CMU lists match the number of HSMs in the cluster\. If not, log out and start over\.

1. Use changePswd to change your own password\. 

   ```
   aws-cloudhsm>changePswd CO example_officer <new password>
   ```

   CMU prompts you about the change password operation\.

   ```
   *************************CAUTION********************************
   This is a CRITICAL operation, should be done on all nodes in the
   cluster. AWS does NOT synchronize these changes automatically with the
   nodes on which this operation is not executed or failed, please
   ensure this operation is executed on all nodes in the cluster.
   ****************************************************************
   
   Do you want to continue(y/n)?
   ```

1. Type **y**\.

   CMU prompts you about the change password operation\.

   ```
   Changing password for example_officer(CO) on 3 nodes
   ```

**To change the password of another user**

1. Use the configure tool to update the CMU configuration\.

------
#### [ Linux ]

   ```
   $ sudo /opt/cloudhsm/bin/configure --cmu <IP address>
   ```

------
#### [ Windows ]

   ```
   C:\Program Files\Amazon\CloudHSM>configure.exe --cmu <IP address>
   ```

------

1. Start CMU\.

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

1. Log in to the HSM as a CO user\.

   ```
   aws-cloudhsm>loginHSM CO admin co12345
   ```

   Make sure the number of connections CMU lists match the number of HSMs in the cluster\. If not, log out and start over\.

1.  Use changePswd to change the password of another user\. 

   ```
   aws-cloudhsm>changePswd CU example_user <new password>
   ```

   CMU prompts you about the change password operation\.

   ```
   *************************CAUTION********************************
   This is a CRITICAL operation, should be done on all nodes in the
   cluster. AWS does NOT synchronize these changes automatically with the
   nodes on which this operation is not executed or failed, please
   ensure this operation is executed on all nodes in the cluster.
   ****************************************************************
   
   Do you want to continue(y/n)?
   ```

1. Type **y**\.

   CMU prompts you about the change password operation\.

   ```
   Changing password for example_user(CU) on 3 nodes
   ```

For more information about changePswd, see [changePswd](cloudhsm_mgmt_util-changePswd.md)\.

### To delete HSM users<a name="delete-user"></a>

Use deleteUser to delete a user\. You must log in as a CO to delete another user\.

**Tip**  
 You can't delete crypto users \(CU\) that own keys\. 

**To delete a user**

1. Use the configure tool to update the CMU configuration\.

------
#### [ Linux ]

   ```
   $ sudo /opt/cloudhsm/bin/configure --cmu <IP address>
   ```

------
#### [ Windows ]

   ```
   C:\Program Files\Amazon\CloudHSM>configure.exe --cmu <IP address>
   ```

------

1. Start CMU\.

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

1. Log in to the HSM as a CO user\.

   ```
   aws-cloudhsm>loginHSM CO admin co12345
   ```

   Make sure the number of connections CMU lists match the number of HSMs in the cluster\. If not, log out and start over\.

1.  Use deleteUser to delete a user\. 

   ```
   aws-cloudhsm>deleteUser CO example_officer
   ```

   CMU deletes the user\.

   ```
   Deleting user example_officer(CO) on 3 nodes
   deleteUser success on server 0(10.0.2.9)
   deleteUser success on server 1(10.0.3.11)
   deleteUser success on server 2(10.0.1.12)
   ```

For more information about deleteUser, see [deleteUser](cloudhsm_mgmt_util-deleteUser.md)\.