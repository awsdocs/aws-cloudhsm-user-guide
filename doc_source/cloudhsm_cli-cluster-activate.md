# cluster activate<a name="cloudhsm_cli-cluster-activate"></a>

Use the cluster activate command in CloudHSM CLI to [activate a new cluster](activate-cluster.md)\. This command must be run before the cluster can be used to perform cryptographic operations\.

## User type<a name="cluster-activate-userType"></a>

The following types of users can run this command\.
+ unactivated admin

## Syntax<a name="chsm-cli-cluster-activate-syntax"></a>

This command has no parameters\.

```
cluster activate
```

```
aws-cloudhsm > help cluster activate
Activate a cluster

This command will set the initial Admin password. This process will cause your CloudHSM cluster to
move into the ACTIVE state.

USAGE:
    cloudhsm-cli cluster activate [OPTIONS] [--password <PASSWORD>]

OPTIONS:
        --password <PASSWORD> 
            Optional: Plaintext activation password If you do not include this argument you will be
            prompted for it.
```

## Example<a name="chsm-cli-cluster-activate-examples"></a>

This command activates your cluster by setting the initial password for you admin user\.

```
aws-cloudhsm > cluster activate
Enter password:
Confirm password:
{
  "error_code": 0,
  "data": "Cluster activation successful"
}
```

## Related topics<a name="chsm-cluster-activate-seealso"></a>
+ [user create](cloudhsm_cli-user-create.md)
+ [user delete](cloudhsm_cli-user-delete.md)
+ [user change\-password](cloudhsm_cli-user-change-password.md)