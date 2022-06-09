# getCert<a name="key_mgmt_util-getCert"></a>

The getCert command in key\_mgmt\_util retrieves an HSM's partition certificates and saves them to a file\. When you run the command, you designate the type of certificate to retrieve\. To do that, you use one of the corresponding integers as described in the [Parameters](#kmu-getCert-parameters) section that follows\. To learn about the role of each of these certificates, see [Verify HSM Identity](verify-hsm-identity.md)\.

Before you run any key\_mgmt\_util command, you must [start key\_mgmt\_util](key_mgmt_util-getting-started.md#key_mgmt_util-start) and [log in](key_mgmt_util-getting-started.md#key_mgmt_util-log-in) to the HSM as a crypto user \(CU\)\. 

## Syntax<a name="kmu-getCert-syntax"></a>

```
getCert -h 

getCert -f <file-name> 
        -t <certificate-type>
```

## Example<a name="kmu-getCert-examples"></a>

This example shows how to use getCert to retrieve a cluster's customer root certificate and save it as a file\.

**Example : Retrieve a customer root certificate**  
This command exports a customer root certificate \(represented by integer `4`\) and saves it to a file called `userRoot.crt`\. When the command succeeds, getCert returns a success message\.  

```
Command: getCert -f userRoot.crt -s 4

Cfm3GetCert() returned 0 :HSM Return: SUCCESS
```

## Parameters<a name="kmu-getCert-parameters"></a>

This command takes the following parameters\.

**`-h`**  
Displays command line help for the command\.  
Required: Yes

**`-f`**  
Specifies the name of the file to which the retrieved certificate will be saved\.  
Required: Yes

**\-s**  
An integer that specifies the type of partition certificate to retrieve\. The integers and their corresponding certificate types are as follows:  
+ **1** – Manufacturer root certificate
+ **2** – Manufacturer hardware certificate
+ **4** – Customer root certificate
+ **8** – Cluster certificate \(signed by customer root certificate\)
+ **16** – Cluster certificate \(chained to the manufacturer root certificate\)
Required: Yes

## Related topics<a name="kmu-getCert-seealso"></a>
+ [Verify HSM Identity](verify-hsm-identity.md)
+ [getCert](cloudhsm_mgmt_util-getCert.md) \(in [cloudhsm\_mgmt\_util](cloudhsm_mgmt_util.md)\)