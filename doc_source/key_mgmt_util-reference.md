# key\_mgmt\_util command reference<a name="key_mgmt_util-reference"></a>

The **key\_mgmt\_util** command line tool helps you to manage keys in the HSMs in your cluster, including creating, deleting, and finding keys and their attributes\. It includes multiple commands, each of which is described in detail in this topic\.

For a quick start, see [Getting started with key\_mgmt\_util](key_mgmt_util-getting-started.md)\. For help interpreting the key attributes, see the [Key Attribute Reference](key-attribute-table.md)\. For information about the cloudhsm\_mgmt\_util command line tool, which includes commands to manage the HSM and users in your cluster, see [CloudHSM Management Utility \(CMU\)](cloudhsm_mgmt_util.md)\. 

Before you run any key\_mgmt\_util command, you must [start key\_mgmt\_util](key_mgmt_util-getting-started.md#key_mgmt_util-start) and [log in](key_mgmt_util-getting-started.md#key_mgmt_util-log-in) to the HSM as a crypto user \(CU\)\. 

To list all key\_mgmt\_util commands, type:

```
Command: help
```

To get help for a particular key\_mgmt\_util command, type:

```
Command: <command-name> -h
```

To end your key\_mgmt\_util session, type:

```
Command: exit
```

The following topics describe commands in key\_mgmt\_util\.

**Note**  
Some commands in key\_mgmt\_util and cloudhsm\_mgmt\_util have the same names\. However, the commands typically have different syntax, different output, and slightly different functionality\.


| Command | Description | 
| --- | --- | 
|  [aesWrapUnwrap](key_mgmt_util-aesWrapUnwrap.md) | Encrypts and decrypts the contents of a key in a file\. | 
| [deleteKey](key_mgmt_util-deleteKey.md) | Deletes a key from the HSMs\. | 
| [Error2String](key_mgmt_util-Error2String.md) | Gets the error that corresponds to a key\_mgmt\_util hexadecimal error code\. | 
| [exit](key_mgmt_util-exit.md) | Exits the key\_mgmt\_util\. | 
| [exportPrivateKey](key_mgmt_util-exportPrivateKey.md) | Exports a copy of a private key from an HSM to a file on disk\. | 
| [exportPubKey](key_mgmt_util-exportPubKey.md) | Exports a copy of a public key from an HSM to a file\. | 
| [exSymKey](key_mgmt_util-exSymKey.md) | Exports a plaintext copy of a symmetric key from the HSMs to a file\. | 
| [extractMaskedObject](key_mgmt_util-extractMaskedObject.md) | Extracts a key from an HSM as a masked object file\. | 
| [findKey](key_mgmt_util-findKey.md) | Search for keys by key attribute value\. | 
| [findSingleKey](key_mgmt_util-findSingleKey.md) |  Verifies that a key exists on all HSMs in the cluster\. | 
| [genDSAKeyPair](key_mgmt_util-genDSAKeyPair.md) |  Generates a [Digital Signing Algorithm](https://en.wikipedia.org/wiki/Digital_Signature_Algorithm) \(DSA\) key pair in your HSMs\. | 
| [genECCKeyPair](key_mgmt_util-genECCKeyPair.md) |  Generates an [Elliptic Curve Cryptography](https://en.wikipedia.org/wiki/Elliptic-curve_cryptography) \(ECC\) key pair in your HSMs\. | 
| [genPBEKey](key_mgmt_util-genPBEKey.md) | \(This command is not supported on the FIPS\-validated HSMs\.\) | 
| [genRSAKeyPair](key_mgmt_util-genRSAKeyPair.md) |  Generates an [RSA](https://en.wikipedia.org/wiki/RSA_%28cryptosystem%29) asymmetric key pair in your HSMs\. | 
| [genSymKey](key_mgmt_util-genSymKey.md) |  Generates a symmetric key in your HSMs | 
| [getAttribute](key_mgmt_util-getAttribute.md) |  Gets the attribute values for an AWS CloudHSM key and writes them to a file\. | 
| [getCaviumPrivKey](key_mgmt_util-getCaviumPrivKey.md) |  Creates a fake PEM\-format version of a private key and exports it to a file\.  | 
| [getCert](key_mgmt_util-getCert.md) |  Retrieves an HSM's partitions certificates and saves them to a file\.  | 
| [getKeyInfo](key_mgmt_util-getKeyInfo.md) |  Gets the HSM user IDs of users who can use the key\.  If the key is quorum controlled, it gets the number of users in the quorum\. | 
| [help](key_mgmt_util-help.md) |  Displays help information about the commands available in key\_mgmt\_util\.  | 
| [importPrivateKey](key_mgmt_util-importPrivateKey.md) | Imports a private key into an HSM\. | 
| [importPubKey](key_mgmt_util-importPubKey.md) | Imports a public key into an HSM\. | 
| [imSymKey](key_mgmt_util-imSymKey.md) |  Imports a plaintext copy of a symmetric key from a file into the HSM\.  | 
| [insertMaskedObject](key_mgmt_util-insertMaskedObject.md) | Inserts a masked object from a file on disk into an HSM contained by related cluster to the object's origin cluster\. Related clusters are any clusters [generated from a backup of the origin cluster](create-cluster-from-backup.md)\. | 
| [IsValidKeyHandlefile](key_mgmt_util-IsValidKeyHandlefile.md) | Determines whether or not a given file contains a real private key or a fake PEM key\. | 
| [listAttributes](key_mgmt_util-listAttributes.md) |  Lists the attributes of an AWS CloudHSM key and the constants that represent them\. | 
| [listUsers](key_mgmt_util-listUsers.md) |  Gets the users in the HSMs, their user type and ID, and other attributes\.  | 
| [loginHSM and logoutHSM](key_mgmt_util-loginHSM.md) |  Log in and out of the HSMs in a cluster\.  | 
| [setAttribute](key_mgmt_util-setAttribute.md) | Converts a session key to a persistent key\. | 
| [sign](key_mgmt_util-sign.md) | Generate a signature for a file using a chosen private key\. | 
| [unWrapKey](key_mgmt_util-unwrapKey.md) |  Imports a wrapped \(encrypted\) key from a file into the HSMs\. | 
| [verify](key_mgmt_util-verify.md) | Verifies whether a given key was used to sign a given file\. | 
| [wrapKey](key_mgmt_util-wrapKey.md) |  Exports an encrypted copy of a key from the HSM to a file\.  | 