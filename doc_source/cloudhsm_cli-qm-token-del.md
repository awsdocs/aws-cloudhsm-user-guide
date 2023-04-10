# quorum token\-sign delete<a name="cloudhsm_cli-qm-token-del"></a>

Use the quorum token\-sign delete command in CloudHSM CLI to delete one or more tokens for a quorum authorized service\.

## User type<a name="quorum-token-delete-user-type"></a>

The following users can run this command\.
+ Admin

## Syntax<a name="quorum-token-delete-syntax"></a>

```
aws-cloudhsm > help quorum token-sign delete 
Delete one or more Quorum Tokens

Usage: quorum token-sign delete --scope <SCOPE>

Options:
      --scope <SCOPE>
          Scope of which token(s) will be deleted

          Possible values:
          - user: Deletes all token(s) of currently logged in user
          - all:  Deletes all token(s) on the HSM
```

## Example<a name="quorum-token-delete-examples"></a>

The following example shows how the quorum token\-sign delete command in CloudHSM CLI can be used to delete one or more tokens for a quorum authorized service\.

**Example : Delete one or more tokens for a quorum authorized service**  

```
aws-cloudhsm > quorum token-sign delete --scope all
{
  "error_code": 0,
  "data": "Deletion of quorum token(s) successful"
}
```

## Arguments<a name="quorum-token-delete-arguments"></a>

***<scope>***  
The scope in which token\(s\) will be deleted in the AWS CloudHSM cluster\.  
**Valid values**  
+ **User**: Used to delete only tokens owned by the logged in user\.
+ **All**: Used to delete all tokens in the AWS CloudHSM cluster\.

## Related topics<a name="quorum-token-delete-seealso"></a>
+ [user list](cloudhsm_cli-user-list.md)
+ [user create](cloudhsm_cli-user-create.md)
+ [user delete](cloudhsm_cli-user-delete.md)