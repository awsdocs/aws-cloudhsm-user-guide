# quorum token\-sign list\-timeouts<a name="cloudhsm_cli-qm-token-list-tm"></a>

Use the quorum token\-sign list\-timeouts command in CloudHSM CLI to obtain the token timeout period in seconds for all token types\.

## User type<a name="quorum-token-list-tm-user-type"></a>

The following users can run this command\.
+ All users\. You do not need to be logged in to run this command\.

## Syntax<a name="quorum-token-list-tm-syntax"></a>

```
aws-cloudhsm > help quorum token-sign list-timeouts 
List timeout durations in seconds for token validity

Usage: quorum token-sign list-timeouts
```

## Example<a name="quorum-token-list-tm-examples"></a>

**Example**  

```
aws-cloudhsm > quorum token-sign list-timeouts
{
  "error_code": 0,
  "data": {
    "generated": 600,
    "approved": 600
  }
}
```
The output includes the following:  
+ **generated**: Timeout period in seconds for a generated token to be approved\.
+ **approved**: Timeout period in seconds for an approved token to be used to execute a quorum authorized operation\.

## Arguments<a name="quorum-token-list-tm-arguments"></a>

There are no arguments for this command\.

## Related topics<a name="quorum-token-list-tm-seealso"></a>
+ [quorum token\-sign set\-timeout](cloudhsm_cli-qm-token-set-tm.md)