# loginHSM and logoutHSM<a name="key_mgmt_util-loginHSM"></a>

The loginHSM and logoutHSM commands in key\_mgmt\_util allow you to log in and out of the HSMs in a cluster\. Once logged in to the HSMs, you can use key\_mgmt\_util to perform a variety of key management operations, including public and private key generation, synchronization, and wrapping\.

Before you run any key\_mgmt\_util command, you must [start key\_mgmt\_util](key_mgmt_util-getting-started.md#key_mgmt_util-start)\. In order to manage keys with key\_mgmt\_util, you must log in to the HSMs as a [crypto user \(CU\)](hsm-users.md#crypto-user)\.

**Note**  
If you exceed five incorrect login attempts, your account is locked out\. If you created your cluster before February 2018, your account is locked out after 20 incorrect login attempts\. To unlock the account, a cryptographic officer \(CO\) must reset your password using the [changePswd](cloudhsm_mgmt_util-changePswd.md) command in cloudhsm\_mgmt\_util\.  
If you have more than one HSM in your cluster, you may be allowed additional incorrect login attempts before your account is locked out\. This is because the CloudHSM client balances load across various HSMs\. Therefore, the login attempt may not begin on the same HSM each time\. If you are testing this functionality, we recommend you do so on a cluster with only one active HSM\. 

## Syntax<a name="loginHSM-syntax"></a>

```
loginHSM -h

loginHSM -u <user type>
         -p <password>
         -s <username>
```

## Example<a name="loginHSM-examples"></a>

This example shows how to log in and out of the HSMs in a cluster with the `loginHSM` and `logoutHSM` commands\.

**Example : Log in to the HSMs**  
This command logs into the HSMs as a crypto user \(`CU`\) with the username `example_user` and password `aws`\. Upon successful completion, the command's output will indicate that you have logged into all HSMs in the cluster\.  

```
Command:  loginHSM -u CU -s example_user -p aws

Cfm3LoginHSM returned: 0x00 : HSM Return: SUCCESS
    
Cluster Error Status
Node id 0 and err state 0x00000000 : HSM Return: SUCCESS
Node id 1 and err state 0x00000000 : HSM Return: SUCCESS
Node id 2 and err state 0x00000000 : HSM Return: SUCCESS
```

**Example : Log out of the HSMs**  
This command logs out of the HSMs\. Upon successful completion, the command's output will indicate that you have logged out of all HSMs in the cluster\.  

```
Command: logoutHSM

>Cfm3LogoutHSM returned: 0x00 : HSM Return: SUCCESS
    
Cluster Error Status
Node id 0 and err state 0x00000000 : HSM Return: SUCCESS
Node id 1 and err state 0x00000000 : HSM Return: SUCCESS
Node id 2 and err state 0x00000000 : HSM Return: SUCCESS
```

## Parameters<a name="loginHSM-parameters"></a>

**\-h**  
Displays help for this command\.  
Required: Yes

**\-u**  
Specifies the login user type\. In order to use key\_mgmt\_util, you must log in as a CU\.  
Required: Yes

**\-s**  
Specifies the login username\.

**\-p**  
Specifies the login password\.  
Required: Yes

## Related Topics<a name="loginHSM-seealso"></a>
+ [exit](key_mgmt_util-exit.md)