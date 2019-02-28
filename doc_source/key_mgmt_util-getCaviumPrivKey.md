# getCaviumPrivKey<a name="key_mgmt_util-getCaviumPrivKey"></a>

The **getCaviumPrivKey** command in key\_mgmt\_util exports a private key from an HSM in fake PEM format\. The fake PEM file, which does not contain the actual private key material but instead references the private key in the HSM, can then be used to establish SSL/TLS offloading from your web server to AWS CloudHSM\. For more information, see [SSL/TLS Offload on Linux](ssl-offload-linux.md)\.

Before you run any key\_mgmt\_util command, you must [start key\_mgmt\_util](key_mgmt_util-getting-started.md#key_mgmt_util-start) and [login](key_mgmt_util-getting-started.md#key_mgmt_util-log-in) to the HSM as a crypto user \(CU\)\.

## Syntax<a name="getCaviumPrivKey-syntax"></a>

```
getCaviumPrivKey -h

exportPrivateKey -k <private-key-handle
                 -out <fake-PEM-file>
```

## Examples<a name="getCaviumPrivKey-examples"></a>

This example shows how to use getCaviumPrivKey to export a private key in fake PEM format\.

**Example : Export a Fake PEM File**  
This command creates and exports a fake PEM version of a private key with handle `15` and saves it to a file called `cavKey.pem`\. When the command succeeds, exportPrivateKey returns a success message\.  

```
Command: getCaviumPrivKey -k 15 -out cavKey.pem

Private Key Handle is written to cavKey.pem in fake PEM format

        getCaviumPrivKey returned: 0x00 : HSM Return: SUCCESS
```

## Parameters<a name="getCaviumPrivKey-parameters"></a>

This command takes the following parameters\.

**`-h`**  
Displays command line help for the command\.  
Required: Yes

**`-k`**  
Specifies the key handle of the private key to be exported in fake PEM format\.  
Required: Yes

**`-out`**  
Specifies the name of the file to which the fake PEM key will be written\.  
Required: Yes

## Related Topics<a name="getCaviumPrivKey-seealso"></a>
+ [importPrivateKey](key_mgmt_util-importPrivateKey.md)
+ [SSL/TLS Offload on Linux](ssl-offload-linux.md)