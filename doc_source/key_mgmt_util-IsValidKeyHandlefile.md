# IsValidKeyHandlefile<a name="key_mgmt_util-IsValidKeyHandlefile"></a>

The IsValidKeyHandlefile command in key\_mgmt\_util is used to find out whether a key file in an HSM contains a real private key or a fake PEM key\. A fake PEM file does not contain the actual private key material but instead references the private key in the HSM\. Such a file can be used to establish SSL/TLS offloading from your web server to AWS CloudHSM\. For more information, see [SSL/TLS Offload on Linux](ssl-offload-linux.md)\.

Before you run any key\_mgmt\_util command, you must [start key\_mgmt\_util](key_mgmt_util-getting-started.md#key_mgmt_util-start) and [log in](key_mgmt_util-getting-started.md#key_mgmt_util-log-in) to the HSM as a crypto user \(CU\)\.

## Syntax<a name="IsValidKeyHandlefile-syntax"></a>

```
IsValidKeyHandlefile -h

IsValidKeyHandlefile -f <private-key-file>
```

## Examples<a name="IsValidKeyHandlefile-examples"></a>

These examples show how to use IsValidKeyHandlefile to determine whether a given key file contains the real key material or fake PEM key material\.

**Example : Validate a real private key**  
This command confirms that the file called `privateKey.pem` contains real key material\.  

```
Command: IsValidKeyHandlefile -f privateKey.pem

Input key file has real private key
```

**Example : Invalidate a fake PEM key**  
This command confirms that the file called `caviumKey.pem` contains fake PEM key material made from key handle `15`\.  

```
Command: IsValidKeyHandlefile -f caviumKey.pem
            
Input file has invalid key handle: 15
```

## Parameters<a name="IsValidKeyHandlefile-parameters"></a>

This command takes the following parameters\.

**`-h`**  
Displays command line help for the command\.  
Required: Yes

**`-f`**  
Specifies the name of the file to be checked for valid key material\.  
Required: Yes

## Related topics<a name="IsValidKeyHandlefile-seealso"></a>
+ [getCaviumPrivKey](key_mgmt_util-getCaviumPrivKey.md)
+ [SSL/TLS Offload on Linux](ssl-offload-linux.md)