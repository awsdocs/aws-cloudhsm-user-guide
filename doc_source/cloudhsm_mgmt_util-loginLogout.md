# loginHSM and logoutHSM<a name="cloudhsm_mgmt_util-loginLogout"></a>

You can use the loginHSM and logoutHSM commands in cloudhsm\_mgmt\_util to log in and out of each HSM in a cluster\. Any user of any type can use these commands\.

**Note**  
If you exceed five incorrect login attempts, your account is locked out\. If you created your cluster before February 2018, your account is locked out after 20 incorrect login attempts\. To unlock the account, a cryptographic officer \(CO\) must reset your password using the [changePswd](cloudhsm_mgmt_util-changePswd.md) command in cloudhsm\_mgmt\_util\.  
If you have more than one HSM in your cluster, you may be allowed additional incorrect login attempts before your account is locked out\. This is because the CloudHSM client balances load across various HSMs\. Therefore, the login attempt may not begin on the same HSM each time\. If you are testing this functionality, we recommend you do so on a cluster with only one active HSM\. 

Before you run these cloudhsm\_mgmt\_util commands, you must [start cloudhsm\_mgmt\_util](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-start)\.

If you add or delete HSMs, [update the configuration files](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-setup) that the AWS CloudHSM client and the command line tools use\. Otherwise, the changes that you make might not be effective on all HSMs in the cluster\.

## User Type<a name="chmu-loginLogout-userType"></a>

The following users can run these commands\.
+ Precrypto officer \(PRECO\)
+ Crypto officer \(CO\)
+ Crypto user \(CU\)
+ Appliance user \(AU\)

## Syntax<a name="chmu-loginLogout-syntax"></a>

Because these commands do not have named parameters, you must enter the arguments in the order specified in the syntax diagrams\.

```
loginHSM <user type> <user name> <password>
```

```
logoutHSM
```

## Examples<a name="chmu-loginLogout-example"></a>

These examples show how to use loginHSM and logoutHSM to log in and out of all HSMs in a cluster\.

**Example : Log In to the HSMs in a Cluster**  
This command logs in to all HSMs in a cluster with the credentials of a CO user named `admin` and a password of `co12345`\. The output shows that the command was successful and that the user has connected to the HSMs \(which, in this case, are `server 0` and `server 1`\)\.  

```
aws-cloudhsm>loginHSM CO admin co12345
loginHSM success on server 0(10.0.2.9)
loginHSM success on server 1(10.0.3.11)
```

**Example : Log Out of an HSM**  
This command logs out of the HSMs that you are currently logged in to \(which, in this case, are `server 0` and `server 1`\)\. The output shows that the command was successful and that the user has disconnected from the HSMs\.  

```
aws-cloudhsm>logoutHSM
logoutHSM success on server 0(10.0.2.9)
logoutHSM success on server 1(10.0.3.11)
```

## Arguments<a name="loginLogout-params"></a>

Because these commands do not have named parameters, you must enter the arguments in the order specified in the syntax diagrams\.

```
loginHSM <user type> <user name> <password>
```

**<user type>**  
Specifies the type of user who is logging in to the HSMs\. For more information, see [User Type](#chmu-loginLogout-userType) above\.  
Required: Yes

**<user name>**  
Specifies the user name of the user who is logging in to the HSMs\.  
Required: Yes

**<password>**  
Specifies the password of the user who is logging in to the HSMs\.  
Required: Yes

## Related Topics<a name="loginLogout-seeAlso"></a>
+ [Getting Started with cloudhsm\_mgmt\_util](cloudhsm_mgmt_util-getting-started.md)
+ [Activate the Cluster](activate-cluster.md)