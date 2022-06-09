# importPubKey<a name="key_mgmt_util-importPubKey"></a>

The importPubKey command in key\_mgmt\_util imports a PEM format public key into an HSM\. You can use it to import public keys that were generated outside of the HSM\. You can also use the command to import keys that were exported from an HSM, such as those exported by the [exportPubKey](key_mgmt_util-exportPubKey.md) command\.

Before you run any key\_mgmt\_util command, you must [start key\_mgmt\_util](key_mgmt_util-getting-started.md#key_mgmt_util-start) and [log in](key_mgmt_util-getting-started.md#key_mgmt_util-log-in) to the HSM as a crypto user \(CU\)\.

## Syntax<a name="importPubKey-syntax"></a>

```
importPubKey -h

importPubKey -l <label>
             -f <key-file>
             [-sess]
             [-id <key-id>]
             [min_srv <minimum-number-of-servers>]
             [-timeout <number-of-seconds>]
```

## Examples<a name="importPubKey-examples"></a>

This example shows how to use importPubKey to import a public key into an HSM\.

**Example : Import a public key**  
This command imports a public key from a file named `public.pem` with the label `importedPublicKey`\. When the command succeeds, importPubKey returns a key handle for the imported key and a success message\.  

```
Command: importPubKey -l importedPublicKey -f public.pem

Cfm3CreatePublicKey returned: 0x00 : HSM Return: SUCCESS

Public Key Handle: 262230

        Cluster Error Status
        Node id 2 and err state 0x00000000 : HSM Return: SUCCESS
        Node id 0 and err state 0x00000000 : HSM Return: SUCCESS
        Node id 1 and err state 0x00000000 : HSM Return: SUCCESS
```

## Parameters<a name="importPubKey-parameters"></a>

This command takes the following parameters\.

**`-h`**  
Displays command line help for the command\.  
Required: Yes

**`-l`**  
Specifies the user\-defined public key label\.  
Required: Yes

**`-f`**  
Specifies the file name of the key to import\.  
Required: Yes

**`-sess`**  
Designates the imported key as a session key\.  
Default: The imported key is held as a persistent \(token\) key in the cluster\.  
Required: No

**`-id`**  
Specifies the ID of the key to be imported\.  
Default: No ID value\.  
Required: No

**`-min_srv`**  
Specifies the minimum number of HSMs to which the imported key is synchronized before the value of the `-timeout` parameter expires\. If the key is not synchronized to the specified number of servers in the time allotted, it is not created\.  
AWS CloudHSM automatically synchronizes every key to every HSM in the cluster\. To speed up your process, set the value of `min_srv` to less than the number of HSMs in the cluster and set a low timeout value\. Note, however, that some requests might not generate a key\.  
Default: 1  
Required: No

**`-timeout`**  
Specifies the number of seconds to wait for the key to sync across HSMs when the `min-serv` parameter is included\. If no number is specified, the polling continues forever\.  
Default: No limit  
Required: No

## Related topics<a name="importPubKey-seealso"></a>
+ [exportPubKey](key_mgmt_util-exportPubKey.md)
+ [Generate Keys](using-kmu.md#generate-keys)