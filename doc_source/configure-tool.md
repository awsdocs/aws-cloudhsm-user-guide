# Configure Tool<a name="configure-tool"></a>

AWS CloudHSM automatically synchronizes data among all HSMs in a cluster\. The configure tool updates the HSM data in the configuration files that the synchronization mechanisms use\. Use configure to refresh the HSM data before you use the command line tools, especially when the HSMs in the cluster have changed\.

Before using the key\_mgmt\_util tool, update the `-a` parameter of configure\. Before using the cloudhsm\_mgmt\_util tool, update the `-a` and `-m` parameters of configure\. You can also run the configure tool's `--ssl` command to update SSL keys and certificates\.

## Syntax<a name="configure-tool-syntax"></a>

```
configure -h | --help
          -a <ENI IP address>
          -m [-i <daemon_id>]
          --ssl --pkey <private key file> --cert <certificate file>
```

## Examples<a name="configure-tool-examples"></a>

These examples show how to use the configure tool\.

**Example : Update the HSM Data for the AWS CloudHSM Client and key\_mgmt\_util**  
This example uses the `-a` parameter of configure to update the HSM data for the AWS CloudHSM client and key\_mgmt\_util\. This command is also the first step in updating the cloudhsm\_mgmt\_util configuration file\.  
Before updating the `-a` parameter, stop the AWS CloudHSM client\. This prevents conflicts that might occur while configure edits the client's configuration file\. If the client is already stopped, this command has no effect, so you can use it in a script\.  

```
$ sudo stop cloudhsm-client
```

```
$ sudo service cloudhsm-client stop
```

```
$ sudo stop cloudhsm-client
```

```
$ sudo service cloudhsm-client stop
```

```
$ sudo stop cloudhsm-client
```

```
$ sudo service cloudhsm-client stop
```

```
$ sudo service cloudhsm-client stop
```
+ For Windows client 1\.1\.2\+:

  ```
  C:\Program Files\Amazon\CloudHSM>net.exe stop AWSCloudHSMClient
  ```
+ For Windows clients 1\.1\.1 and older:

  Use **Ctrl**\+**C** in the command window where you started the AWS CloudHSM client\.
Next, get the ENI IP address of any one of the HSMs in your cluster\. This command uses the [describe\-clusters](https://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/describe-clusters.html) command in the AWS CLI, but you can also use the [DescribeClusters](https://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_DescribeClusters.html) operation or the [Get\-HSM2Cluster](https://docs.aws.amazon.com/powershell/latest/reference/items/Get-HSM2Cluster.html) PowerShell cmdlet\.   
This excerpt of the output shows the ENI IP addresses of the HSMs in a sample cluster\. We can use either of the IP addresses in the next command\.   

```
$  aws cloudhsmv2 describe-clusters
	

{
    "Clusters": [
        { ... }
            "Hsms": [
                {
...
                    "EniIp": "10.0.0.9",
...
                },
                {
...
                    "EniIp": "10.0.1.6",
...
```
This step uses the `-a` parameter of configure to add the `10.0.0.9` ENI IP address to the configurations files\.  

```
$ sudo /opt/cloudhsm/bin/configure -a 10.0.0.9
```

```
$ sudo /opt/cloudhsm/bin/configure -a 10.0.0.9
```

```
$ sudo /opt/cloudhsm/bin/configure -a 10.0.0.9
```

```
$ sudo /opt/cloudhsm/bin/configure -a 10.0.0.9
```

```
$ sudo /opt/cloudhsm/bin/configure -a 10.0.0.9
```

```
$ sudo /opt/cloudhsm/bin/configure -a 10.0.0.9
```

```
$ sudo /opt/cloudhsm/bin/configure -a 10.0.0.9
```

```
c:\Program Files\Amazon\CloudHSM>configure.exe -a 10.0.0.9
```
Next, restart the AWS CloudHSM client\. When the client starts, it uses the ENI IP address in its configuration file to query the cluster\. Then, it writes the ENI IP addresses of all HSMs in the cluster to the `cluster.info` file\.   

```
$ sudo start cloudhsm-client
```

```
$ sudo service cloudhsm-client start
```

```
$ sudo start cloudhsm-client
```

```
$ sudo service cloudhsm-client start
```

```
$ sudo start cloudhsm-client
```

```
$ sudo service cloudhsm-client start
```

```
$ sudo service cloudhsm-client start
```
+ For Windows client 1\.1\.2\+:

  ```
  C:\Program Files\Amazon\CloudHSM>net.exe start AWSCloudHSMClient
  ```
+ For Windows clients 1\.1\.1 and older:

  ```
  C:\Program Files\Amazon\CloudHSM>start "cloudhsm_client" cloudhsm_client.exe C:\ProgramData\Amazon\CloudHSM\data\cloudhsm_client.cfg
  ```
When the command completes, the HSM data that the AWS CloudHSM client and key\_mgmt\_util use is complete and accurate\. Before using cloudhsm\_mgmt\_util, update the `-m` parameter of configure, as shown in the following example\. 

**Example : Update the HSM Data for cloudhsm\_mgmt\_util**  
This example uses the `-m` configure command to copy the updated HSM data from the `cluster.info` file to the `cloudhsm_mgmt_util.cfg` file that cloudhsm\_mgmt\_util uses\.   
Before running the `-m`, stop the AWS CloudHSM client, run the `-a` command, and then restart the AWS CloudHSM client, as shown in the [previous example](#configure-tool-examples)\. This ensures that the data copied into the `cloudhsm_mgmt_util.cfg` file from the `cluster.info` file is complete and accurate\.   

```
$ sudo /opt/cloudhsm/bin/configure -m
```

```
$ sudo /opt/cloudhsm/bin/configure -m
```

```
$ sudo /opt/cloudhsm/bin/configure -m
```

```
$ sudo /opt/cloudhsm/bin/configure -m
```

```
$ sudo /opt/cloudhsm/bin/configure -m
```

```
$ sudo /opt/cloudhsm/bin/configure -m
```

```
$ sudo /opt/cloudhsm/bin/configure -m
```

```
c:\Program Files\Amazon\CloudHSM>configure.exe -m
```

## Parameters<a name="configure-tool-params"></a>

**\-h \| \-\-help**  
Displays command syntax\.  
Required: Yes

**\-a *<ENI IP address>***  
Adds the specified HSM elastic network interface \(ENI\) IP address to AWS CloudHSM configuration files\. Enter the ENI IP address of any one of the HSMs in the cluster\. It does not matter which one you select\.   
To get the ENI IP addresses of the HSMs in your cluster, use the [DescribeClusters](https://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_DescribeClusters.html) operation, the [describe\-clusters](https://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/describe-clusters.html) AWS CLI command, or the [Get\-HSM2Cluster](https://docs.aws.amazon.com/powershell/latest/reference/items/Get-HSM2Cluster.html) PowerShell cmdlet\.   
Before running the ` -a` configure command, stop the AWS CloudHSM client\. Then, when the `-a` command completes, restart the AWS CloudHSM client\. For details, [see the examples](#configure-tool-examples)\. 
This parameter edits the following configuration files:  
+ `/opt/cloudhsm/etc/cloudhsm_client.cfg`: Used by AWS CloudHSM client and [key\_mgmt\_util](key_mgmt_util.md)\. 
+ `/opt/cloudhsm/etc/cloudhsm_mgmt_util.cfg`: Used by [cloudhsm\_mgmt\_util](cloudhsm_mgmt_util.md)\.
When the AWS CloudHSM client starts, it uses the ENI IP address in its configuration file to query the cluster and update the `cluster.info` file \(`/opt/cloudhsm/daemon/1/cluster.info`\) with the correct ENI IP addresses for all HSMs in the cluster\.   
Required: Yes

**\-m**  
Updates the HSM ENI IP addresses in the configuration file that cloudhsm\_mgmt\_util uses\.   
When you update the `-a` parameter of configure and then start the AWS CloudHSM client, the client daemon queries the cluster and updates the `cluster.info` files with the correct HSM IP addresses for all HSMs in the cluster\. Running the `-m` configure command completes the update by copying the HSM IP addresses from the `cluster.info` to the `cloudhsm_mgmt_util.cfg` configuration file that cloudhsm\_mgmt\_util uses\.   
Be sure to run `-a` configure command and restart the AWS CloudHSM client before running the `-m` command\. This ensures that the data copied into `cloudhsm_mgmt_util.cfg` from `cluster.info` is complete and accurate\.   
Required: Yes

**\-i**  
Specifies an alternate client daemon\. The default value represents the AWS CloudHSM client\.  
Default: `1`  
Required: No

**\-\-ssl**  
Replaces the SSL key and certificate for the cluster with the specified private key and certificate\. When you use this parameter, the `--pkey` and `--cert` parameters are required\.   
Required: No

**\-\-pkey**  
Specifies the new private key\. Enter the path and file name of the file that contains the private key\.  
Required: Yes if \-\-ssl is specified\. Otherwise, this should not be used\.

**\-\-cert**  
Specifies the new certificate\. Enter the path and file name of the file that contains the certificate\. The certificate should chain up to the `customerCA.crt` certificate, the self\-signed certificate used to initialize the cluster\. For more information, see [Initialize the Cluster](https://docs.aws.amazon.com/cloudhsm/latest/userguide/initialize-cluster.html#sign-csr)\.   
Required: Yes if \-\-ssl is specified\. Otherwise, this should not be used\.

## Related Topics<a name="configure-tool-seealso"></a>
+ [Set Up key\_mgmt\_util](key_mgmt_util-getting-started.md#key_mgmt_util-setup)
+ [Prepare to run cloudhsm\_mgmt\_util](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-setup)