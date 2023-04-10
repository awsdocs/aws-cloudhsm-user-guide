# loginHSM and logoutHSM<a name="key_mgmt_util-loginHSM"></a>

The loginHSM and logoutHSM commands in key\_mgmt\_util allow you to log in and out of the HSMs in a cluster\. Once logged in to the HSMs, you can use key\_mgmt\_util to perform a variety of key management operations, including public and private key generation, synchronization, and wrapping\.

Before you run any key\_mgmt\_util command, you must [start key\_mgmt\_util](key_mgmt_util-getting-started.md#key_mgmt_util-start)\. In order to manage keys with key\_mgmt\_util, you must log in to the HSMs as a [crypto user \(CU\)](manage-hsm-users-cmu.md#crypto-user-cmu)\.

**Note**  
If you exceed five incorrect login attempts, your account is locked out\. If you created your cluster before February 2018, your account is locked out after 20 incorrect login attempts\. To unlock the account, a cryptographic officer \(CO\) must reset your password using the [changePswd](cloudhsm_mgmt_util-changePswd.md) command in cloudhsm\_mgmt\_util\.  
If you have more than one HSM in your cluster, you may be allowed additional incorrect login attempts before your account is locked out\. This is because the CloudHSM client balances load across various HSMs\. Therefore, the login attempt may not begin on the same HSM each time\. If you are testing this functionality, we recommend you do so on a cluster with only one active HSM\. 

## Syntax<a name="loginHSM-syntax"></a>

```
loginHSM -h

loginHSM -u <user type>
         { -p | -hpswd } <password>
         -s <username>
```

## Example<a name="loginHSM-examples"></a>

This example shows how to log in and out of the HSMs in a cluster with the `loginHSM` and `logoutHSM` commands\.

**Example : Log in to the HSMs**  
This command logs you into the HSMs as a crypto user \(`CU`\) with the username `example_user` and password `aws`\. The output shows that you have logged into all HSMs in the cluster\.  

```
Command:  loginHSM -u CU -s example_user -p aws

Cfm3LoginHSM returned: 0x00 : HSM Return: SUCCESS
    
Cluster Status
Node id 0 and err state 0x00000000 : HSM Return: SUCCESS
Node id 1 and err state 0x00000000 : HSM Return: SUCCESS
Node id 2 and err state 0x00000000 : HSM Return: SUCCESS
```

**Example : Log in with a hidden password**  
This command is the same as the example above, except this time you specify that the system should hide the password\.  

```
Command:  loginHSM -u CU -s example_user -hpswd
```
The system prompts you for your password\. You enter the password, the system hides the password, and the output shows that the command was successful and that the you have connected to the HSMs\.  

```
Enter password: 

Cfm3LoginHSM returned: 0x00 : HSM Return: SUCCESS
    
Cluster Status
Node id 0 and err state 0x00000000 : HSM Return: SUCCESS
Node id 1 and err state 0x00000000 : HSM Return: SUCCESS
Node id 2 and err state 0x00000000 : HSM Return: SUCCESS

Command:
```

**Example : Log out of the HSMs**  
This command logs you out of the HSMs\. The output shows that you have logged out of all HSMs in the cluster\.  

```
Command: logoutHSM

Cfm3LogoutHSM returned: 0x00 : HSM Return: SUCCESS
    
Cluster Status
Node id 0 and err state 0x00000000 : HSM Return: SUCCESS
Node id 1 and err state 0x00000000 : HSM Return: SUCCESS
Node id 2 and err state 0x00000000 : HSM Return: SUCCESS
```

## Parameters<a name="loginHSM-parameters"></a>

**\-h**  
Displays help for this command\.

**\-u**  
Specifies the login user type\. In order to use key\_mgmt\_util, you must log in as a CU\.  
Required: Yes

**\-s**  
Specifies the login username\.  
Required: Yes

**\{ \-p \| \-hpswd \}**  
Specify the login password with `-p`\. The password appears in plaintext when you type it\. To hide your password, use the optional `-hpswd` parameter instead of `-p` and follow the prompt\.  
Required: Yes

## Related topics<a name="loginHSM-seealso"></a>
+ [exit](key_mgmt_util-exit.md)