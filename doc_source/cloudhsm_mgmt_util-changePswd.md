# changePswd<a name="cloudhsm_mgmt_util-changePswd"></a>

The changePswd command in cloudhsm\_mgmt\_util changes the password of an existing user on the HSMs in the cluster\. 

Any user can change their own password\. In addition, Crypto officers \(COs and PCOs\) can change the password of another CO or crypto user \(CU\)\. You do not need to enter the current password to make the change\.

**Note**  
You cannot change the password of a user who is currently logged into the AWS CloudHSM client or key\_mgmt\_util\.

## To troubleshoot changePswd<a name="troubleshoot-changepassword"></a>

Before you run any CMU command, you must start CMU and log in to the HSM\. Be sure that you log in with the user account type that can run the commands you plan to use\.

If you add or delete HSMs, update the configuration files for CMU\. Otherwise, the changes that you make might not be effective for all HSMs in the cluster\.

## User type<a name="changePswd-userType"></a>

The following users can run this command\.
+ Crypto officers \(CO\)
+ Crypto users \(CU\)

## Syntax<a name="changePswd-syntax"></a>

Enter the arguments in the order specified in the syntax diagram\. Use the `-hpswd` parameter to mask your password\. To enable two\-factor authentication \(2FA\) for a CO user, use the `-2fa` parameter and include a file path\. For more information, see [Arguments](#changePswd-params)\.

```
changePswd <user-type> <user-name> <password |-hpswd> [-2fa </path/to/authdata>]
```

## Examples<a name="changePswd-examples"></a>

The following examples show how to use changePassword to reset the password for the current user or any other user in your HSMs\.

**Example : Change your password**  
Any user on the HSMs can use changePswd to change their own password\. Before you change the password, use [info](cloudhsm_mgmt_util-info.md) to get information about each of the HSMs in the cluster, including the username and the user type of the logged in user\.   
The following output shows that Bob is currently logged in as a crypto user\(CU\)\.  

```
        aws-cloudhsm> info server 0
        
Id      Name                    Hostname         Port   State           Partition        LoginState
0       10.1.9.193              10.1.9.193      2225    Connected       hsm-jqici4covtv  Logged in as 'bob(CU)'
        
aws-cloudhsm> info server 1
        
Id      Name                    Hostname         Port   State           Partition        LoginState
1       10.1.10.7               10.1.10.7       2225    Connected       hsm-ogi3sywxbqx  Logged in as 'bob(CU)'
```
To change password, Bob runs changePswd followed with the user type, username, and a new password\.  

```
aws-cloudhsm> changePswd CU bob newPassword

*************************CAUTION********************************
This is a CRITICAL operation, should be done on all nodes in the
cluster. AWS does NOT synchronize these changes automatically with the
nodes on which this operation is not executed or failed, please
ensure this operation is executed on all nodes in the cluster.
****************************************************************

Do you want to continue(y/n)?y
Changing password for bob(CU) on 2 nodes
```

**Example : Change the password of another user**  
You must be a CO or PCO to change the password of another CO, or CU on the HSMs\. Before you change the password for another user, use the [info](cloudhsm_mgmt_util-info.md) command to confirm that your user type is either CO or PCO\.  
The following output confirms that Alice, who is a CO, is currently logged in\.  

```
aws-cloudhsm>info server 0
        
Id      Name             Hostname         Port   State           Partition        LoginState
0       10.1.9.193       10.1.9.193        2225   Connected      hsm-jqici4covtv  Logged in as 'alice(CO)'
        

aws-cloudhsm>info server 1
        
Id      Name             Hostname         Port   State           Partition        LoginState
0       10.1.10.7        10.1.10.7        2225   Connected       hsm-ogi3sywxbqx  Logged in as 'alice(CO)'
```
 Alice wants to reset the password of another user, John\. Before she changes the password, she uses the [listUsers](cloudhsm_mgmt_util-listUsers.md) command to verify John's user type\.   
 The following output lists John as a CO user\.   

```
aws-cloudhsm> listUsers
Users on server 0(10.1.9.193):
Number of users found:5

    User Id             User Type       User Name            MofnPubKey    LoginFailureCnt         2FA
         1              PCO             admin                     YES               0               NO
         2              AU              jane                       NO               0               NO
         3              CU              bob                        NO               0               NO
         4              CU              alice                      NO               0               NO
         5              CO              john                       NO               0               NO
Users on server 1(10.1.10.7):
Number of users found:5

    User Id             User Type       User Name            MofnPubKey    LoginFailureCnt         2FA
         1              PCO             admin                     YES               0               NO
         2              AU              jane                       NO               0               NO
         3              CU              bob                        NO               0               NO
         4              CO              alice                      NO               0               NO
         5              CO              john                       NO               0               NO
```
To change the password, Alice runs changePswd followed with John's user type, username, and a new password\.  

```
aws-cloudhsm>changePswd CO john newPassword

*************************CAUTION********************************
This is a CRITICAL operation, should be done on all nodes in the
cluster. AWS does NOT synchronize these changes automatically with the
nodes on which this operation is not executed or failed, please
ensure this operation is executed on all nodes in the cluster.
****************************************************************

Do you want to continue(y/n)?y
Changing password for john(CO) on 2 nodes
```

## Arguments<a name="changePswd-params"></a>

Enter the arguments in the order specified in the syntax diagram\. Use the `-hpswd` parameter to mask your password\. To enable 2FA for a CO user, use the `-2fa` parameter and include a file path\. For more information about working with 2FA, see [Managing 2FA](manage-2fa.md)

```
changePswd <user-type> <user-name> <password |-hpswd> [-2fa </path/to/authdata>]
```

**<user\-type>**  
Specifies the current type of the user whose password you are changing\. You cannot use changePswd to change the user type\.   
Valid values are `CO`, `CU`, `PCO`, and `PRECO`\.  
To get the user type, use [listUsers](cloudhsm_mgmt_util-listUsers.md)\. For detailed information about the user types on an HSM, see [Understanding HSM users](manage-hsm-users.md#understanding-users)\.  
Required: Yes

**<user\-name>**  
Specifies the user's friendly name\. This parameter is not case\-sensitive\. You cannot use changePswd to change the user name\.   
Required: Yes

**<password \| \-hpswd >**  
Specifies a new password for the user\. Enter a string of 7 to 32 characters\. This value is case sensitive\. The password appears in plaintext when you type it\. To hide your password, use the `-hpswd` parameter in place of the password and follow the prompts\.   
Required: Yes

**\[\-2fa </path/to/authdata>\]**  
Specifies enabling 2FA for this CO user\. To get the data necessary for setting up 2FA, include a path to a location in the file system with a file name after the `-2fa` parameter\. For more information about working with 2FA, see [Managing 2FA](manage-2fa.md)\.  
Required: No

## Related topics<a name="changePswd-seealso"></a>
+ [info](cloudhsm_mgmt_util-info.md)
+ [listUsers](cloudhsm_mgmt_util-listUsers.md)
+ [createUser](cloudhsm_mgmt_util-createUser.md)
+ [deleteUser](cloudhsm_mgmt_util-deleteUser.md)