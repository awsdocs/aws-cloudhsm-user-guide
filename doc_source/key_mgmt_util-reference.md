# key\_mgmt\_util Command Reference<a name="key_mgmt_util-reference"></a>

The **key\_mgmt\_util** command line tool helps you to manage keys in the HSMs in your cluster, including creating, deleting, and finding keys and their attributes\. It includes multiple commands, each of which is described in detail in this topic\. For a quick start, see [Getting Started with key\_mgmt\_util](key_mgmt_util-getting-started.md)\. For help interpreting the key attributes, see the [Key Attribute Reference](key-attribute-table.md)\.

To list all key\_mgmt\_util commands, type:

```
Command: help
```

To get help for a particular key\_mgmt\_util command, type:

```
Command: <command-name> -h
```

For information about the cloudhsm\_mgmt\_util command line tool, which includes commands to manage the HSM and users in your cluster, see [Getting Started with cloudhsm\_mgmt\_util](cloudhsm_mgmt_util.md)\.

The following topics describe commands in key\_mgmt\_util\.


+ [aesWrapUnwrap](key_mgmt_util-aesWrapUnwrap.md)
+ [deleteKey](key_mgmt_util-deleteKey.md)
+ [Error2String](key_mgmt_util-Error2String.md)
+ [exSymKey](key_mgmt_util-exSymKey.md)
+ [findKey](key_mgmt_util-findKey.md)
+ [findSingleKey](key_mgmt_util-findSingleKey.md)
+ [genDSAKeyPair](key_mgmt_util-genDSAKeyPair.md)
+ [genPBEKey](key_mgmt_util-genPBEKey.md)
+ [genRSAKeyPair](key_mgmt_util-genRSAKeyPair.md)
+ [genSymKey](key_mgmt_util-genSymKey.md)
+ [getAttribute](key_mgmt_util-getAttribute.md)
+ [getKeyInfo](key_mgmt_util-getKeyInfo.md)
+ [listAttributes](key_mgmt_util-listAttributes.md)
+ [listUsers](key_mgmt_util-listUsers.md)
+ [setAttribute](key_mgmt_util-setAttribute.md)
+ [unWrapKey](key_mgmt_util-unwrapKey.md)
+ [wrapKey](key_mgmt_util-wrapKey.md)
+ [Key Attribute Reference](key-attribute-table.md)