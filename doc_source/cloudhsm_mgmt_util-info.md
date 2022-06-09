# info<a name="cloudhsm_mgmt_util-info"></a>

The info command in cloudhsm\_mgmt\_util gets information about each of the HSMs in the cluster, including the host name, port, IP address and the name and type of the user who is logged in to cloudhsm\_mgmt\_util on the HSM\.

Before you run any CMU command, you must start CMU and log in to the HSM\. Be sure that you log in with the user account type that can run the commands you plan to use\.

If you add or delete HSMs, update the configuration files for CMU\. Otherwise, the changes that you make might not be effective for all HSMs in the cluster\.

## User type<a name="info-userType"></a>

The following types of users can run this command\.
+ All users\. You do not have to be logged in to run this command\.

## Syntax<a name="info-syntax"></a>

Because this command does not have named parameters, you must enter the arguments in the order specified in the syntax diagram\.

```
info server <server ID>
```

## Example<a name="info-examples"></a>

This example uses info to get information about an HSM in the cluster\. The command uses 0 to refer to the first HSM in the cluster\. The output shows the IP address, port, and the type and name of the current user\.

```
aws-cloudhsm> info server 0
Id      Name                    Hostname         Port   State           Partition               LoginState
0       10.0.0.1                10.0.0.1        2225    Connected       hsm-udw0tkfg1ab         Logged in as 'testuser(CU)'
```

## Arguments<a name="info-params"></a>

Because this command does not have named parameters, you must enter the arguments in the order specified in the syntax diagram\.

```
info server <server ID>
```

**<server id>**  
Specifies the server ID of the HSM\. The HSMs are assigned ordinal numbers that represent the order in which they are added to the cluster, beginning with 0\. To find the server ID of an HSM, use getHSMInfo\.  
Required: Yes

## Related topics<a name="info-seealso"></a>
+ [getHSMInfo](cloudhsm_mgmt_util-getHSMInfo.md)
+ [loginHSM and logoutHSM](cloudhsm_mgmt_util-loginLogout.md)