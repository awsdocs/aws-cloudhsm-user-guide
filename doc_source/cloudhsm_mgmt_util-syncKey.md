# syncKey<a name="cloudhsm_mgmt_util-syncKey"></a>

You can use the syncKey command in cloudhsm\_mgmt\_util to manually synchronize keys across HSM instances within a cluster or across cloned clusters\. In general, you will not need to use this command, as HSM instances within a cluster sync keys automatically\. However, key synchronization across cloned clusters must be done manually\. Cloned clusters are usually created in different AWS Regions in order to simplify the global scaling and disaster recovery processes\.

You cannot use syncKey to synchronize keys across arbitrary clusters: one of the clusters must have been created from a backup of the other\. Additionally, both clusters must have consistent CO and CU credentials in order for the operation to be successful\. For more information, see [HSM Users](manage-hsm-users-chsm-cli.md#understanding-users)\.

To use syncKey, you must first [create an AWS CloudHSM configuration file](cloned-clusters.md) that specifies one HSM from the source cluster and one from the destination cluster\. This will allow cloudhsm\_mgmt\_util to connect to both HSM instances\. Use this configuration file to start cloudhsm\_mgmt\_util\. Then log in with the credentials of a CO or a CU who owns the keys you want to synchronize\.

## User type<a name="syncKey-userType"></a>

The following types of users can run this command\.
+ Crypto officers \(CO\)
+ Crypto users \(CU\)

**Note**  
COs can use syncKey on any keys, while CUs can only use this command on keys that they own\. For more information, see [Understanding HSM users](manage-hsm-users-chsm-cli.md#understanding-users)\.

## Prerequisites<a name="syncKey-prereqs"></a>

Before you begin, you must know the `key handle` of the key on the source HSM to be synchronized with the destination HSM\. To find the `key handle`, use the [listUsers](cloudhsm_mgmt_util-listUsers.md) command to list all identifiers for named users\. Then, use the [findAllKeys](cloudhsm_mgmt_util-findAllKeys.md) command to find all keys that belong to a particular user\. 

You also need to know the `server IDs` assigned to the source and destination HSMs, which are shown in the trace output returned by cloudhsm\_mgmt\_util upon initiation\. These are assigned in the same order that the HSMs appear in the configuration file\.

Follow the instructions in [Using CMU Across Cloned Clusters](cloned-clusters.md) and initialize cloudhsm\_mgmt\_util with the new config file\. Then, enter server mode on the source HSM by issuing the [server](cloudhsm_mgmt_util-server.md) command\.

## Syntax<a name="syncKey-syntax"></a>

**Note**  
To run syncKey, first enter server mode on the HSM which contains the key to be synchronized\.

Because this command does not have named parameters, you must enter the arguments in the order specified in the syntax diagram\.

**User Type**: Crypto user \(CU\)

```
syncKey <key handle> <destination hsm>
```

## Example<a name="syncKey-example"></a>

Run the **server** command to log into the source HSM and enter server mode\. For this example, we assume that `server 0` is the source HSM\.

```
aws-cloudhsm> server 0
```

Now run the **syncKey** command\. In this example, we assume key `261251` is to be synced to `server 1`\.

```
aws-cloudhsm> syncKey 261251 1
syncKey success
```

## Arguments<a name="syncKey-params"></a>

Because this command does not have named parameters, you must enter the arguments in the order specified in the syntax diagram\.

```
syncKey <key handle> <destination hsm>
```

**<key handle>**  
Specifies the key handle of the key to sync\. You can specify only one key in each command\. To get the key handle of a key, use [findAllKeys](cloudhsm_mgmt_util-findAllKeys.md) while logged in to an HSM server\.  
Required: Yes

**<destination hsm>**  
Specifies the number of the server to which you are syncing a key\.  
Required: Yes

## Related topics<a name="chmu-syncKey-seealso"></a>
+ [listUsers](cloudhsm_mgmt_util-listUsers.md)
+ [findAllKeys](cloudhsm_mgmt_util-findAllKeys.md)
+ [describe\-clusters](https://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/describe-clusters.html) in AWS CLI
+ [server](cloudhsm_mgmt_util-server.md)