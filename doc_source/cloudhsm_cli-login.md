# login<a name="cloudhsm_cli-login"></a>

You can use the login command in CloudHSM CLI to log in and out of each HSM in a cluster\.

Before you run these CloudHSM CLI commands, you must start CloudHSM CLI\.

**Note**  
If you exceed five incorrect login attempts, your account is locked out\. To unlock the account, an admin must reset your password using the [user change\-password](cloudhsm_cli-user-change-password.md) command in cloudhsm\_cli\.

## To troubleshoot login and logout<a name="troubleshoot-login-logout"></a>

If you have more than one HSM in your cluster, you may be allowed additional incorrect login attempts before your account is locked out\. This is because the CloudHSM client balances load across various HSMs\. Therefore, the login attempt may not begin on the same HSM each time\. If you are testing this functionality, we recommend you do so on a cluster with only one active HSM\. 

If you created your cluster before February 2018, your account is locked out after 20 incorrect login attempts\. 

## User type<a name="chsm-cli-login-logout-userType"></a>

The following users can run these commands\.
+ Unactivated admin
+ Admin
+ Crypto user \(CU\)

## Syntax<a name="chsm-cli-login-syntax"></a>

```
aws-cloudhsm > help login
Login to your cluster  
        
USAGE:
    cloudhsm-cli login [OPTIONS] --username <USERNAME> --role <ROLE> [SUBCOMMAND]

OPTIONS:
        --config <CONFIG>        [default: /opt/cloudhsm/etc/cloudhsm-cli.cfg]
        --password <PASSWORD>    Optional: Plaintext user's password. If you do not include this
                                 argument you will be prompted for it
        --role <ROLE>            Role the user has in the Cluster [possible values: crypto-user,
                                 admin]
        --username <USERNAME>    Username to access the Cluster

SUBCOMMANDS:
    help              Print this message or the help of the given subcommand(s)
    mfa-token-sign    Login with token-sign mfa
```

## Example<a name="chsm-cli-login-example"></a>

**Example**  
This command logs you in to all HSMs in a cluster with the credentials of an admin user named `admin1`\.  

```
aws-cloudhsm >login --username admin1 --role admin
Enter password:
{
  "error_code": 0,
  "data": {
    "username": "admin1",
    "role": "admin"
  }
}
```

## Related topics<a name="login-seeAlso"></a>
+ [Getting Started with CloudHSM CLI](cloudhsm_cli-getting-started.md)
+ [Activate the Cluster](activate-cluster.md)