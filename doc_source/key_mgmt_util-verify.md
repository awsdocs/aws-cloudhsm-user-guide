# verify<a name="key_mgmt_util-verify"></a>

The verify command in key\_mgmt\_util confirms whether or not a file has been signed by a given key\. To do so, the verify command compares a signed file against a source file and analyzes whether they are cryptographically related based on a given public key and signing mechanism\. Files can be signed in AWS CloudHSM with the [sign](key_mgmt_util-sign.md) operation\.

Signing mechanisms are represented by the integers listed in the [parameters](#verify-parameters) section\.

Before you run any key\_mgmt\_util command, you must [start key\_mgmt\_util](key_mgmt_util-getting-started.md#key_mgmt_util-start) and [log in](key_mgmt_util-getting-started.md#key_mgmt_util-log-in) to the HSM as a crypto user \(CU\)\.

## Syntax<a name="verify-syntax"></a>

```
verify -h

verify -f <message-file>
       -s <signature-file>
       -k <public-key-handle>
       -m <signature-mechanism>
```

## Example<a name="verify-examples"></a>

These examples show how to use verify to check whether a certain public key was used to sign a given file\.

**Example : Successfully Verify a File Signature**  
This command attempts to verify whether a file named `hardwarCert.crt` was signed by public key `262276` using the `SHA256_RSA_PKCS` signing mechanism to produce the `hardwareCertSigned` signed file\. Because the given parameters represent a true signing relationship, the command returns a success message\.  

```
Command: verify -f hardwareCert.crt -s hardwareCertSigned -k 262276 -m 1

Signature verification successful

Cfm3Verify returned: 0x00 : HSM Return: SUCCESS
```

**Example : Prove False Signing Relationship**  
This command verifies whether a file named `hardwareCert.crt` was signed by public key `262276` using the `SHA256_RSA_PKCS` signing mechanism to produce the `userCertSigned` signed file\. Because the given parameters do not make up a true signing relationship, the command returns an error message\.  

```
Command: verify -f hardwarecert.crt -s usercertsigned -k 262276 -m 1
Cfm3Verify rturned: 0x1b

CSP Error: ERR_BAD_PKCS_DATA
```

## Parameters<a name="verify-parameters"></a>

This command takes the following parameters\.

**`-f`**  
The name of the origin message file\.  
Required: Yes

**`-s`**  
The name of the signed file\.  
Require: Yes

**`-k`**  
The handle of the public key that is thought to be used to sign the file\.  
Required: Yes

**`-m`**  
An integer that represents the proposed signing mechanism that is used to sign the file\. The possible mechanisms correspond to the follow integers:      
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/key_mgmt_util-verify.html)
Required: Yes

## Related Topics<a name="verify-seealso"></a>
+ [sign](key_mgmt_util-sign.md)
+ [getCert](key_mgmt_util-genECCKeyPair.md)
+ [Generate Keys](manage-keys.md#generate-keys)