# Client SDK 3 key synchronization failures<a name="ts-client-sync-fail"></a>

In Client SDK 3, if client\-side synchronization fails, AWS CloudHSM makes a best\-effort response to clean up any unwanted keys that may have been created \(and are now unwanted\)\. This process involves removing unwanted key material immediately or marking unwanted material for later removal\. In both these cases, the resolution does not require any action from you\. In the rare case that AWS CloudHSM cannot remove *and* cannot mark unwanted key material, you must delete the key material\.

**Problem**: You attempt a token key generation, import, or unwrap operation and see errors that specify a failure to *tombstone*\.

```
2018-12-24T18:28:54Z liquidSecurity ERR: print_node_ts_status:
[create_object_min_nodes]Key: 264617 failed to tombstone on node:1
```

**Cause**: AWS CloudHSM was unsuccessful removing *and* marking unwanted key material\. 

**Resolution**: An HSM in your cluster contains unwanted key material that is not marked as unwanted\. You must manually remove the key material\. To manually delete unwanted key material, use key\_mgmt\_util \(KMU\) or an API from the PKCS \#11 library or the JCE provider\. For more information, see [deleteKey](key_mgmt_util-deleteKey.md) or [Client SDKs](use-hsm.md)\.

To make token keys more durable, AWS CloudHSM fails key creation operations that don't succeed on the minimum number of HSMs specified in client\-side synchronization settings\. For more information, see [Key Synchronization in AWS CloudHSM](manage-key-sync.md)\.