# createUser<a name="cloudhsm_mgmt_util-createUser"></a>

The createUser command in cloudhsm\_mgmt\_util creates a user on the HSMs\. Only crypto officers \(COs and PCOs\) can run this command\. When the command succeeds, it creates the user in all HSMs in the cluster\. 

## To troubleshoot createUser<a name="troubleshoot-createuser"></a>

 If your HSM configuration is inaccurate, the user might not be created on all HSMs\. To add the user to any HSMs in which it is missing, use the [syncUser](cloudhsm_mgmt_util-syncUser.md) or [createUser](#cloudhsm_mgmt_util-createUser) command only on the HSMs that are missing that user\. To prevent configuration errors, run the [configure](configure-tool.md) tool with the `-m` option\. 

Before you run any CMU command, you must start CMU and log in to the HSM\. Be sure that you log in with a user type that can run the commands you plan to use\.

If you add or delete HSMs, update the configuration files for CMU\. Otherwise, the changes that you make might not be effective for all HSMs in the cluster\.

## User type<a name="createUser-userType"></a>

The following types of users can run this command\.
+ Crypto officers \(CO, PCO\)

## Syntax<a name="createUser-syntax"></a>

Enter the arguments in the order specified in the syntax diagram\. Use the `-hpswd` parameter to mask your password\. To create a CO user with two\-factor authentication \(2FA\), use the `-2fa` parameter and include a file path\. For more information, see [Arguments](#createUser-params)\.

```
createUser <user-type> <user-name> <password |-hpswd> [-2fa </path/to/authdata>]
```

## Examples<a name="createUser-examples"></a>

These examples show how to use createUser to create new users in your HSMs\.

**Example : Create a crypto officer**  
This example creates a crypto officer \(CO\) on the HSMs in a cluster\. The first command uses [loginHSM](cloudhsm_mgmt_util-loginLogout.md) to log in to the HSM as a crypto officer\.  

```
aws-cloudhsm> loginHSM CO admin 735782961

loginHSM success on server 0(10.0.0.1)
loginHSM success on server 1(10.0.0.2)
loginHSM success on server 1(10.0.0.3)
```
The second command uses the createUser command to create `alice`, a new crypto officer on the HSM\.  
The caution message explains that the command creates users on all of the HSMs in the cluster\. But, if the command fails on any HSMs, the user will not exist on those HSMs\. To continue, type `y`\.  
The output shows that the new user was created on all three HSMs in the cluster\.  

```
aws-cloudhsm> createUser CO alice 391019314

*************************CAUTION********************************
This is a CRITICAL operation, should be done on all nodes in the
cluster. AWS does NOT synchronize these changes automatically with the
nodes on which this operation is not executed or failed, please
ensure this operation is executed on all nodes in the cluster.
****************************************************************

Do you want to continue(y/n)?Invalid option, please type 'y' or 'n'

Do you want to continue(y/n)?y
Creating User alice(CO) on 3 nodes
```
When the command completes, `alice` has the same permissions on the HSM as the `admin` CO user, including changing the password of any user on the HSMs\.  
The final command uses the [listUsers](cloudhsm_mgmt_util-listUsers.md) command to verify that `alice` exists on all three HSMs on the cluster\. The output also shows that `alice` is assigned user ID `3`\.`.` You use the user ID to identify `alice` in other commands, such as [findAllKeys](cloudhsm_mgmt_util-findAllKeys.md)\.  

```
aws-cloudhsm> listUsers
Users on server 0(10.0.0.1):
Number of users found:3

    User Id             User Type       User Name                          MofnPubKey    LoginFailureCnt         2FA
         1              PCO             admin                                   YES               0               NO
         2              AU              app_user                                 NO               0               NO
         3              CO              alice                                    NO               0               NO
Users on server 1(10.0.0.2):
Number of users found:3

    User Id             User Type       User Name                          MofnPubKey    LoginFailureCnt         2FA
         1              PCO             admin                                   YES               0               NO
         2              AU              app_user                                 NO               0               NO
         3              CO              alice                                    NO               0               NO

Users on server 1(10.0.0.3):
Number of users found:3

    User Id             User Type       User Name                          MofnPubKey    LoginFailureCnt         2FA
         1              PCO             admin                                   YES               0               NO
         2              AU              app_user                                 NO               0               NO
         3              CO              alice                                    NO               0               NO
```

**Example : Create a crypto user**  
This example creates a crypto user \(CU\), `bob`, on the HSM\. Crypto users can create and manage keys, but they cannot manage users\.   
After you type `y` to respond to the caution message, the output shows that `bob` was created on all three HSMs in the cluster\. The new CU can log in to the HSM to create and manage keys\.  
The command used a password value of `defaultPassword`\. Later, `bob` or any CO can use the [changePswd](cloudhsm_mgmt_util-changePswd.md) command to change his password\.  

```
aws-cloudhsm> createUser CU bob defaultPassword

*************************CAUTION********************************
This is a CRITICAL operation, should be done on all nodes in the
cluster. AWS does NOT synchronize these changes automatically with the
nodes on which this operation is not executed or failed, please
ensure this operation is executed on all nodes in the cluster.
****************************************************************

Do you want to continue(y/n)?Invalid option, please type 'y' or 'n'

Do you want to continue(y/n)?y
Creating User bob(CU) on 3 nodes
```

## Arguments<a name="createUser-params"></a>

Enter the arguments in the order specified in the syntax diagram\. Use the `-hpswd` parameter to mask your password\. To create a CO user with 2FA enabled, use the `-2fa` parameter and include a file path\. For more information about 2FA, see [Using CMU to manage 2FA](manage-2fa.md)\.

```
createUser <user-type> <user-name> <password |-hpswd> [-2fa </path/to/authdata>]
```

**<user\-type>**  
Specifies the type of user\. This parameter is required\.   
For detailed information about the user types on an HSM, see [Understanding HSM users](manage-hsm-users-chsm-cli.md#understanding-users)\.  
Valid values:  
+ **CO**: Crypto officers can manage users, but they cannot manage keys\. 
+ **CU**: Crypto users can create an manage keys and use keys in cryptographic operations\.
PCO, PRECO, and preCO are also valid values, but they are rarely used\. A PCO is functionally identical to a CO user\. A PRECO user is a temporary type that is created automatically on each HSM\. The PRECO is converted to a PCO when you assign a password during [HSM activation](activate-cluster.md)\.  
Required: Yes

**<user\-name>**  
Specifies a friendly name for the user\. The maximum length is 31 characters\. The only special character permitted is an underscore \( \_ \)\.  
You cannot change the name of a user after it is created\. In cloudhsm\_mgmt\_util commands, the user type and password are case\-sensitive, but the user name is not\.  
Required: Yes

**<password \| \-hpswd >**  
Specifies a password for the user\. Enter a string of 7 to 32 characters\. This value is case\-sensitive\. The password appears in plaintext when you type it\. To hide your password, use the `-hpswd` parameter in place of the password and follow the prompts\.   
To change a user password, use [changePswd](cloudhsm_mgmt_util-changePswd.md)\. Any HSM user can change their own password, but CO users can change the password of any user \(of any type\) on the HSMs\.  
Required: Yes

**\[\-2fa </path/to/authdata>\]**  
Specifies the creation of a CO user with 2FA enabled\. To get the data necessary for setting up 2FA authentication, include a path to a location in the file system with a file name after the `-2fa` parameter\. For more information about setting up and working with 2FA, see [Using CMU to manage 2FA](manage-2fa.md)\.  
Required: No

## Related topics<a name="createUser-seealso"></a>
+ [listUsers](cloudhsm_mgmt_util-listUsers.md)
+ [deleteUser](cloudhsm_mgmt_util-deleteUser.md)
+ [syncUser](cloudhsm_mgmt_util-syncUser.md)
+ [changePswd](cloudhsm_mgmt_util-changePswd.md)