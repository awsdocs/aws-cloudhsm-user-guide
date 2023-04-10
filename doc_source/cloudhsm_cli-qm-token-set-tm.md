# quorum token\-sign set\-timeout<a name="cloudhsm_cli-qm-token-set-tm"></a>

Use the quorum token\-sign set\-timeout command in CloudHSM CLI to set the token timeout period in seconds for each token type\.

## User type<a name="quorum-token-set-tm-user-type"></a>

The following users can run this command\.
+ Admin

## Syntax<a name="quorum-token-set-tm-syntax"></a>

```
aws-cloudhsm > help quorum token-sign set-timeout
Set timeout duration in seconds for token validity

Usage: quorum token-sign set-timeout <--generated <GENERATED> |--approved <APPROVED>>

Options:
      --generated <GENERATED>    Timeout period in seconds for a generated (non-approved) token to be approved
      --approved <APPROVED>    Timeout period in seconds for an approved token to be used to execute a quorum operation
```

## Example<a name="quorum-token-set-tm-examples"></a>

The following examples show how to use the quorum token\-sign set\-timeout command to set the token timeout period\.

```
aws-cloudhsm > quorum token-sign set-timeout --generated 900
{
  "error_code": 0,
  "data": "Set token timeout successful"
}
```

## Arguments<a name="quorum-token-set-tm-arguments"></a>

There are no arguments for this command\.

## Related topics<a name="quorum-token-set-tm-seealso"></a>
+ [quorum token\-sign list\-timeouts](cloudhsm_cli-qm-token-list-tm.md)