# login mfa\-token\-sign<a name="cloudhsm_cli-login-mfa-token-sign"></a>

Use the login mfa\-token\-signcommand in AWS CloudHSM CloudHSM CLI log in using multifactor authentication\. To use this command, you must first set up [MFA for CloudHSM CLI](login-mfa-token-sign.md)\.

Before you run these CloudHSM CLI commands, you must start CloudHSM CLI\.

## User type<a name="cloudhsm_cli-login-mfa-token-userType"></a>

The following users can run these commands\.
+ Admin
+ Crypto user \(CU\)

## Syntax<a name="cloudhsm_cli-login-mfa-token-syntax"></a>

```
aws-cloudhsm > help login mfa-token-sign
Login with token-sign mfa

USAGE:
    login --username <USERNAME> --role <ROLE> mfa-token-sign --token <TOKEN> 

OPTIONS:
        --token <TOKEN>    Filepath where the unsigned token file will be written
```

## Example<a name="cloudhsm_cli-login-mfa-token-example"></a>

**Example**  

```
aws-cloudhsm >login --username test_user --role admin mfa-token-sign --token /home/valid.token
Enter password:
Enter signed token file path (press enter if same as the unsigned token file):
{
  "error_code": 0,
  "data": {
    "username": "test_user",
    "role": "admin"
  }
}
```

## Arguments<a name="cloudhsm_cli-login-mfa-token-arguments"></a>

***<TOKEN>***  
Filepath where the unsigned token file will be written\.  
Required: Yes

## Related topics<a name="cloudhsm_cli-login-mfa-token-seeAlso"></a>
+ [Getting Started with CloudHSM CLI](cloudhsm_cli-getting-started.md)
+ [Activate the Cluster](activate-cluster.md)
+ [Using CloudHSM CLI to manage MFA](login-mfa-token-sign.md)