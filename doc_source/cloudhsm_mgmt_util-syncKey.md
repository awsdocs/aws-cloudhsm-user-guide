# syncKey<a name="cloudhsm_mgmt_util-syncKey"></a>

You can use the **syncKey** command in cloudhsm\_mgmt\_util to manually synchronize keys across HSM instances within a cluster or across cloned clusters\. In general, you will not need to use this command, as HSM instances within a cluster sync keys automatically\. However, key synchronization across cloned clusters must be done manually\. Cloned clusters are usually created in different AWS Regions in order to simplify the global scaling and disaster recovery processes\.

You cannot use `syncKey` to synchronize keys across arbitrary clusters: one of the clusters must have been created from a backup of the other\. Additionally, both clusters must have consistent CO and CU credentials in order for the operation to be successful\. For more information, see [HSM Users](hsm-users.md)\.

To use `syncKey`, you must first create an AWS CloudHSM configuration file that specifies one HSM from the source cluster and one from the destination cluster\. This will allow cloudhsm\_mgmt\_util to connect to both HSM instances\. Use this configuration file to [start cloudhsm\_mgmt\_util](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-start)\. Then [log in](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-log-in) with the credentials of a CO or a CU who owns the keys you want to synchronize\. For further instructions on how to create this configuration file, see the [configuration file instructions](#syncKey-config) below\.

## User Type<a name="syncKey-userType"></a>

The following types of users can run this command\.
+ Crypto officers \(CO\)
+ Crypto users \(CU\)

**Note**  
COs can use `syncKey` on any keys, while CUs can only use this command on keys that they own\. For more information, see [ HSM Users  Use the credentials of the appropriate type of HSM user to securely interact with the HSMs in your AWS CloudHSM cluster\.   Most operations that you perform on the HSM require the credentials of an *HSM user*\. The HSM authenticates each HSM user by means of a user name and password\. Each HSM user has a *type* that determines which operations the user is allowed to perform on the HSM\. The following topics explain the types of HSM users\.   Precrypto Officer \(PRECO\)  The precrypto officer \(PRECO\) is a temporary user that exists only on the first HSM in an AWS CloudHSM cluster\. The first HSM in a new cluster contains a PRECO user with a default user name and password\. To [activate a cluster](activate-cluster.md), you log in to the HSM and change the PRECO user's password\. When you change the password, the PRECO user becomes a crypto officer \(CO\)\. The PRECO user can only change its own password and perform read\-only operations on the HSM\.   Crypto Officer \(CO\)  A crypto officer \(CO\) can perform user management operations\. For example, a CO can create and delete users and change user passwords\. For more information, see the [HSM User Permissions Table](#user-permissions-table)\. When you [activate a new cluster](activate-cluster.md), the user changes from a [Precrypto Officer](#preco) \(PRECO\) to a Crypto Officer \(CO\)\.    Crypto User \(CU\)  A crypto user \(CU\) can perform the following key management and cryptographic operations\.   **Key management** – Create, delete, share, import, and export cryptographic keys\.   **Cryptographic operations** – Use cryptographic keys for encryption, decryption, signing, verifying, and more\.   For more information, see the [HSM User Permissions Table](#user-permissions-table)\.   Appliance User \(AU\)  The appliance user \(AU\) can perform cloning and synchronization operations\. AWS CloudHSM uses the AU to synchronize the HSMs in an AWS CloudHSM cluster\. The AU exists on all HSMs provided by AWS CloudHSM, and has limited permissions\. For more information, see the [HSM User Permissions Table](#user-permissions-table)\. AWS uses the AU to perform cloning and synchronization operations on your cluster's HSMs\. AWS cannot perform any operations on your HSMs except those granted to the AU and unauthenticated users\. AWS cannot view or modify your users or keys and cannot perform any cryptographic operations using those keys\.   HSM User Permissions Table  The following table lists HSM operations and whether each type of HSM user can perform them\. 


|  | Crypto Officer \(CO\) | Crypto User \(CU\) | Appliance User \(AU\) | Unauthenticated user | 
| --- | --- | --- | --- | --- | 
| Get basic cluster info¹ | Yes | Yes | Yes | Yes | 
| Zeroize an HSM² | Yes | Yes | Yes | Yes | 
| Change own password | Yes | Yes | Yes | Not applicable | 
| Change any user's password | Yes | No | No | No | 
| Add, remove users | Yes | No | No | No | 
| Get sync status³ | Yes | Yes | Yes | No | 
| Extract, insert masked objects⁴ | Yes | Yes | Yes | No | 
| Create, share, delete keys | No | Yes | No | No | 
| Encrypt, decrypt | No | Yes | No | No | 
| Sign, verify | No | Yes | No | No | 
| Generate digests and HMACs | No | Yes | No | No |  ¹Basic cluster information includes the number of HSMs in the cluster and each HSM's IP address, model, serial number, device ID, firmware ID, etc\. ²When an HSM is zeroized, all keys, certificates, and other data on the HSM is destroyed\. You can use your cluster's security group to prevent an unauthenticated user from zeroizing your HSM\. For more information, see [Create a Cluster](create-cluster.md)\. ³The user can get a set of digests \(hashes\) that correspond to the keys on the HSM\. An application can compare these sets of digests to understand the synchronization status of HSMs in a cluster\. ⁴Masked objects are keys that are encrypted before they leave the HSM\. They cannot be decrypted outside of the HSM\. They are only decrypted after they are inserted into an HSM that is in the same cluster as the HSM from which they were extracted\. An application can extract and insert masked objects to synchronize the HSMs in a cluster\.   ](hsm-users.md)\.

## Prerequisites<a name="syncKey-prereqs"></a>

Before you begin, you must know the `key handle` of the key on the source HSM to be synchronized with the destination HSM\. To find the `key handle`, use the [listUsers](cloudhsm_mgmt_util-listUsers.md) command to list all identifiers for named users\. Then, use the [findAllKeys](cloudhsm_mgmt_util-findAllKeys.md) command to find all keys that belong to a particular user\. In this example, we assume that the key handle to be synchronized is `261251`\.

You also need to know the `server IDs` assigned to the source and destination HSMs, which are shown in the trace output returned by cloudhsm\_mgmt\_util upon initiation\. These are assigned in the same order that the HSMs appear in the configuration file\. For this example, we assume that `server 0` is the source HSM, and `server 1` is the destination HSM\.

### Create a Configuration File for `syncKey` Across Cloned Clusters<a name="syncKey-config"></a>

Create a copy of your current config file \(`/opt/cloudhsm/etc/cloudhsm_mgmt_config.cfg`\)\. For this example, change the copy's name to `clustersync.cfg`\.

Edit `clustersync.cfg` to include the Elastic Network Interface \(ENI\) IPs of the two HSMs to be synced\. We recommend that you specify the source HSM first, followed by the destination HSM\. To find the ENI IP of an HSM, use the [describe\-clusters](https://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/describe-clusters.html) CLI command\.

Initialize `cloudhsm_mgmt_util` with the new config file by issuing the following command:

```
aws-cloudhsm> /opt/cloudhsm/bin/cloudhsm_mgmt_util /opt/cloudhsm/etc/clustersync.cfg
```

Check the status messages returned to ensure that the `cloudhsm_mgmt_util` is connected to both HSMs and determine which of the returned ENI IPs corresponds to each cluster\. Then, enter server mode on the source HSM by issuing the `server` command\. In this example, server 0 is the HSM instance from the source cluster, and server 1 is the HSM instance from the destination cluster\. 

## Syntax<a name="syncKey-syntax"></a>

**Note**  
To run `syncKey`, first enter server mode on the HSM, which contains the key to be synchronized\.

Because this command does not have named parameters, you must enter the arguments in the order specified in the syntax diagram\.

**User Type**: Crypto user \(CU\)

```
syncKey <key handle> <destination hsm>
```

## Example<a name="syncKey-example"></a>

Run the **server** command to log into the source HSM and enter server mode\.

```
aws-cloudhsm> server 0
```

Now run the **syncKey** command\.

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

## Related Topics<a name="chmu-syncKey-seealso"></a>
+ [listUsers](cloudhsm_mgmt_util-listUsers.md)
+ [findAllKeys](cloudhsm_mgmt_util-findAllKeys.md)
+ [describe\-clusters](https://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/describe-clusters.html) in AWS CLI