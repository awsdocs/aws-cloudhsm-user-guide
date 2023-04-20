# logout<a name="cloudhsm_cli-logout"></a>

You can use the logout command in CloudHSM CLI to log out of each HSM in a cluster\.

Before you run these CloudHSM CLI commands, you must start CloudHSM CLI\.

## User type<a name="chsm-cli-logout-userType"></a>

The following users can run this command\.
+ Admin
+ Crypto user \(CU\)

## Syntax<a name="chsm-cli-logout-syntax"></a>

```
aws-cloudhsm > help logout
Logout of your cluster

USAGE:
    logout

OPTIONS:
    -h, --help       Print help information
    -V, --version    Print version information
```

## Logout example<a name="chsm-cli-logout-example"></a>

**Example**  
This command logs you out of all HSMs in a cluster\.  

```
aws-cloudhsm >logout

{
  "error_code": 0,
  "data": "Logout successful"
}
```

## Arguments<a name="logout-arguments"></a>

***<USERNAME>***  
Specifies a friendly name for the user\. The maximum length is 31 characters\. The only special character permitted is an underscore \( \_ \)\. The username is not case sensitive in this command, username is always displayed in lowercase\.  
Required: Yes

***<ROLE>***  
Specifies the role assigned to this user\. This parameter is required\. Valid values are admin, crypto\-user\.  
To get the user’s role, use the user list command\. For detailed information about the user types on an HSM, see [Understanding HSM users](manage-hsm-users.md)\.

***<PASSWORD>***  
Specifies the password of the user who is logging in to the HSMs\.

## Related topics<a name="logout-seeAlso"></a>
+ [Getting Started with CloudHSM CLI](cloudhsm_cli-getting-started.md)
+ [Activate the Cluster](activate-cluster.md)