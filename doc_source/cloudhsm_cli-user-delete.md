# user delete<a name="cloudhsm_cli-user-delete"></a>

The user delete command in CloudHSM CLI deletes a user from your AWS CloudHSM cluster\. Only user accounts with the admin role may run this command\. You cannot delete a user who is currently logged into a HSM\. 

## User type<a name="user-delete-userType"></a>

The following types of users can run this command\.
+ Admin

## Requirements<a name="user-delete-requirements"></a>
+ You can't delete user accounts that own keys\.
+ Your user account must have the admin role to run this command\.

## Syntax<a name="user-delete-syntax"></a>

Because this command does not have named parameters, you must enter the arguments in the order specified in the syntax diagram\.

```
aws-cloudhsm > help user delete
Delete a user

Usage: user delete [OPTIONS] --username <USERNAME> --role <ROLE>

Options:
      --username <USERNAME>
          Username to access the HSM cluster

      --role <ROLE>
          Role the user has in the cluster

          Possible values:
          - crypto-user: A CryptoUser has the ability to manage and use keys
          - admin:       An Admin has the ability to manage user accounts

      --approval <APPROVAL>
          Filepath of signed quorum token file to approve operation
```

## Example<a name="user-delete-examples"></a>

```
aws-cloudhsm > user delete --username alice --role crypto-user
{
  "error_code": 0,
  "data": {
    "username": "alice",
    "role": "crypto-user"
  }
}
```

## Arguments<a name="user-delete-arguments"></a>

***<username>***  
Specifies a friendly name for the user\. The maximum length is 31 characters\. The only special character permitted is an underscore \( \_ \)\. The username is not case sensitive in this command, username is always displayed in lowercase\.  
Required: Yes

***<role>***  
Specifies the role assigned to this user\. This parameter is required\. Valid values are admin, crypto\-user\.  
To get the user’s role, use the user list command\. For detailed information about the user types on an HSM, see [Understanding HSM users](manage-hsm-users.md)\.  
Required: Yes

***<approval>***  
Specifies the file path to a signed quorum token file to approve operation\. Only required if quorum user service quorum value is greater than 1\.  
Required: Yes

## Related topics<a name="user-delete-seealso"></a>
+ [user list](cloudhsm_cli-user-list.md)
+ [user create](cloudhsm_cli-user-create.md)
+ [user change\-password](cloudhsm_cli-user-change-password.md)