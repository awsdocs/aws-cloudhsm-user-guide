# quorum token\-sign list<a name="cloudhsm_cli-qm-token-list"></a>

Use the quorum token\-sign list command in CloudHSM CLI to list all token\-sign quorum tokens present in your AWS CloudHSM cluster\.

## User type<a name="quorum-token-list-user-type"></a>

The following users can run this command\.
+ Admin
+ Crypto user \(CU\)

## Syntax<a name="quorum-token-list-syntax"></a>

```
aws-cloudhsm > help quorum token-sign list 
List the token-sign tokens in your cluster

Usage: quorum token-sign list
```

## Example<a name="quorum-token-list-examples"></a>

This command will list all token\-sign tokens present in your AWS CloudHSM cluster\.

**Example**  

```
aws-cloudhsm > quorum token-sign list
{
  "error_code": 0,
  "data": {
    "tokens": [
      {
        "username": "admin",
        "service": "quorum",
        "approvals-required": 2
        "number-of-approvals": 0
        "token-timeout-seconds": 397
        "cluster-coverage": "full"
      },
      {
        "username": "admin",
        "service": "user",
        "approvals-required": 2
        "number-of-approvals": 2
        "token-timeout-seconds": 588
        "cluster-coverage": "full"
      }
    ]
  }
}
```

## Arguments<a name="quorum-token-list-arguments"></a>

There are no arguments for this command\.

## Related topics<a name="quorum-token-list-seealso"></a>
+ [quorum token\-sign generate](cloudhsm_cli-qm-token-gen.md)