# getCert<a name="cloudhsm_mgmt_util-getCert"></a>

With the getCert command in cloudhsm\_mgmt\_util, you can retrieve the certificates of a particular HSM in a cluster\. When you run the command, you designate the type of certificate to retrieve\. To do that, you use one of the corresponding integers as described in the [Arguments](#getCert-arguments) section below\. To learn about the role of each of these certificates, see [Verify HSM Identity](verify-hsm-identity.md)\.

Before you run any CMU command, you must start CMU and log in to the HSM\. Be sure that you log in with the user account type that can run the commands you plan to use\.

If you add or delete HSMs, update the configuration files for CMU\. Otherwise, the changes that you make might not be effective for all HSMs in the cluster\.

## User type<a name="getCert-userType"></a>

The following users can run this command\.
+ All users\.

## Prerequisites<a name="getCert-prerequisites"></a>

Before you begin, you must enter server mode on the target HSM\. For more information, see [server](cloudhsm_mgmt_util-server.md)\.

## Syntax<a name="getCert-syntax"></a>

To use the getCert command once in server mode:

```
server> getCert <file-name> <certificate-type>
```

## Example<a name="getCert-examples"></a>

First, enter server mode\. This command enters server mode on an HSM with server number `0`\.

```
aws-cloudhsm> server 0

Server is in 'E2' mode...
```

Then, use the getCert command\. In this example, we use `/tmp/PO.crt` as the name of the file to which the certificate will be saved and `4` \(Customer Root Certificate\) as the desired certificate type: 

```
server0> getCert /tmp/PO.crt 4
getCert Success
```

## Arguments<a name="getCert-arguments"></a>

```
getCert <file-name> <certificate-type>
```

**<file\-name>**  
Specifies the name of the file to which the certificate is saved\.  
Required: Yes

**<certificate\-type>**  
An integer that specifies the type of certificate to retrieve\. The integers and their corresponding certificate types are as follows:  
+ **1** – Manufacturer Root Certificate
+ **2** – Manufacturer Hardware Certificate
+ **4** – Customer Root Certificate
+ **8** – Cluster Certificate \(signed by Customer Root Certificate\)
+ **16** – Cluster Certificate \(chained to the Manufacturer Root Certificate\)
Required: Yes

## Related topics<a name="chmu-getCert-seealso"></a>
+ [server](cloudhsm_mgmt_util-server.md)