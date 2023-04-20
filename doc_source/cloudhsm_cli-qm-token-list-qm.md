# quorum token\-sign list\-quorum\-values<a name="cloudhsm_cli-qm-token-list-qm"></a>

Use the quorum token\-sign list\-quorum\-values command in CloudHSM CLI to lists the quorum values set in your AWS CloudHSM cluster\.

## User type<a name="quorum-token-list-qm-user-type"></a>

The following users can run this command\.
+ All users\. You do not need to be logged in to run this command\.

## Syntax<a name="quorum-token-list-qm-syntax"></a>

```
aws-cloudhsm > help quorum token-sign list-quorum-values 
List current quorum values

Usage: quorum token-sign list-quorum-values
```

## Example<a name="quorum-token-list-qm-examples"></a>

This command lists quorum values set in your AWS CloudHSM cluster for each service\.

**Example**  

```
aws-cloudhsm > quorum token-sign list-quorum-values
{
  "error_code": 0,
  "data": {
    "user": 1,
    "quorum": 1
  }
}
```

## Arguments<a name="quorum-token-list-qm-arguments"></a>

***<ROLE>***  
Specifies the role given to the user account\. This parameter is required\. For detailed information about the user types on an HSM, see [Understanding HSM users](manage-hsm-users.md)\.  
**Valid values**  
+ **Admin**: Admins can manage users, but they cannot manage keys\.
+ **Crypto user**: Crypto users can create an manage keys and use keys in cryptographic operations\.

***<USERNAME>***  
Specifies a friendly name for the user\. The maximum length is 31 characters\. The only special character permitted is an underscore \( \_ \)\.  
You cannot change the name of a user after it is created\. In CloudHSM CLI commands, the role and password are case\-sensitive, but the username is not\.  
**Required**: Yes

## Related topics<a name="quorum-token-list-qm-seealso"></a>
+ [Service names and types that support quorum authentication](quorum-auth-chsm-cli-service-names.md)