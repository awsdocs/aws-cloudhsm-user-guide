# exportPrivateKey<a name="key_mgmt_util-exportPrivateKey"></a>

The exportPrivateKey command in key\_mgmt\_util exports an asymmetric private key in an HSM to a file\. You can use it to export private keys that you generate on the HSM\. You can also use the command to export private keys that were imported into an HSM, such as those imported with the [importPrivateKey](key_mgmt_util-importPrivateKey.md) command\.

During the export process, exportPrivateKey uses an AES key that you select \(the *wrapping key*\) to *wrap* \(encrypt\) the private key\. This way, the private key file maintains integrity during transit\. For more information, see [wrapKey](key_mgmt_util-wrapKey.md)\.

The exportPrivateKey command copies the key material to a file that you specify\. But it does not remove the key from the HSM, change its [key attributes](key-attribute-table.md), or prevent you from using the key in further cryptographic operations\. You can export the same key multiple times\.

You can only export public keys that have a `OBJ_ATTR_EXTRACTABLE` value of `1`\. To find a key's attributes, use the [getAttribute](key_mgmt_util-getAttribute.md) command\.

Before you run any key\_mgmt\_util command, you must [start key\_mgmt\_util](key_mgmt_util-getting-started.md#key_mgmt_util-start) and [log in](key_mgmt_util-getting-started.md#key_mgmt_util-log-in) to the HSM as a crypto user \(CU\)\.

## Syntax<a name="exportPrivateKey-syntax"></a>

```
exportPrivateKey -h

exportPrivateKey -k <private-key-handle
                 -w <wrapping-key-handle>
                 -out <key-file>
                 [-m <wrapping-mechanism>]
                 [-wk <wrapping-key-file>]
```

## Examples<a name="exportPrivateKey-examples"></a>

This example shows how to use exportPrivateKey to export a private key out of an HSM\.

**Example : Export a Private Key**  
This command exports a private key with handle `15` using a wrapping key with handle `16` to a PEM file called `exportKey.pem`\. When the command succeeds, exportPrivateKey returns a success message\.  

```
Command: exportPrivateKey -k 15 -w 16 -out exportKey.pem

Cfm3WrapKey returned: 0x00 : HSM Return: SUCCESS

        Cfm3UnWrapHostKey returned: 0x00 : HSM Return: SUCCESS

PEM formatted private key is written to exportKey.pem
```

## Parameters<a name="exportPrivateKey-parameters"></a>

This command takes the following parameters\.

**`-h`**  
Displays command line help for the command\.  
Required: Yes

**`-k`**  
Specifies the key handle of the private key to be exported\.  
Required: Yes

**`-w`**  
Specifies the key handle of the wrapping key\. This parameter is required\. To find key handles, use the [findKey](key_mgmt_util-findKey.md) command\.  
To determine whether a key can be used as a wrapping key, use [getAttribute](key_mgmt_util-getAttribute.md) to get the value of the `OBJ_ATTR_WRAP` attribute \(262\)\. To create a wrapping key, use [genSymKey](key_mgmt_util-genSymKey.md) to create an AES key \(type 31\)\.  
If you use the `-wk` parameter to specify an external unwrapping key, the `-w` wrapping key is used to wrap, but not unwrap, the key during export\.  
Required: Yes

**`-out`**  
Specifies the name of the file to which the exported private key will be written\.  
Required: Yes

**`-m`**  
Specifies the wrapping mechanism with which to wrap the private key being exported\. The only valid value is `4`, which represents the `NIST_AES_WRAP mechanism.`  
Default: 4 \(`NIST_AES_WRAP`\)  
Required: No

**`-wk`**  
Specifies the key to be used to unwrap the key being exported\. Enter the path and name of a file that contains a plaintext AES key\.  
When you include this parameter, exportPrivateKey uses the key in the `-w` file to wrap the key being exported and uses the key specified by the `-wk` parameter to unwrap it\.  
Default: Use the wrapping key specified in the `-w` parameter to both wrap and unwrap\.  
Required: No

## Related Topics<a name="exportPrivateKey-seealso"></a>
+ [importPrivateKey](key_mgmt_util-importPrivateKey.md)
+ [wrapKey](key_mgmt_util-wrapKey.md)
+ [unWrapKey](key_mgmt_util-unwrapKey.md)
+ [genSymKey](key_mgmt_util-genSymKey.md)