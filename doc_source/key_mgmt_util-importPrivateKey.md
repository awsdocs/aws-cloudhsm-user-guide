# importPrivateKey<a name="key_mgmt_util-importPrivateKey"></a>

The importPrivateKey command in key\_mgmt\_util imports an asymmetric private key into an HSM\. You can use it to import private keys that were generated outside of the HSM\. You can also use this command import keys that were exported from an HSM, such as those exported by the [exportPrivateKey](key_mgmt_util-exportPrivateKey.md) command\.

During the import process, importPrivateKey uses an AES key that you select \(the *wrapping key*\) to *wrap* \(encrypt\) the private key\. This way, the private key file maintains integrity during transit\. For more information, see [wrapKey](key_mgmt_util-wrapKey.md)\.

Before you run any key\_mgmt\_util command, you must [start key\_mgmt\_util](key_mgmt_util-getting-started.md#key_mgmt_util-start) and [log in](key_mgmt_util-getting-started.md#key_mgmt_util-log-in) to the HSM as a crypto user \(CU\)\.

## Syntax<a name="importPrivateKey-syntax"></a>

```
importPrivateKey -h

importPrivateKey -l <label>
                 -f <key-file>
                 -w <wrapping-key-handle>
                 [-sess]
                 [-id <key-id>]
                 [-m_value <0...8>]
                 [min_srv <minimum-number-of-servers>]
                 [-timeout <number-of-seconds>]
                 [-u <user-ids>]
                 [-wk <wrapping-key-file>]
                 [-attest]
```

## Examples<a name="importPrivateKey-examples"></a>

This example shows how to use importPrivateKey to import a private key into an HSM\.

**Example : Import a Private Key**  
This command imports the private key from a file named `rsa2048.key` with the label `rsa2048-imported` and a wrapping key with handle `524299`\. When the command succeeds, importPrivateKey returns a key handle for the imported key and a success message\.  

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

## Parameters<a name="importPrivateKey-parameters"></a>

This command takes the following parameters\.

**`-h`**  
Displays command line help for the command\.  
Required: Yes

**`-l`**  
Specifies the user\-defined private key label\.  
Required: Yes

**`-f`**  
Specifies the file name of the key to import\.  
Required: Yes

**`-w`**  
Specifies the key handle of the wrapping key\. This parameter is required\. To find key handles, use the [findKey](key_mgmt_util-findKey.md) command\.  
To determine whether a key can be used as a wrapping key, use [getAttribute](key_mgmt_util-getAttribute.md) to get the value of the `OBJ_ATTR_WRAP` attribute \(262\)\. To create a wrapping key, use [genSymKey](key_mgmt_util-genSymKey.md) to create an AES key \(type 31\)\.  
If you use the `-wk` parameter to specify an external unwrapping key, the `-w` wrapping key is used to wrap, but not unwrap, the key during import\.  
Required: Yes

**`-sess`**  
Specifies the imported key as a session key\.  
Default: The imported key is held as a persistent \(token\) key in the cluster\.  
Required: No

**`-id`**  
Specifies the ID of the key to be imported\.  
Default: No ID value\.  
Required: No

**`-m_value`**  
Specifies the number of users who must approve any cryptographic operation that uses the imported key\. Enter a value from **0** to **8**\.  
This parameter is valid only when the `-u` parameter in the command shares the key with enough users to satisfy the `m_value` requirement\.  
Default: 0  
Required: No

**`-min_srv`**  
Specifies the minimum number of HSMs on which the imported key is synchronized before the value of the `-timeout` parameter expires\. If the key is not synchronized to the specified number of servers in the time allotted, it is not created\.  
AWS CloudHSM automatically synchronizes every key to every HSM in the cluster\. To speed up your process, set the value of `min_srv` to less than the number of HSMs in the cluster and set a low timeout value\. Note, however, that some requests might not generate a key\.  
Default: 1  
Required: No

**`-timout`**  
Specifies the number of seconds to wait for the key to sync across HSMs when the `min-serv` parameter is included\. If no number is specified, the polling continues forever\.  
Default: No limit  
Required: No

**`-u`**  
Specifies the list of users with whom to share the imported private key\. This parameter gives other HSM crypto users \(CUs\) permission to use the imported key in cryptographic operations\.  
Enter a comma\-separated list of HSM user IDs, such as `-u 5,6`\. Do not include the HSM user ID of the current user\. To find the HSM user IDs of CUs on the HSM, use [listUsers](key_mgmt_util-listUsers.md)\.  
Default: Only the current user can use the imported key\.  
Required: No

**`-wk`**  
Specifies the key to be used to wrap the key that is being imported\. Enter the path and name of a file that contains a plaintext AES key\.  
When you include this parameter, importPrivateKey uses the key in the `-wk` file to wrap the key being imported\. It also uses the key specified by the `-w` parameter to unwrap it\.  
Default: Use the wrapping key specified in the `-w` parameter to both wrap and unwrap\.  
Required: No

**`-attest`**  
Performs an attestation check on the firmware response to ensure that the firmware on which the cluster runs has not been compromised\.  
Required: No

## Related Topics<a name="importPrivateKey-seealso"></a>
+ [wrapKey](key_mgmt_util-wrapKey.md)
+ [unWrapKey](key_mgmt_util-unwrapKey.md)
+ [genSymKey](key_mgmt_util-genSymKey.md)
+ [exportPrivateKey](key_mgmt_util-exportPrivateKey.md)