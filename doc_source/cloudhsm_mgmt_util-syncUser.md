# syncUser<a name="cloudhsm_mgmt_util-syncUser"></a>

You can use the syncUser command in cloudhsm\_mgmt\_util to manually synchronize crypto users \(CUs\) or crypto officers \(COs\) across HSM instances within a cluster or across cloned clusters\. AWS CloudHSM does not automatically synchronize users\. Generally, you manage users in global mode so that all HSMs in a cluster are updated together\. You might need to use syncUser if an HSM is accidentally desynchronized \(for example, due to password changes\) or if you want to rotate user credentials across cloned clusters\. Cloned clusters are usually created in different AWS Regions to simplify the global scaling and disaster recovery processes\.

Before you run any CMU command, you must start CMU and log in to the HSM\. Be sure that you log in with the user account type that can run the commands you plan to use\.

If you add or delete HSMs, update the configuration files for CMU\. Otherwise, the changes that you make might not be effective for all HSMs in the cluster\.

## User type<a name="syncUser-userType"></a>

The following types of users can run this command\.
+ Crypto officers \(CO\)

## Prerequisites<a name="syncUey-prereqs"></a>

Before you begin, you must know the `user ID` of the user on the source HSM to be synchronized with the destination HSM\. To find the `user ID`, use the [listUsers](cloudhsm_mgmt_util-listUsers.md) command to list all users on the HSMs in a cluster\.

You also need to know the `server ID` assigned to the source and destination HSMs, which are shown in the trace output returned by cloudhsm\_mgmt\_util upon initiation\. These are assigned in the same order that the HSMs appear in the configuration file\.

If you are synchronizing HSMs across cloned clusters, follow the instructions in [Using CMU Across Cloned Clusters](cloned-clusters.md) and initialize cloudhsm\_mgmt\_util with the new config file\.

When you are ready to run syncUser, enter server mode on the source HSM by issuing the [server](cloudhsm_mgmt_util-server.md) command\.

## Syntax<a name="syncUser-syntax"></a>

Because this command does not have named parameters, you must enter the arguments in the order specified in the syntax diagram\.

```
syncUser <user ID> <server ID>
```

## Example<a name="syncUser-example"></a>

Run the server command to log into the source HSM and enter server mode\. For this example, we assume that `server 0` is the source HSM\.

```
aws-cloudhsm> server 0
```

Now run the syncUser command\. For this example, we assume that user `6` is the user to be synced, and `server 1` is the destination HSM\.

```
server 0> syncUser 6 1
ExtractMaskedObject: 0x0 !
InsertMaskedObject: 0x0 !
syncUser success
```

## Arguments<a name="syncUser-params"></a>

Because this command does not have named parameters, you must enter the arguments in the order specified in the syntax diagram\.

```
syncUser <user ID> <server ID>
```

**<user ID>**  
Specifies the ID of the user to sync\. You can specify only one user in each command\. To get the ID of a user, use [listUsers](cloudhsm_mgmt_util-listUsers.md)\.  
Required: Yes

**<server ID>**  
Specifies the server number of the HSM to which you are syncing a user\.  
Required: Yes

## Related topics<a name="chmu-syncUser-seealso"></a>
+ [listUsers](cloudhsm_mgmt_util-listUsers.md)
+ [describe\-clusters](https://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/describe-clusters.html) in AWS CLI
+ [server](cloudhsm_mgmt_util-server.md)