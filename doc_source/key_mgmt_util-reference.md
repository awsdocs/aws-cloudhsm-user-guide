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

For help interpreting the key attributes, see the [Key Attribute Reference](key-attribute-table.md)\.

The following topics describe commands in key\_mgmt\_util\.


| Command | Description | 
| --- | --- | 
|  aesWrapUnwrap | Encrypts and decrypts the contents of a key in a file on disk\. | 
| deleteKey | Deletes a key from the HSMs\. | 
| Error2String | Gets the error that corresponds to a key\_mgmt\_util hexadecimal error code\. | 
| exSymKey | Exports a plaintext copy of a symmetric key from the HSMs to a file on disk\. | 
| findKey | Search for keys by key attribute value\. | 
| findSingleKey |  Verifies that a key exists on all HSMs in the cluster\. | 
| genDSAKeyPair |  Generates a [Digital Signing Algorithm](https://en.wikipedia.org/wiki/Digital_Signature_Algorithm) \(DSA\) key pair in your HSMs\. | 
| genECCKeyPair |  Generates an [Elliptic Curve Cryptography](https://en.wikipedia.org/wiki/Elliptic-curve_cryptography) \(ECC\) key pair in your HSMs\. | 
| genPBEKey | \(This command is not supported on the FIPS\-validated HSMs\.\) | 
| genRSAKeyPair |  Generates an [RSA](https://en.wikipedia.org/wiki/RSA_%28cryptosystem%29) asymmetric key pair in your HSMs\. | 
| genSymKey |  Generates a symmetric key in your HSMs | 
| getAttribute |  Gets the attribute values for an AWS CloudHSM key and writes them to a file\. | 
| getKeyInfo |  Gets the HSM user IDs of users who can use the key\.  If the key is quorum controlled, it gets the number of users in the quorum\. | 
| imSymKey |  Imports a plaintext copy of a symmetric key from a file into the HSM\.  | 
| listAttributes |  Lists the attributes of an AWS CloudHSM key and the constants that represent them\. | 
| listUsers |  Gets the users in the HSMs, their user type and ID, and other attributes\.  | 
| setAttribute | Converts a session key to a persistent key\. | 
| unWrapKey |  Imports a wrapped \(encrypted\) key from a file into the HSMs\. | 
| wrapKey |  Exports an encrypted copy of a key from the HSM to a file on disk  | 