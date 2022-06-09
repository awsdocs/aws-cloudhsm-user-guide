# exportPubKey<a name="key_mgmt_util-exportPubKey"></a>

The exportPubKey command in key\_mgmt\_util exports a public key in an HSM to a file\. You can use it to export public keys that you generate in an HSM\. You can also use this command to export public keys that were imported into an HSM, such as those imported with the [importPubKey](key_mgmt_util-importPubKey.md) command\.

The exportPubKey operation copies the key material to a file that you specify\. But it does not remove the key from the HSM, change its [key attributes](key-attribute-table.md), or prevent you from using the key in further cryptographic operations\. You can export the same key multiple times\.

You can only export public keys that have a `OBJ_ATTR_EXTRACTABLE` value of `1`\. To find a key's attributes, use the [getAttribute](key_mgmt_util-getAttribute.md) command\.

Before you run any `key_mgmt_util` command, you must [start key\_mgmt\_util](key_mgmt_util-getting-started.md#key_mgmt_util-start) and [log in](key_mgmt_util-getting-started.md#key_mgmt_util-log-in) to the HSM as a crypto user \(CU\)\.

## Syntax<a name="exportPubKey-syntax"></a>

```
exportPubKey -h

exportPubKey -k <public-key-handle
             -out <key-file>
```

## Examples<a name="exportPubKey-examples"></a>

This example shows how to use exportPubKey to export a public key from an HSM\.

**Example : Export a public key**  
This command exports a public key with handle `10` to a file called `public.pem`\. When the command succeeds, exportPubKey returns a success message\.  

```
Command: exportPubKey -k 10 -out public.pem

PEM formatted public key is written to public.pem

Cfm3ExportPubKey returned: 0x00 : HSM Return: SUCCESS
```

## Parameters<a name="exportPubKey-parameters"></a>

This command takes the following parameters\.

**`-h`**  
Displays command line help for the command\.  
Required: Yes

**`-k`**  
Specifies the key handle of the public key to be exported\.  
Required: Yes

**`-out`**  
Specifies the name of the file to which the exported public key will be written\.  
Required: Yes

## Related topics<a name="exportPubKey-seealso"></a>
+ [importPubKey](key_mgmt_util-importPubKey.md)
+ [Generate Keys](using-kmu.md#generate-keys)