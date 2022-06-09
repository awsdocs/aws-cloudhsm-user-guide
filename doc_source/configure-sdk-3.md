# Client SDK 3 configure tool<a name="configure-sdk-3"></a>

Use the Client SDK 3 configure tool to bootstrap the client daemon and configure CloudHSM Management Utility\. 

## Syntax<a name="configure-tool-syntax"></a>

```
configure -h | --help
          -a <ENI IP address>
          -m [-i <daemon_id>]
          --ssl --pkey <private key file> --cert <certificate file>
          --cmu <ENI IP address>
```

## Examples<a name="configure-tool-examples"></a>

These examples show how to use the configure tool\.

**Example : Update the HSM data for the AWS CloudHSM client and key\_mgmt\_util**  
This example uses the `-a` parameter of configure to update the HSM data for the AWS CloudHSM client and key\_mgmt\_util\. To use the `-a` parameter, you must have the IP address for one of the HSMs in your cluster\. Use either the console or the AWS CLI to get the IP address\.   

**To get an IP address for a HSM \(console\)**

1. Open the AWS CloudHSM console at [https://console\.aws\.amazon\.com/cloudhsm/](https://console.aws.amazon.com/cloudhsm/)\.

1. To change the AWS Region, use the Region selector in the upper\-right corner of the page\.

1. In the navigation pane, choose **Clusters**\.

1. To open the cluster detail page, in the cluster table, choose the cluster ID\.

1. To get the IP address, on the HSMs tab, choose one of the IP addresses listed under **ENI IP address**\. 

**To get an IP address for a HSM \(AWS CLI\)**
+ Get the IP address of an HSM by using the [describe\-clusters](https://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/describe-clusters.html) command from the AWS CLI\. In the output from the command, the IP address of the HSMs are the values of `EniIp`\. 

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

**To update the HSM data**

1. Before updating the `-a` parameter, stop the AWS CloudHSM client\. This prevents conflicts that might occur while configure edits the client's configuration file\. If the client is already stopped, this command has no effect, so you can use it in a script\.

------
#### [ Amazon Linux ]

   ```
   $ sudo stop cloudhsm-client
   ```

------
#### [ Amazon Linux 2 ]

   ```
   $ sudo service cloudhsm-client stop
   ```

------
#### [ CentOS 7 ]

   ```
   $ sudo service cloudhsm-client stop
   ```

------
#### [ CentOS 8 ]

   ```
   $ sudo service cloudhsm-client stop
   ```

------
#### [ RHEL 7 ]

   ```
   $ sudo service cloudhsm-client stop
   ```

------
#### [ RHEL 8 ]

   ```
   $ sudo service cloudhsm-client stop
   ```

------
#### [ Ubuntu 16\.04 LTS ]

   ```
   $ sudo service cloudhsm-client stop
   ```

------
#### [ Ubuntu 18\.04 LTS ]

   ```
   $ sudo service cloudhsm-client stop
   ```

------
#### [ Windows ]
   + For Windows client 1\.1\.2\+:

     ```
     C:\Program Files\Amazon\CloudHSM>net.exe stop AWSCloudHSMClient
     ```
   + For Windows clients 1\.1\.1 and older:

     Use **Ctrl**\+**C** in the command window where you started the AWS CloudHSM client\.

------

1. This step uses the `-a` parameter of configure to add the `10.0.0.9` ENI IP address to the configurations files\.

------
#### [ Amazon Linux ]

   ```
   $ sudo /opt/cloudhsm/bin/configure -a 10.0.0.9
   ```

------
#### [ Amazon Linux 2 ]

   ```
   $ sudo /opt/cloudhsm/bin/configure -a 10.0.0.9
   ```

------
#### [ CentOS 7 ]

   ```
   $ sudo /opt/cloudhsm/bin/configure -a 10.0.0.9
   ```

------
#### [ CentOS 8 ]

   ```
   $ sudo /opt/cloudhsm/bin/configure -a 10.0.0.9
   ```

------
#### [ RHEL 7 ]

   ```
   $ sudo /opt/cloudhsm/bin/configure -a 10.0.0.9
   ```

------
#### [ RHEL 8 ]

   ```
   $ sudo /opt/cloudhsm/bin/configure -a 10.0.0.9
   ```

------
#### [ Ubuntu 16\.04 LTS ]

   ```
   $ sudo /opt/cloudhsm/bin/configure -a 10.0.0.9
   ```

------
#### [ Ubuntu 18\.04 LTS ]

   ```
   $ sudo /opt/cloudhsm/bin/configure -a 10.0.0.9
   ```

------
#### [ Windows ]

   ```
   c:\Program Files\Amazon\CloudHSM>configure.exe -a 10.0.0.9
   ```

------

1. Next, restart the AWS CloudHSM client\. When the client starts, it uses the ENI IP address in its configuration file to query the cluster\. Then, it writes the ENI IP addresses of all HSMs in the cluster to the `cluster.info` file\. 

------
#### [ Amazon Linux ]

   ```
   $ sudo start cloudhsm-client
   ```

------
#### [ Amazon Linux 2 ]

   ```
   $ sudo service cloudhsm-client start
   ```

------
#### [ CentOS 7 ]

   ```
   $ sudo service cloudhsm-client start
   ```

------
#### [ CentOS 8 ]

   ```
   $ sudo service cloudhsm-client start
   ```

------
#### [ RHEL 7 ]

   ```
   $ sudo service cloudhsm-client start
   ```

------
#### [ RHEL 8 ]

   ```
   $ sudo service cloudhsm-client start
   ```

------
#### [ Ubuntu 16\.04 LTS ]

   ```
   $ sudo service cloudhsm-client start
   ```

------
#### [ Ubuntu 18\.04 LTS ]

   ```
   $ sudo service cloudhsm-client start
   ```

------
#### [ Windows ]
   + For Windows client 1\.1\.2\+:

     ```
     C:\Program Files\Amazon\CloudHSM>net.exe start AWSCloudHSMClient
     ```
   + For Windows clients 1\.1\.1 and older:

     ```
     C:\Program Files\Amazon\CloudHSM>start "cloudhsm_client" cloudhsm_client.exe C:\ProgramData\Amazon\CloudHSM\data\cloudhsm_client.cfg
     ```

------

   When the command completes, the HSM data that the AWS CloudHSM client and key\_mgmt\_util use is complete and accurate\. 

**Example : Update the HSM Data for CMU from client SDK 3\.2\.1 and earlier**  
This example uses the `-m` configure command to copy the updated HSM data from the `cluster.info` file to the `cloudhsm_mgmt_util.cfg` file that cloudhsm\_mgmt\_util uses\. Use this with CMU that ships with Client SDK 3\.2\.1 and earlier\.  
+ Before running the `-m`, stop the AWS CloudHSM client, run the `-a` command, and then restart the AWS CloudHSM client, as shown in the [previous example](#configure-tool-examples)\. This ensures that the data copied into the `cloudhsm_mgmt_util.cfg` file from the `cluster.info` file is complete and accurate\. 

------
#### [ Linux ]

  ```
  $ sudo /opt/cloudhsm/bin/configure -m
  ```

------
#### [ Windows ]

  ```
  c:\Program Files\Amazon\CloudHSM>configure.exe -m
  ```

------

**Example : Update the HSM Data for CMU from client SDK 3\.3\.0 and later**  
This example uses the `--cmu` parameter of the configure command to update HSM data for CMU\. Use this with CMU that ships with Client SDK 3\.3\.0 and later\. For more information about using CMU, see [Using CloudHSM Management Utility \(CMU\) to Manage Users](cli-users.md) and [Using CMU with Client SDK 3\.2\.1 and Earlier](cli-users.md#downlevel-cmu)\.  
+ Use the `--cmu` parameter to pass the IP address of an HSM in your cluster\.

------
#### [ Linux ]

  ```
  $ sudo /opt/cloudhsm/bin/configure --cmu <IP address>
  ```

------
#### [ Windows ]

  ```
  C:\Program Files\Amazon\CloudHSM>configure.exe --cmu <IP address>
  ```

------

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
Updates the HSM ENI IP addresses in the configuration file that CMU uses\.   
The `-m` parameter is for use with CMU from Client SDK 3\.2\.1 and earlier\. For CMU from Client SDK 3\.3\.0 and later, see `--cmu` parameter, which simplifies the process of updating HSM data for CMU\.
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

**\-\-cmu *<ENI IP address>***  
Combines the `-a` and `-m` parameters into one parameter\. Adds the specified HSM elastic network interface \(ENI\) IP address to AWS CloudHSM configuration files and then updates the CMU configuration file\. Enter an IP address from any HSM in the cluster\. For Client SDK 3\.2\.1 and earlier, see [Using CMU with Client SDK 3\.2\.1 and Earlier](cli-users.md#downlevel-cmu)\.  
Required: Yes

## Related topics<a name="configure-tool-seealso"></a>
+ [Set up key\_mgmt\_util](key_mgmt_util-getting-started.md#key_mgmt_util-setup)