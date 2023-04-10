# loginHSM and logoutHSM<a name="cloudhsm_mgmt_util-loginLogout"></a>

You can use the loginHSM and logoutHSM commands in cloudhsm\_mgmt\_util to log in and out of each HSM in a cluster\. Any user of any type can use these commands\.

**Note**  
If you exceed five incorrect login attempts, your account is locked out\. To unlock the account, a cryptographic officer \(CO\) must reset your password using the [changePswd](cloudhsm_mgmt_util-changePswd.md) command in cloudhsm\_mgmt\_util\.

## To troubleshoot loginHSM and logoutHSM<a name="troubleshoot-login-logout"></a>

Before you run these cloudhsm\_mgmt\_util commands, you must start cloudhsm\_mgmt\_util\.

If you add or delete HSMs, update the configuration files that the AWS CloudHSM client and the command line tools use\. Otherwise, the changes that you make might not be effective on all HSMs in the cluster\.

If you have more than one HSM in your cluster, you may be allowed additional incorrect login attempts before your account is locked out\. This is because the CloudHSM client balances load across various HSMs\. Therefore, the login attempt may not begin on the same HSM each time\. If you are testing this functionality, we recommend you do so on a cluster with only one active HSM\. 

If you created your cluster before February 2018, your account is locked out after 20 incorrect login attempts\. 

## User type<a name="chmu-loginLogout-userType"></a>

The following users can run these commands\.
+ Precrypto officer \(PRECO\)
+ Crypto officer \(CO\)
+ Crypto user \(CU\)

## Syntax<a name="chmu-loginLogout-syntax"></a>

Enter the arguments in the order specified in the syntax diagram\. Use the `-hpswd` parameter to mask your password\. To login with two\-factor authentication \(2FA\), use the `-2fa` parameter and include a file path\. For more information, see [Arguments](#loginLogout-params)\.

```
loginHSM <user-type> <user-name> <password |-hpswd> [-2fa </path/to/authdata>]
```

```
logoutHSM
```

## Examples<a name="chmu-loginLogout-example"></a>

These examples show how to use loginHSM and logoutHSM to log in and out of all HSMs in a cluster\.

**Example : Log in to the HSMs in a cluster**  
This command logs you in to all HSMs in a cluster with the credentials of a CO user named `admin` and a password of `co12345`\. The output shows that the command was successful and that you have connected to the HSMs \(which, in this case, are `server 0` and `server 1`\)\.  

```
aws-cloudhsm>loginHSM CO admin co12345

loginHSM success on server 0(10.0.2.9)
loginHSM success on server 1(10.0.3.11)
```

**Example : Log in with a hidden password**  
This command is the same as the example above, except this time you specify that the system should hide the password\.   

```
aws-cloudhsm>loginHSM CO admin -hpswd
```
The system prompts you for your password\. You enter the password, the system hides the password, and the output shows that the command was successful and that the you have connected to the HSMs\.  

```
Enter password:

loginHSM success on server 0(10.0.2.9)
loginHSM success on server 1(10.0.3.11)

aws-cloudhsm>
```

**Example : Log out of an HSM**  
This command logs you out of the HSMs that you are currently logged in to \(which, in this case, are `server 0` and `server 1`\)\. The output shows that the command was successful and that you have disconnected from the HSMs\.  

```
aws-cloudhsm>logoutHSM

logoutHSM success on server 0(10.0.2.9)
logoutHSM success on server 1(10.0.3.11)
```

## Arguments<a name="loginLogout-params"></a>

Enter the arguments in the order specified in the syntax diagram\. Use the `-hpswd` parameter to mask your password\. To login with two\-factor authentication \(2FA\), use the `-2fa` parameter and include a file path\. For more information about working with 2FA, see [Using CMU to manage 2FA](manage-2fa.md) 

```
loginHSM <user-type> <user-name> <password |-hpswd> [-2fa </path/to/authdata>]
```

**<user type>**  
Specifies the type of user who is logging in to the HSMs\. For more information, see [User Type](#chmu-loginLogout-userType) above\.  
Required: Yes

**<user name>**  
Specifies the user name of the user who is logging in to the HSMs\.  
Required: Yes

**<password \| \-hpswd >**  
Specifies the password of the user who is logging in to the HSMs\. To hide your password, use the `-hpswd` parameter in place of the password and follow the prompt\.   
Required: Yes

**\[\-2fa </path/to/authdata>\]**  
Specifies that the system should use a second factor to authenticate this 2FA\-enabled CO user\. To get the necessary data for logging in with 2FA, include a path to a location in the file system with a file name after the `-2fa` parameter\. For more information about working with 2FA, see [Using CMU to manage 2FA](manage-2fa.md) \.   
Required: No

## Related topics<a name="loginLogout-seeAlso"></a>
+ [Getting Started with cloudhsm\_mgmt\_util](cloudhsm_mgmt_util-getting-started.md)
+ [Activate the Cluster](activate-cluster.md)