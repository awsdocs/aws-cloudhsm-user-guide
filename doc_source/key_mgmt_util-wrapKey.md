# wrapKey<a name="key_mgmt_util-wrapKey"></a>

The wrapKey command in key\_mgmt\_util exports an encrypted copy of a symmetric or private key from the HSM to a file\. When you run wrapKey, you specify the key to export, a key on the HSM to encrypt \(wrap\) the key that you want to export, and the output file\.

The `wrapKey` command writes the encrypted key to a file that you specify, but it does not remove the key from the HSM or prevent you from using it in cryptographic operations\. You can export the same key multiple times\. 

Only the owner of a key, that is, the crypto user \(CU\) who created the key, can export it\. Users who share the key can use it in cryptographic operations, but they cannot export it\.

To import the encrypted key back into the HSM, use [unWrapKey](key_mgmt_util-unwrapKey.md)\. To export a plaintext key from an HSM, use [exSymKey](key_mgmt_util-exSymKey.md) or [exportPrivateKey](key_mgmt_util-exportPrivateKey.md) as appropriate\. The [aesWrapUnwrap](key_mgmt_util-aesWrapUnwrap.md) command cannot decrypt \(unwrap\) keys that wrapKey encrypts\.

Before you run any key\_mgmt\_util command, you must [start key\_mgmt\_util](key_mgmt_util-getting-started.md#key_mgmt_util-start) and [log in](key_mgmt_util-getting-started.md#key_mgmt_util-log-in) to the HSM as a crypto user \(CU\)\. 

## Syntax<a name="wrapKey-syntax"></a>

```
wrapKey -h

wrapKey -k <exported-key-handle>
        -w <wrapping-key-handle>
        -out <output-file>
        [-m <wrapping-mechanism>]
        [-aad <additional authenticated data filename>]
        [-t <hash-type>]
        [-noheader]
        [-i <wrapping IV>]  
        [-iv_file <IV file>]
        [-tag_size <num_tag_bytes>>]
```

## Example<a name="wrapKey-examples"></a>

**Example**  
This command exports a 192\-bit Triple DES \(3DES\) symmetric key \(key handle `7`\)\. It uses a 256\-bit AES key in the HSM \(key handle `14`\) to wrap key `7`\. Then, it writes the encrypted 3DES key to the `3DES-encrypted.key` file\.  
The output shows that key `7` \(the 3DES key\) was successfully wrapped and written to the specified file\. The encrypted key is 307 bytes long\.  

```
        Command:  wrapKey -k 7 -w 14 -out 3DES-encrypted.key -m 4

        Key Wrapped.

        Wrapped Key written to file "3DES-encrypted.key length 307

        Cfm2WrapKey returned: 0x00 : HSM Return: SUCCESS
```

## Parameters<a name="wrapKey-params"></a>

**\-h**  
Displays help for the command\.   
Required: Yes

**\-k**  
The key handle of the key that you want to export\. Enter the key handle of a symmetric or private key that you own\. To find key handles, use the [findKey](key_mgmt_util-findKey.md) command\.  
To verify that a key can be exported, use the [getAttribute](key_mgmt_util-getAttribute.md) command to get the value of the `OBJ_ATTR_EXTRACTABLE` attribute, which is represented by constant `354`\. For help interpreting the key attributes, see the [Key Attribute Reference](key-attribute-table.md)\.  
You can export only those keys that you own\. To find the owner of a key, use the [getKeyInfo](key_mgmt_util-getKeyInfo.md) command\.  
Required: Yes

**\-w**  
Specifies the wrapping key\. Enter the key handle of an AES key or RSA key on the HSM\. This parameter is required\. To find key handles, use the [findKey](key_mgmt_util-findKey.md) command\.  
To create a wrapping key, use [genSymKey](key_mgmt_util-genSymKey.md) to generate an AES key \(type 31\) or [genRSAKeyPair](key_mgmt_util-genRSAKeyPair.md) to generate an RSA key pair \(type 0\)\. If you are using an RSA key pair, be sure to wrap the key with one of the keys, and unwrap it with the other\. To verify that a key can be used as a wrapping key, use [getAttribute](key_mgmt_util-getAttribute.md) to get the value of the `OBJ_ATTR_WRAP` attribute, which is represented by constant `262`\.  
Required: Yes

**\-out**  
The path and name of the output file\. When the command succeeds, this file contains an encrypted copy of the exported key\. If the file already exists, the command overwrites it without warning\.  
Required: Yes

**\-m**  
The value representing the wrapping mechanism\. CloudHSM supports the following mechanisms:       
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/key_mgmt_util-wrapKey.html)
Required: Yes

**\-t**  
The value representing the hash algorithm\. CloudHSM supports the following algorithms:      
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/key_mgmt_util-wrapKey.html)
Required: No  
When using the `RSA_OAEP` wrapping mechanism, the maximum key size that you can wrap is determined by the modulus of the RSA key and the length of the specified hash as follows: Maximum key size = \(modulusLengthInBytes\-2\*hashLengthInBytes\-2\)\.

**\-aad**  
The file name containing `AAD`\.  
Valid only for `AES_GCM` and `CLOUDHSM_AES_GCM` mechanisms\.
Required: No

**\-noheader**  
Omits the header that specifies CloudHSM\-specific [key attributes](key_mgmt_util-reference.md)\. Use this parameter *only* if you want to unwrap the key with tools outside of key\_mgmt\_util\.  
Required: No

**\-i**  
The initialization vector \(IV\) \(hex value\)\.  
Valid only when passed with the `-noheader` parameter for `CLOUDHSM_AES_KEY_WRAP`, `NIST_AES_WRAP`, and `NIST_TDEA_WRAP` mechanisms\.
Required: No

**\-iv\_file**  
The file in which you want to write the IV value obtained in response\.  
Valid only when passed with the `-noheader` parameter for `AES_GCM` mechanism\.
Required: No

**\-tag\_size**  
The size of tag to be saved along with wrapped blob\.  
Valid only when passed with the `-noheader` parameter for `AES_GCM` and `CLOUDHSM_AES_GCM` mechanisms\. Minimum tag size is eight\.
Required: No

## Related topics<a name="wrapKey-seealso"></a>
+ [exSymKey](key_mgmt_util-exSymKey.md)
+ [imSymKey](key_mgmt_util-imSymKey.md)
+ [unWrapKey](key_mgmt_util-unwrapKey.md)