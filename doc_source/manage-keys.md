# Managing Keys in AWS CloudHSM<a name="manage-keys"></a>

To manage keys on the HSMs in your AWS CloudHSM cluster, use the AWS CloudHSM command line tool known as key\_mgmt\_util\. Before you can manage keys, you must start the AWS CloudHSM client, start key\_mgmt\_util, and log in to the HSMs\. For more information, see [key\_mgmt\_util](key_mgmt_util.md)\.

To manage keys, log in to the HSM with the user name and password of a crypto user \(CU\)\. Only CUs can create keys\. Keys are inherently owned and can be managed by the CU that created them\.


+ [Generate Keys](#generate-keys)
+ [Import Keys](#import-keys)
+ [Export Keys](#export-keys)
+ [Delete Keys](#delete-keys)
+ [Share and Unshare Keys](#share-keys)

## Generate Keys<a name="generate-keys"></a>

To generate keys on the HSM, use the command that corresponds to the kind of key that you want to generate\.

### Generate Symmetric Keys<a name="generate-symmetric-keys"></a>

Use the genSymKey command to generate AES, triple DES, and other types of symmetric keys\. To see all available options, use the genSymKey \-h command\.

The following example creates a 256\-bit AES key\.

```
Command: genSymKey -t 31 -s 32 -l aes256
Cfm3GenerateSymmetricKey returned: 0x00 : HSM Return: SUCCESS

Symmetric Key Created.  Key Handle: 524295

Cluster Error Status
Node id 0 and err state 0x00000000 : HSM Return: SUCCESS
Node id 1 and err state 0x00000000 : HSM Return: SUCCESS
Node id 2 and err state 0x00000000 : HSM Return: SUCCESS
```

### Generate RSA Key Pairs<a name="generate-rsa-key-pairs"></a>

To generate an RSA key pair, use the genRSAKeyPair command\. To see all available options, use the genRSAKeyPair \-h command\.

The following example generates an RSA 2048\-bit key pair\.

```
Command: genRSAKeyPair -m 2048 -e 65537 -l rsa2048
Cfm3GenerateKeyPair returned: 0x00 : HSM Return: SUCCESS

Cfm3GenerateKeyPair:    public key handle: 524294    private key handle: 524296

Cluster Error Status
Node id 0 and err state 0x00000000 : HSM Return: SUCCESS
Node id 1 and err state 0x00000000 : HSM Return: SUCCESS
Node id 2 and err state 0x00000000 : HSM Return: SUCCESS
```

### Generate ECC \(Elliptic Curve Cryptography\) Key Pairs<a name="generate-ecc-key-pairs"></a>

To generate an ECC key pair, use the genECCKeyPair command\. To see all available options, including a list of the supported elliptic curves, use the genECCKeyPair \-h command\.

The following example generates an ECC key pair using the P\-384 elliptic curve defined in [NIST FIPS publication 186\-4](http://dx.doi.org/10.6028/NIST.FIPS.186-4)\.

```
Command: genECCKeyPair -i 14 -l ecc-p384
Cfm3GenerateKeyPair returned: 0x00 : HSM Return: SUCCESS

Cfm3GenerateKeyPair:    public key handle: 524297    private key handle: 524298

Cluster Error Status
Node id 0 and err state 0x00000000 : HSM Return: SUCCESS
Node id 1 and err state 0x00000000 : HSM Return: SUCCESS
Node id 2 and err state 0x00000000 : HSM Return: SUCCESS
```

## Import Keys<a name="import-keys"></a>

To import secret keys—that is, symmetric keys and asymmetric private keys—into the HSM, you must first create a wrapping key\. You can import public keys directly without a wrapping key\.

### Import Secret Keys<a name="import-secret-keys"></a>

Complete the following steps to import a secret key\. Before you import a secret key, save it to a file\. Save symmetric keys as raw bytes, and asymmetric private keys in PEM format\.

**To import a secret key**

1. Use the genSymKey command to create a wrapping key\. The following command creates a 128\-bit AES wrapping key that is valid only for the current session\.

   ```
   Command: genSymKey -t 31 -s 16 -sess -l import-wrapping-key
   Cfm3GenerateSymmetricKey returned: 0x00 : HSM Return: SUCCESS
   
   Symmetric Key Created.  Key Handle: 524299
   
   Cluster Error Status
   Node id 2 and err state 0x00000000 : HSM Return: SUCCESS
   ```

1. Use one of the following commands, depending on the type of secret key that you are importing\.

   + To import a symmetric key, use the imSymKey command\. The following command imports an AES key from a file named `aes256.key` using the wrapping key created in the previous step\. To see all available options, use the imSymKey \-h command\.

     ```
     Command: imSymKey -f aes256.key -t 31 -l aes256-imported -w 524299
     Cfm3WrapHostKey returned: 0x00 : HSM Return: SUCCESS
     
     Cfm3CreateUnwrapTemplate returned: 0x00 : HSM Return: SUCCESS
     
     Cfm3UnWrapKey returned: 0x00 : HSM Return: SUCCESS
     
     Symmetric Key Unwrapped.  Key Handle: 524300
     
     Cluster Error Status
     Node id 0 and err state 0x00000000 : HSM Return: SUCCESS
     Node id 1 and err state 0x00000000 : HSM Return: SUCCESS
     Node id 2 and err state 0x00000000 : HSM Return: SUCCESS
     ```

   + To import an asymmetric private key, use the importPrivateKey command\. The following command imports a private key from a file named `rsa2048.key` using the wrapping key created in the previous step\. To see all available options, use the importPrivateKey \-h command\.

     ```
     Command: importPrivateKey -f rsa2048.key -l rsa2048-imported -w 524299
     BER encoded key length is 1216
     
     Cfm3WrapHostKey returned: 0x00 : HSM Return: SUCCESS
     
     Cfm3CreateUnwrapTemplate returned: 0x00 : HSM Return: SUCCESS
     
     Cfm3UnWrapKey returned: 0x00 : HSM Return: SUCCESS
     
     Private Key Unwrapped.  Key Handle: 524301
     
     Cluster Error Status
     Node id 0 and err state 0x00000000 : HSM Return: SUCCESS
     Node id 1 and err state 0x00000000 : HSM Return: SUCCESS
     Node id 2 and err state 0x00000000 : HSM Return: SUCCESS
     ```

### Import Public Keys<a name="import-public-keys"></a>

Use the importPubKey command to import a public key\. To see all available options, use the importPubKey \-h command\.

The following example imports an RSA public key from a file named `rsa2048.pub`\.

```
Command: importPubKey -f rsa2048.pub -l rsa2048-public-imported
Cfm3CreatePublicKey returned: 0x00 : HSM Return: SUCCESS

Public Key Handle: 524302

Cluster Error Status
Node id 0 and err state 0x00000000 : HSM Return: SUCCESS
Node id 1 and err state 0x00000000 : HSM Return: SUCCESS
Node id 2 and err state 0x00000000 : HSM Return: SUCCESS
```

## Export Keys<a name="export-keys"></a>

To export secret keys—that is, symmetric keys and asymmetric private keys—from the HSM, you must first create a wrapping key\. You can export public keys directly without a wrapping key\.

**Important**  
When you export a secret key, the end result is a plaintext \(unencrypted\) key\. The process requires a wrapping key, but the end result is ***not*** a wrapped key\.

### Export Secret Keys<a name="export-secret-keys"></a>

Complete the following steps to export a secret key\.

**To export a secret key**

1. Use the genSymKey command to create a wrapping key\. The following command creates a 128\-bit AES wrapping key that is valid only for the current session\.

   ```
   Command: genSymKey -t 31 -s 16 -sess -l export-wrapping-key
   Cfm3GenerateSymmetricKey returned: 0x00 : HSM Return: SUCCESS
   
   Symmetric Key Created.  Key Handle: 524304
   
   Cluster Error Status
   Node id 2 and err state 0x00000000 : HSM Return: SUCCESS
   ```

1. Use one of the following commands, depending on the type of secret key that you are exporting\.

   + To export a symmetric key, use the exSymKey command\. The following command exports an AES key to a file named `aes256.key.exp`\. To see all available options, use the exSymKey \-h command\.

     ```
     Command: exSymKey -k 524295 -out aes256.key.exp -w 524304
     Cfm3WrapKey returned: 0x00 : HSM Return: SUCCESS
     
     Cfm3UnWrapHostKey returned: 0x00 : HSM Return: SUCCESS
     
     
     Wrapped Symmetric Key written to file "aes256.key.exp"
     ```
**Note**  
The command's output says that a "Wrapped Symmetric Key" is written to the output file\. However, the output file contains a plaintext \(not wrapped\) key\.

   + To export a private key, use the exportPrivateKey command\. The following command exports a private key to a file named `rsa2048.key.exp`\. To see all available options, use the exportPrivateKey \-h command\.

     ```
     Command: exportPrivateKey -k 524296 -out rsa2048.key.exp -w 524304
     Cfm3WrapKey returned: 0x00 : HSM Return: SUCCESS
     
     Cfm3UnWrapHostKey returned: 0x00 : HSM Return: SUCCESS
     
     PEM formatted private key is written to rsa2048.key.exp
     ```

### Export Public Keys<a name="export-public-keys"></a>

Use the exportPubKey command to export a public key\. To see all available options, use the exportPubKey \-h command\.

The following example exports an RSA public key to a file named `rsa2048.pub.exp`\.

```
Command: exportPubKey -k 524294 -out rsa2048.pub.exp
PEM formatted public key is written to rsa2048.pub.key

Cfm3ExportPubKey returned: 0x00 : HSM Return: SUCCESS
```

## Delete Keys<a name="delete-keys"></a>

Use the deleteKey command to delete a key, as in the following example\.

```
Command: deleteKey -k 524300
Cfm3DeleteKey returned: 0x00 : HSM Return: SUCCESS

Cluster Error Status
Node id 0 and err state 0x00000000 : HSM Return: SUCCESS
Node id 1 and err state 0x00000000 : HSM Return: SUCCESS
Node id 2 and err state 0x00000000 : HSM Return: SUCCESS
```

## Share and Unshare Keys<a name="share-keys"></a>

To share keys with a different HSM user, use the AWS CloudHSM command line tool known as cloudhsm\_mgmt\_util\. This is different from most of the tasks documented in this section, which use the AWS CloudHSM command line tool known as key\_mgmt\_util\.

Before you can share a key, you must start cloudhsm\_mgmt\_util, enable end\-to\-end encryption, and log in to the HSMs\. For more information, see [Getting Started with cloudhsm\_mgmt\_util](cloudhsm_mgmt_util.md)\.

To share a key, log in to the HSM as the crypto user \(CU\) that owns the key\. Only key owners can share a key\.

Use the shareKey command to share or unshare a key, specifying the handle of the key and the IDs of the user or users\. To share or unshare with more than one user, specify a comma\-separated list of user IDs\. To share a key, use `1` as the command's last parameter, as in the following example\. To unshare, use `0`\.

```
aws-cloudhsm>shareKey 524295 4 1
*************************CAUTION********************************
This is a CRITICAL operation, should be done on all nodes in the
cluster. Cav server does NOT synchronize these changes with the
nodes on which this operation is not executed or failed, please
ensure this operation is executed on all nodes in the cluster.
****************************************************************

Do you want to continue(y/n)?y
shareKey success on server 0(10.0.2.9)
shareKey success on server 1(10.0.3.11)
shareKey success on server 2(10.0.1.12)
```

The following shows the syntax for the shareKey command\.

```
aws-cloudhsm>shareKey <key handle> <user ID> <Boolean: 1 for share, 0 for unshare>
```