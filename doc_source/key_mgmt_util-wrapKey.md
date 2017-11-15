# wrapKey<a name="key_mgmt_util-wrapKey"></a>

The `wrapKey` command in key\_mgmt\_util exports an encrypted copy of a key from the HSM to a file on disk\. When you run `wrapKey`, you specify a symmetric or private key to export, an AES key on the HSM to encrypt \(wrap\) the key to be exported, and the output file\. 

The `wrapKey` command writes the encrypted key to a file that you specify, but it does not remove the key from the HSM, change its key attributes, or prevent you from using it in cryptographic operations\. You can export the same key multiple times\. 

To import \(and unwrap\) the encrypted key from the file to an HSM, use unWrapKey\. To export a plaintext key from the HSM, use exSymKey\. The aesWrapUnwrap command cannot decrypt \(unwrap\) keys that `wrapkey` encrypts\.

Before you run any key\_mgmt\_util command, you must start key\_mgmt\_util and login to the HSM as a crypto user \(CU\)\. 

## Syntax<a name="wrapKey-syntax"></a>

```
wrapKey -h

wrapKey -k <exported-key-handle>
        -w <wrapping-key-handle>
        -out <output-file>
```

## Examples<a name="wrapKey-examples"></a>

These examples show how to use `wrapKey` to export an encrypted key to a file\. 

**Example : Export a wrapped symmetric key**  
This command exports a 192\-bit Triple DES \(3DES\) symmetric key \(key handle 7\)\. It uses a 256\-bit AES key in the HSM \(key handle 6\) to wrap key 7\. Then, it writes the encrypted 3DES key to the `3DES-encrypted.key` file\.  
The output shows that key 7 \(the 3DES key\) was successfully wrapped and written to the specified file\. The encrypted key is 307 bytes long\.  

```
        Command:  wrapKey -k 7 -w 6 -out 3DES-encrypted.key
                       
        Key Wrapped.

        Wrapped Key written to file "3DES-encrypted.key length 307

        Cfm2WrapKey returned: 0x00 : HSM Return: SUCCESS
```

## Parameters<a name="wrapKey-params"></a>

**\-h**  
Displays help for the command\.   
Required: Yes

**\-k**  
Specifies the key handle of the key to export\. Enter the key handle of a symmetric or private key\. This parameter is required\. To find key handles, use the findKey command\.  
To verify that a key can be exported, use the getAttribute command to get the value of the `OBJ_ATTR_EXTRACTABLE` attribute, which is represented by constant `354`\. For help interpreting the key attributes, see the [Key Attribute Reference](key-attribute-table.md)\.  
Required: Yes

**\-w**  
Specifies the wrapping key\. Enter the key handle of an AES key on the HSM\. This parameter is required\. To find key handles, use the findKey command\.  
To verify that a key can be used as a wrapping key, use getAttribute to get the value of the `OBJ_ATTR_WRAP` attribute, which is represented by constant `262`\. To create a wrapping key, use genSymKey to create an AES key \(type 31\)\.  
Key handle 4, which represents a static key encryption key \(KEK\), is not valid on FIPS\-mode HSMs\.  
Required: Yes

**\-out**  
Specifies the path and name of the output file\. When the command succeeds, this file contains an encrypted copy of the exported key\. If the file already exists, the command overwrites it without warning\.  
Required: Yes

## Related Topics<a name="wrapKey-seealso"></a>

+ exSymKey

+ unWrapKey