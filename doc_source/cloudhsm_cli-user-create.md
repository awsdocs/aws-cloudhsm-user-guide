# user create<a name="cloudhsm_cli-user-create"></a>

The user create command in CloudHSM CLI creates a user in your HSM cluster\. Only user accounts with the admin role can run this command\.

## User type<a name="user-create-userType"></a>

The following types of users can run this command\.
+ Admin

## Requirements<a name="user-create-requirements"></a>

To run this command, you must be logged in as an admin user

## Syntax<a name="user-create-syntax"></a>

```
aws-cloudhsm > help user create
Create a new user

Usage: cloudhsm-cli user create [OPTIONS] --username <USERNAME> --role <ROLE> [--password <PASSWORD>]

Options:
      --username <USERNAME>
          Username to access the HSM cluster

      --role <ROLE>
          Role the user has in the cluster

          Possible values:
          - crypto-user: A CryptoUser has the ability to manage and use keys
          - admin:       An Admin has the ability to manage user accounts

      --password <PASSWORD>
          Optional: Plaintext user's password. If you do not include this argument you will be prompted for it

      --approval <APPROVAL>
          Filepath of signed quorum token file to approve operation
```

## Arguments<a name="user-create-arguments"></a>

***<USERNAME>***  
Specifies a friendly name for the user\. The maximum length is 31 characters\. The only special character permitted is an underscore \( \_ \)\. The username is not case sensitive in this command, username is always displayed in lowercase\.  
Required: Yes

***<ROLE>***  
Specifies the role assigned to this user\. This parameter is required\. Valid values are admin, crypto\-user\.  
To get the user’s role, use the user list command\. For detailed information about the user types on an HSM, see [Understanding HSM users](manage-hsm-users.md)\.

***<PASSWORD>***  
Specifies the password of the user who is logging in to the HSMs\.  
Required: Yes

***<APPROVAL>***  
Specifies the file path to a signed quorum token file to approve operation\. Only required if quorum user service quorum value is greater than 1\.

## Example<a name="user-create-examples"></a>

These examples show how to use user create to create new users in your HSMs\.

**Example : Create a crypto user**  
This example creates an account in your AWS CloudHSM cluster with the crypto user role\.  

```
aws-cloudhsm > user create --username alice --role crypto-user
Enter password:
Confirm password:
{
  "error_code": 0,
  "data": {
    "username": "alice",
    "role": "crypto-user"
  }
}
```

## Related topics<a name="user-create-seealso"></a>
+ [user list](cloudhsm_cli-user-list.md)
+ [user delete](cloudhsm_cli-user-delete.md)
+ [user change\-password](cloudhsm_cli-user-change-password.md)