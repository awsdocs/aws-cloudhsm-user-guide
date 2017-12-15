# listUsers<a name="key_mgmt_util-listUsers"></a>

The listUsers command in the key\_mgmt\_util gets the users in the HSM, along with their user type and other attributes\.

The user commands in key\_mgmt\_util, listUsers and getKeyInfo, are read\-only commands that crypto users \(CUs\) have permission to run\. The remaining user management commands are part of cloudhsm\_mgmt\_util, which is typically run by a CO with user management permissions\.

Before you run any key\_mgmt\_util command, you must start key\_mgmt\_util and login to the HSM as a crypto user \(CU\)\. 

## Syntax<a name="listUsers-syntax"></a>

```
listUsers 

listUsers -h
```

## Example<a name="listUsers-examples"></a>

This command lists the users of HSMs in the cluster and their attributes\. You can use the `User ID` attribute to identify users in other commands, such as findKey, getAttribute, and getKeyInfo\.

```
Command:  listUsers

        Number Of Users found 4

        Index       User ID     User Type       User Name           MofnPubKey    LoginFailureCnt         2FA
        1                1      PCO             admin                     NO               0               NO
        2                2      AU              app_user                  NO               0               NO
        3                3      CU              alice                     YES              0               NO
        4                4      CU              bob                       NO               0               NO
        5                5      CU              trent                     YES              0               NO

        Cfm3ListUsers returned: 0x00 : HSM Return: SUCCESS
```

The output includes the following user attributes:

+ **User ID**: Use the user ID to identify the user in key\_mgmt\_util and cloudhsm\_mgmt\_util commands\.

+ User type: Determines the operations that the user can perform on the HSM\.

+ **MofnPubKey**: Indicates whether the user has registered a key pair for signing approval tokens\. 

+ **LoginFailureCnt**: 

+ **2FA**: Indicates that the user has enabled multi\-factor authentication\. 

## Parameters<a name="listUsers-parameters"></a>

**\-h**  
Displays help for the command\.   
Required: Yes

## Related Topics<a name="listUsers-seealso"></a>

+ findKey

+ getAttribute

+ getKeyInfo