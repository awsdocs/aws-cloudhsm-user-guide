# user change\-password<a name="cloudhsm_cli-user-change-password"></a>

Use the user change\-password command in CloudHSM CLI to change the password of an existing user in your AWS CloudHSM cluster\. To enable MFA for a user, use the `user change-mfa` command\.

Any user can change their own password\. In addition, users with the admin role can change the password of another user in the cluster\. You do not need to enter the current password to make the change\.

**Note**  
You cannot change the password of a user who is currently logged in to the cluster\.

## User type<a name="change-password-user-type"></a>

The following users can run this command\.
+ Admin
+ Crypto user \(CU\)

## Syntax<a name="change-password-syntax"></a>

**Note**  
 To enable multifactor authentication \(MFA\) for a user, use the user change\-mfa command\.

```
aws-cloudhsm > help user change-password 
Change a user's password

    Usage:
        cloudhsm-cli user change-password [OPTIONS] --username <USERNAME> --role <ROLE> [--password <PASSWORD>]
    
    Options:

      --username <USERNAME>
          Username of the user that will be modified

      --role <ROLE>
          Role the user has in the cluster

          Possible values:
          - crypto-user: A CryptoUser has the ability to manage and use keys
          - admin:       An Admin has the ability to manage user accounts

      --password <PASSWORD>
          Optional: Plaintext user's password. If you do not include this argument you will be prompted for it

      --approval <APPROVAL>
          Filepath of signed quorum token file to approve operation
          
      --deregister-mfa <DEREGISTER-MFA>
          Deregister the user's mfa public key, if present
          
      --deregister-quorum <DEREGISTER-QUORUM>
          Deregister the user's quorum public key, if present
```

## Example<a name="change-password-examples"></a>

The following examples show how to use user change\-password to reset the password for the current user or any other user in your cluster\.

**Example : Change your password**  
Any user in the cluster can use user change\-password to change their own password\.  
The following output shows that Bob is currently logged in as a crypto user\(CU\)\.  

```
aws-cloudhsm > user change-password --username bob --role crypto-user
Enter password:
Confirm password:
{
  "error_code": 0,
  "data": {
    "username": "bob",
    "role": "crypto-user"
  }
}
```

## Arguments<a name="change-password-arguments"></a>

***<APPROVAL>***  
Specifies the file path to a signed quorum token file to approve operation\. Only required if quorum user service quorum value is greater than 1\.

***<DEREGISTER\-MFA>***  
Deregisters the MFA public key, if present\.

***<DEREGISTER\-QUORUM>***  
Deregister the Quorum public key, if present\.

***<PASSWORD>***  
Specifies the plaintext new password of the user\.  
**Required**: Yes

***<ROLE>***  
Specifies the role given to the user account\. This parameter is required\. For detailed information about the user types on an HSM, see [Understanding HSM users](manage-hsm-users.md)\.  
**Valid values**  
+ **Admin**: Admins can manage users, but they cannot manage keys\.
+ **Crypto user**: Crypto users can create an manage keys and use keys in cryptographic operations\.

***<USERNAME>***  
Specifies a friendly name for the user\. The maximum length is 31 characters\. The only special character permitted is an underscore \( \_ \)\.  
You cannot change the name of a user after it is created\. In CloudHSM CLI commands, the role and password are case\-sensitive, but the username is not\.  
**Required**: Yes

## Related topics<a name="change-password-seealso"></a>
+ [user list](cloudhsm_cli-user-list.md)
+ [user create](cloudhsm_cli-user-create.md)
+ [user delete](cloudhsm_cli-user-delete.md)