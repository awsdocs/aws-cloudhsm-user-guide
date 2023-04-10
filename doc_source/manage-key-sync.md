# Key synchronization in AWS CloudHSM<a name="manage-key-sync"></a>

This topic describes key synchronization settings in AWS CloudHSM, common issues customers face working with keys on a cluster, and strategies for making keys more durable\.

**Topics**
+ [Concepts](#concepts-key-sync)
+ [Understanding key synchronization](#understand-key-sync)
+ [Working with client key durability settings](#working-client-sync)
+ [Synchronizing keys across cloned clusters](#cli-sync)

## Concepts<a name="concepts-key-sync"></a>

**Token keys**  
Persistent keys that you create during key generate, import or unwrap operations\. AWS CloudHSM synchronizes token keys across a cluster\.

**Session keys**  
Ephemeral keys that exist only on one hardware security module \(HSM\) in the cluster\. AWS CloudHSM does *not* synchronize session keys across a cluster\.

**Client\-side key synchronization**  
A client\-side process that clones token keys you create during key generate, import or unwrap operations\. You can make token keys more durable by running a cluster with a minimum of two HSMs\.

**Server\-side key synchronization**  
Periodically clones keys to every HSM in the cluster\. Requires no management\.

**Client key durability settings**  
Settings you configure on the client that impact key durability\. Works differently in Client SDK 5 and Client SDK 3\.   
+ In Client SDK 5, use this setting to run a single HSM cluster\. 
+ In Client SDK 3, use this setting to specify the number of HSMs required for key creation operations to succeed\.

## Understanding key synchronization<a name="understand-key-sync"></a>

AWS CloudHSM uses key synchronization to clone token keys across all the HSMs in a cluster\. You create token keys as persistent keys during key generation, import, or unwrap operations\. To distribute these keys across the cluster, CloudHSM offers both client\-side and server\-side key synchronization\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/key-synch.png)

The goal with key synchronization—both server side and client side—is to distribute new keys across the cluster as quickly as possible after you create them\. This is important because the subsequent calls you make to use new keys can get routed to any available HSM in the cluster\. If the call you make routes to a HSM without the key, then the call fails\. You can mitigate these type failures by specifying that your applications retry subsequent calls made after key creation operations\. The time required to synchronize can vary, depending on the workload of your cluster and other intangibles\. Use CloudWatch metrics to determine the timing your application should employ in this type situation\. For more information, see [CloudWatch Metrics](hsm-metrics-cw.md)\.

The challenge with key synchronization in a cloud environment is key durability\. You create keys on a single HSM and often begin using those keys immediately\. If the HSM on which you create keys should fail before the keys have been cloned to another HSM in the cluster, you lose the keys *and* access to anything encrypted by the keys\. To mitigate this risk, we offer *client\-side synchronization*\. Client side synchronization is a client\-side process that clones the keys you create during key generate, import, or unwrap operations\. Cloning keys as you create them makes keys more durable\. Of course, you can't clone keys in a cluster with a single HSM\. To make keys more durable, we also recommend you configure your cluster to use a minimum of two HSMs\. With client\-side synchronization and a cluster with two HSMs, you can meet the challenge of key durability in a cloud environment\.

## Working with client key durability settings<a name="working-client-sync"></a>

Key synchronization is mostly an automatic process, but you can manage client\-side key durability settings\. Client\-side key durability settings works differently in Client SDK 5 and Client SDK 3\. 
+ In Client SDK 5, we introduce the concept of *key availability quorums* which requires you to run clusters with a minimum of two HSMs\. You can use client\-side key durability settings to opt out of the two HSM requirement\. For more information about quorums, see [Client SDK 5 concepts](#client-sync-8concept)\.
+ In Client SDK 3, you use client\-side key durability settings to specify the number of HSMs on which key creation must succeed for the overall operation to be deemed a success\. 

### Client SDK 5 client key durability settings<a name="client-sync-sdk8"></a>

#### Client SDK 5 concepts<a name="client-sync-8concept"></a>

**Key Availability Quorum**  
AWS CloudHSM specifies the number of HSMs in a cluster on which keys must exist before your application can use the key\. Requires clusters with a minimum of two HSMs\.

In Client SDK 5, key synchronization is a fully automatic process\. With key availability quorum, newly created keys must exist on two HSMs in the cluster before your application can use the key\. To use key availability quorum, your cluster must have a minimum of two HSMs\.

If your cluster configuration doesn’t meet the key durability requirements, any attempt to create or use a token key will fail with the following error message in the logs: 

```
Key <key handle> does not meet the availability requirements - The key must be available on at least 2 HSMs before being used.
```

You can use client configuration settings to opt out of key availability quorum\. You might want to opt out to run a cluster with a single HSM, for example\. 

#### Managing client key durability settings<a name="setting-file-sdk8"></a>

To manage client key durability settings, you must use the configure tool for Client SDK 5\. 

------
#### [ PKCS \#11 library ]

**To disable client key durability for Client SDK 5 on Linux**
+  Use the configure tool to disable client key durability settings\. 

  ```
  $ sudo /opt/cloudhsm/bin/configure-pkcs11 --disable-key-availability-check
  ```

**To disable client key durability for Client SDK 5 on Windows**
+  Use the configure tool to disable client key durability settings\. 

  ```
  C:\Program Files\Amazon\CloudHSM\configure-pkcs11.exe --disable-key-availability-check
  ```

------
#### [ OpenSSL Dynamic Engine ]

**To disable client key durability for Client SDK 5 on Linux**
+  Use the configure tool to disable client key durability settings\. 

  ```
  $ sudo /opt/cloudhsm/bin/configure-dyn --disable-key-availability-check
  ```

------
#### [ JCE provider ]

**To disable client key durability for Client SDK 5 on Linux**
+  Use the configure tool to disable client key durability settings\. 

  ```
  $ sudo /opt/cloudhsm/bin/configure-jce --disable-key-availability-check
  ```

**To disable client key durability for Client SDK 5 on Windows**
+  Use the configure tool to disable client key durability settings\. 

  ```
  C:\Program Files\Amazon\CloudHSM\configure-jce.exe --disable-key-availability-check
  ```

------

### Client SDK 3 client key durability settings<a name="client-sync-sdk3"></a>

In Client SDK 3, key synchronization is mostly an automatic process, but you can use the client key durability settings to make keys more durable\. You specify the number of HSMs on which key creation must succeed for the overall operation to be deemed a success\. Client\-side synchronization always makes a best\-effort attempt to clone keys to every HSM in the cluster no matter what setting you choose\. Your setting enforces key creation on the number of HSMs you specify\. If you specify a value and the system cannot replicate the key to that number of HSMs, then the system automatically cleans up any unwanted key material and you can try again\.

**Important**  
If you don’t set client key durability settings \(or if you use the default value of 1\), your keys are vulnerable to loss\. If your current HSM should fail before the server\-side service has cloned that key to another HSM, you lose the key material\. 

To maximize key durability, consider specifying at least two HSMs for client\-side synchronization\. Remember that no matter how many HSMs you specify, the workload on your cluster remains the same\. Client\-side synchronization always makes a best\-effort attempt to clone keys to every HSM in the cluster\. 

**Recommendations**
+ **Minimum**: Two HSMs per cluster
+ **Maximum**: One fewer than the total number of HSMs in your cluster

If client\-side synchronization fails, the client service cleans up any unwanted keys that may have been created and are now unwanted\. This clean up is a best\-effort response that may not always work\. If cleanup fails, you may have to delete unwanted key material\. For more information, see [Key Synchronization Failures](ts-client-sync-fail.md)\.

#### Setting up the configuration file for client key durability<a name="setting-file"></a>

To specify client key durability settings, you must edit `cloudhsm_client.cfg`\. 

**To edit the client configuration file**

1. Open `cloudhsm_client.cfg`\.

   **Linux**:

   ```
   /opt/cloudhsm/etc/cloudhsm_client.cfg
   ```

   **Windows**:

   ```
   C:\ProgramData\Amazon\CloudHSM\data\cloudhsm_client.cfg
   ```

1. In the `client` node of the file, add `create_object_minimum_nodes` and specify a value for the minimum number of HSMs on which AWS CloudHSM must successfully create keys for key creation operations to succeed\.

   ```
   "create_object_minimum_nodes" : 2
   ```
**Note**  
The key\_mgmt\_util \(KMU\) command\-line tool has an additional setting for client key durability\. For more information, see [KMU and client\-side synchronization](#kmu-sync)

##### Configuration reference<a name="client-side-ref"></a>

These are the client\-side synchronization properties, shown in an excerpt of the `cloudhsm_client.cfg`:

```
{
    "client": {
        "create_object_minimum_nodes" : 2,
        ...
    },
    
    ...
}
```

**create\_object\_minimum\_nodes**  
Specifies the minimum number of HSMs required to deem key generation, key import, or key unwrap operations a success\. If set, the default is 1\. This means that for every key create operation, the client\-side service attempts to create keys on every HSM in the cluster, but to return a success, only needs to create a *single key* on one HSM in the cluster\. 

#### KMU and client\-side synchronization<a name="kmu-sync"></a>

If you create keys with the key\_mgmt\_util \(KMU\) command\-line tool, you use an optional command line parameter \(`-min_srv`\) to *limit* the number of HSMs on which to clone keys\. If you specify the command\-line parameter *and* a value in the configuration file, AWS CloudHSM honors the LARGER of the two values\.

 For more information, see the following topics:
+ [genDSAKeyPair](key_mgmt_util-genDSAKeyPair.md)
+ [genECCKeyPair](key_mgmt_util-genECCKeyPair.md)
+ [genRSAKeyPair](key_mgmt_util-genRSAKeyPair.md)
+ [genSymKey](key_mgmt_util-genSymKey.md)
+ [importPrivateKey](key_mgmt_util-importPrivateKey.md)
+ [importPubKey](key_mgmt_util-importPubKey.md)
+ [imSymKey](key_mgmt_util-imSymKey.md)
+ [insertMaskedObject](key_mgmt_util-insertMaskedObject.md)
+ [unWrapKey](key_mgmt_util-unwrapKey.md)

## Synchronizing keys across cloned clusters<a name="cli-sync"></a>

Client\-side and server\-side synchronization are only for synchronizing keys within the *same* cluster\. If you copy a backup of a cluster to another region, you can use the syncKey command of the cloudhsm\_mgmt\_util \(CMU\) for synchronizing keys between clusters\. You might use cloned clusters for cross\-region redundancy or to simplify your disaster recovery process\. For more information, see [syncKey](cloudhsm_mgmt_util-syncKey.md)\.