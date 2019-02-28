# sign<a name="key_mgmt_util-sign"></a>

The sign command in key\_mgmt\_util uses a chosen private key to generate a signature for a file\.

In order to use sign, you must first have a private key in your HSM\. You can generate a private key with the [genSymKey](key_mgmt_util-genSymKey.md), [genRSAKeyPair](key_mgmt_util-genRSAKeyPair.md), or [genECCKeyPair](key_mgmt_util-genECCKeyPair.md) commands\. You can also import one with the [importPrivateKey](key_mgmt_util-importPrivateKey.md) command\. For more information, see [Generate Keys](manage-keys.md#generate-keys)\.

The sign command uses a user\-designated signing mechanism, represented by an integer, to sign a message file\. For a list of possible signing mechanisms, see [Parameters](#sign-parameters)\.

Before you run any key\_mgmt\_util command, you must [start key\_mgmt\_util](key_mgmt_util-getting-started.md#key_mgmt_util-start) and [log in](key_mgmt_util-getting-started.md#key_mgmt_util-log-in) to the HSM as a crypto user \(CU\)\.

## Syntax<a name="sign-syntax"></a>

```
sign -h

sign -f <file name>
     -k <private key handle>
     -m <signature mechanism>
     -out <signed file name>
```

## Example<a name="sign-examples"></a>

This example shows how to use sign to sign a file\.

**Example : Sign a file**  
This command signs a file named `messageFile` with a private key with handle `266309`\. It uses the `SHA256_RSA_PKCS` \(`1`\) signing mechanism and saves the resulting signed file as `signedFile`\.  

```
Command: sign -f messageFile -k 266309 -m 1 -out signedFile

Cfm3Sign returned: 0x00 : HSM Return: SUCCESS

signature is written to file signedFile

Cluster Error Status
Node id 0 and err state 0x00000000 : HSM Return: SUCCESS
Node id 1 and err state 0x00000000 : HSM Return: SUCCESS
Node id 2 and err state 0x00000000 : HSM Return: SUCCESS
```

## Parameters<a name="sign-parameters"></a>

This command takes the following parameters\.

**`-f`**  
The name of the file to sign\.  
Required: Yes

**`-k`**  
The handle of the private key to be used for signing\.  
Required: Yes

**`-m`**  
An integer that represents the signing mechanism to be used for signing\. The possible mechanisms correspond to the follow integers:      
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/key_mgmt_util-sign.html)
Required: Yes

**`-out`**  
The name of the file to which the signed file will be saved\.  
Required: Yes

## Related Topics<a name="sign-seealso"></a>
+ [verify](key_mgmt_util-verify.md)
+ [importPrivateKey](key_mgmt_util-importPrivateKey.md)
+ [genRSAKeyPair](key_mgmt_util-genRSAKeyPair.md)
+ [genECCKeyPair](key_mgmt_util-genECCKeyPair.md)
+ [genSymKey](key_mgmt_util-genSymKey.md)
+ [Generate Keys](manage-keys.md#generate-keys)