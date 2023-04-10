# Client SDK 5 configure tool<a name="configure-sdk-5"></a>

Use the Client SDK 5 configure tool to update client\-side configuration files\. 

Each component in Client SDK 5 includes a configure tool with a designator of the component in the file name of the configure tool\. For example, the PKCS \#11 library for Client SDK 5 includes a configure tool named `configure-pkcs11` on Linux or `configure-pkcs11.exe` on Windows\.

## Syntax<a name="configure-tool-syntax5"></a>

```
configure-[ pkcs11 | dyn | jce | cli ][ .exe ] 
                   -a <ENI IP address>
                   [--hsm-ca-cert <CustomerCA/certificate/file>]
                   [--cluster-id <Cluster ID>]
                   [--endpoint <Endpoint>]
                   [--region <Region>] 
                   [--server-client-cert-file <Client/certificate/file>]
                   [--server-client-key-file <Private/key/file>]
                   [--log-level <Error | warn | info | debug | trace>] 
                   [--log-rotation <Weekly | daily>]
                   [--log-file <Log/file/path>]
                   [-h | --help]
                   [-V | --version]
                   [--disable-key-availability-check]
                   [--enable-key-availability-check]
                   [--disable-validate-key-at-init]
                   [--enable-validate-key-at-init]
```

## Advanced configurations<a name="configure-tool-subcommands5"></a>

For a list of advanced configurations specific to the Client SDK 5 configure tool, refer to [Advanced configurations for the Client SDK 5 configure tool](configure-sdk5-advanced-configs.md)\. 

## Examples<a name="configure-tool-examples5"></a>

These examples show how to use the configure tool for Client SDK 5\.

### Bootstrap Client SDK 5<a name="ex1"></a>

**Example**  
This example uses the `-a` parameter to update the HSM data for Client SDK 5\. To use the `-a` parameter, you must have the IP address for one of the HSMs in your cluster\.   

**To bootstrap a Linux EC2 instance for Client SDK 5**
+  Use the configure tool to specify the IP address of a HSM in your cluster\. 

  ```
  $ sudo /opt/cloudhsm/bin/configure-pkcs11 -a <HSM IP address>
  ```

**To bootstrap a Windows EC2 instance for Client SDK 5**
+  Use the configure tool to specify the IP address of a HSM in your cluster\. 

  ```
  C:\Program Files\Amazon\CloudHSM\bin\ .\configure-pkcs11.exe -a <HSM IP address>
  ```

**To bootstrap a Linux EC2 instance for Client SDK 5**
+  Use the configure tool to specify the IP address of a HSM in your cluster\. 

  ```
  $ sudo /opt/cloudhsm/bin/configure-dyn -a <HSM IP address>
  ```

**To bootstrap a Linux EC2 instance for Client SDK 5**
+  Use the configure tool to specify the IP address of a HSM in your cluster\. 

  ```
  $ sudo /opt/cloudhsm/bin/configure-jce -a <HSM IP address>
  ```

**To bootstrap a Windows EC2 instance for Client SDK 5**
+  Use the configure tool to specify the IP address of a HSM in your cluster\. 

  ```
  C:\Program Files\Amazon\CloudHSM\bin\ .\configure-jce.exe -a <HSM IP address>
  ```

**To bootstrap a Linux EC2 instance for Client SDK 5**
+  Use the configure tool to specify the IP address of a HSM in your cluster\. 

  ```
  $ sudo /opt/cloudhsm/bin/configure-cli -a <HSM IP address>
  ```

**To bootstrap a Windows EC2 instance for Client SDK 5**
+  Use the configure tool to specify the IP address of a HSM in your cluster\. 

  ```
  C:\Program Files\Amazon\CloudHSM\bin\ .\configure-cli.exe -a <HSM IP address>
  ```
For more information about the `-a` parameter, see [Parameters](#configure-tool-params5)\.

### Specify cluster, region, and endpoint for Client SDK 5<a name="ex2"></a>

**Example**  
 This example uses the `cluster-id` parameter to bootstrap Client SDK 5 by making a `DescribeClusters` call\.   

**To bootstrap a Linux EC2 instance for Client SDK 5 with `cluster-id`**
+  Use the cluster ID `cluster-1234567` to specify the IP address of a HSM in your cluster\. 

  ```
  $ sudo /opt/cloudhsm/bin/configure-pkcs11 --cluster-id cluster-1234567
  ```

**To bootstrap a Windows EC2 instance for Client SDK 5 with `cluster-id`**
+  Use the cluster ID `cluster-1234567` to specify the IP address of a HSM in your cluster\. 

  ```
  C:\Program Files\Amazon\CloudHSM\configure-pkcs11.exe --cluster-id cluster-1234567
  ```

**To bootstrap a Linux EC2 instance for Client SDK 5 with `cluster-id`**
+  Use the cluster ID `cluster-1234567` to specify the IP address of a HSM in your cluster\. 

  ```
  $ sudo /opt/cloudhsm/bin/configure-dyn --cluster-id cluster-1234567
  ```

**To bootstrap a Linux EC2 instance for Client SDK 5 with `cluster-id`**
+  Use the cluster ID `cluster-1234567` to specify the IP address of a HSM in your cluster\. 

  ```
  $ sudo /opt/cloudhsm/bin/configure-jce --cluster-id cluster-1234567
  ```

**To bootstrap a Windows EC2 instance for Client SDK 5 with `cluster-id`**
+  Use the cluster ID `cluster-1234567` to specify the IP address of a HSM in your cluster\. 

  ```
  C:\Program Files\Amazon\CloudHSM\configure-jce.exe --cluster-id cluster-1234567
  ```
 You can use the `--region` and `--endpoint` parameters in combination with the `cluster-id` parameter to specify how the system makes the `DescribeClusters` call\. For instance, if the region of the cluster is different than the one configured as your AWS CLI default, you should use the `--region` parameter to use that region\. Additionally, you have the ability to specify the AWS CloudHSM API endpoint to use for the call, which might be necessary for various network setups, such as using VPC interface endpoints that don’t use the default DNS hostname for AWS CloudHSM\.   

**To bootstrap a Linux EC2 instance with a custom endpoint and region**
+  Use the configure tool to specify the IP address of a HSM in your cluster with a custom region and endpoint\. 

  ```
  $ sudo /opt/cloudhsm/bin/configure-pkcs11 --cluster-id cluster-1234567 --region us-east-1 --endpoint https://cloudhsmv2.us-east-1.amazonaws.com
  ```

**To bootstrap a Windows EC2 instance with a endpoint and region**
+  Use the configure tool to specify the IP address of a HSM in your cluster with a custom region and endpoint\.

  ```
  C:\Program Files\Amazon\CloudHSM\configure-pkcs11.exe --cluster-id cluster-1234567--region us-east-1 --endpoint https://cloudhsmv2.us-east-1.amazonaws.com
  ```

**To bootstrap a Linux EC2 instance with a custom endpoint and region**
+  Use the configure tool to specify the IP address of a HSM in your cluster with a custom region and endpoint\. 

  ```
  $ sudo /opt/cloudhsm/bin/configure-dyn --cluster-id cluster-1234567 --region us-east-1 --endpoint https://cloudhsmv2.us-east-1.amazonaws.com
  ```

**To bootstrap a Linux EC2 instance with a custom endpoint and region**
+  Use the configure tool to specify the IP address of a HSM in your cluster with a custom region and endpoint\. 

  ```
  $ sudo /opt/cloudhsm/bin/configure-jce --cluster-id cluster-1234567 --region us-east-1 --endpoint https://cloudhsmv2.us-east-1.amazonaws.com
  ```

**To bootstrap a Windows EC2 instance with a endpoint and region**
+  Use the configure tool to specify the IP address of a HSM in your cluster with a custom region and endpoint\.

  ```
  C:\Program Files\Amazon\CloudHSM\configure-jce.exe --cluster-id cluster-1234567 --region us-east-1 --endpoint https://cloudhsmv2.us-east-1.amazonaws.com
  ```
For more information about the `--cluster-id`, `--region`, and `--endpoint` parameters, see [Parameters](#configure-tool-params5)\.

### Update client certificate and key for TLS client\-server mutual authentication<a name="ex3"></a>

**Example**  
 This examples shows how to use the `server-client-cert-file` and `--server-client-key-file` parameters to reconfigure SSL by specifying a custom key and SSL certificate for AWS CloudHSM   

**To use a custom certificate and key for TLS client\-server mutual authentication with Client SDK 5 on Linux**

1. Copy your key and certificate to the appropriate directory\.

   ```
   $ sudo cp ssl-client.crt /opt/cloudhsm/etc
   sudo cp ssl-client.key /opt/cloudhsm/etc
   ```

1.  Use the configure tool to specify `ssl-client.crt` and `ssl-client.key`\.

   ```
   $ sudo /opt/cloudhsm/bin/configure-pkcs11 \
               --server-client-cert-file /opt/cloudhsm/etc/ssl-client.crt \
               --server-client-key-file /opt/cloudhsm/etc/ssl-client.key
   ```

**To use a custom certificate and key for TLS client\-server mutual authentication with Client SDK 5 on Windows**

1. Copy your key and certificate to the appropriate directory\.

   ```
   cp ssl-client.crt C:\ProgramData\Amazon\CloudHSM\ssl-client.crt
   cp ssl-client.key C:\ProgramData\Amazon\CloudHSM\ssl-client.key
   ```

1.  With a PowerShell interpreter, use the configure tool to specify `ssl-client.crt` and `ssl-client.key`\.

   ```
   & "C:\Program Files\Amazon\CloudHSM\bin\configure-pkcs11.exe" `
               --server-client-cert-file C:\ProgramData\Amazon\CloudHSM\ssl-client.crt `
               --server-client-key-file C:\ProgramData\Amazon\CloudHSM\ssl-client.key
   ```

**To use a custom certificate and key for TLS client\-server mutual authentication with Client SDK 5 on Linux**

1. Copy your key and certificate to the appropriate directory\.

   ```
   $ sudo cp ssl-client.crt /opt/cloudhsm/etc
   sudo cp ssl-client.key /opt/cloudhsm/etc
   ```

1.  Use the configure tool to specify `ssl-client.crt` and `ssl-client.key`\.

   ```
   $ sudo /opt/cloudhsm/bin/configure-dyn \
               --server-client-cert-file /opt/cloudhsm/etc/ssl-client.crt \
               --server-client-key-file /opt/cloudhsm/etc/ssl-client.key
   ```

**To use a custom certificate and key for TLS client\-server mutual authentication with Client SDK 5 on Linux**

1. Copy your key and certificate to the appropriate directory\.

   ```
   $ sudo cp ssl-client.crt /opt/cloudhsm/etc
   sudo cp ssl-client.key /opt/cloudhsm/etc
   ```

1.  Use the configure tool to specify `ssl-client.crt` and `ssl-client.key`\.

   ```
   $ sudo /opt/cloudhsm/bin/configure-jce \
               --server-client-cert-file /opt/cloudhsm/etc/ssl-client.crt \
               --server-client-key-file /opt/cloudhsm/etc/ssl-client.key
   ```

**To use a custom certificate and key for TLS client\-server mutual authentication with Client SDK 5 on Windows**

1. Copy your key and certificate to the appropriate directory\.

   ```
   cp ssl-client.crt C:\ProgramData\Amazon\CloudHSM\ssl-client.crt
   cp ssl-client.key C:\ProgramData\Amazon\CloudHSM\ssl-client.key
   ```

1.  With a PowerShell interpreter, use the configure tool to specify `ssl-client.crt` and `ssl-client.key`\.

   ```
   & "C:\Program Files\Amazon\CloudHSM\bin\configure-jce.exe" `
               --server-client-cert-file C:\ProgramData\Amazon\CloudHSM\ssl-client.crt `
               --server-client-key-file C:\ProgramData\Amazon\CloudHSM\ssl-client.key
   ```
For more information about the `server-client-cert-file` and `--server-client-key-file` parameters, see [Parameters](#configure-tool-params5)\.

### Disable client key durability settings<a name="ex4"></a>

**Example**  
This example uses the `--disable-key-availability-check` parameter to disable client key durability settings\. To run a cluster with a single HSM, you must disable client key durability settings\.   

**To disable client key durability for Client SDK 5 on Linux**
+  Use the configure tool to disable client key durability settings\. 

  ```
  $ sudo /opt/cloudhsm/bin/configure-pkcs11 --disable-key-availability-check
  ```

**To disable client key durability for Client SDK 5 on Windows**
+  Use the configure tool to disable client key durability settings\. 

  ```
  C:\Program Files\Amazon\CloudHSM\configure-pkcs11.exe --disable-key-availability-check
  ```

**To disable client key durability for Client SDK 5 on Linux**
+  Use the configure tool to disable client key durability settings\. 

  ```
  $ sudo /opt/cloudhsm/bin/configure-dyn --disable-key-availability-check
  ```

**To disable client key durability for Client SDK 5 on Linux**
+  Use the configure tool to disable client key durability settings\. 

  ```
  $ sudo /opt/cloudhsm/bin/configure-jce --disable-key-availability-check
  ```

**To disable client key durability for Client SDK 5 on Windows**
+  Use the configure tool to disable client key durability settings\. 

  ```
  C:\Program Files\Amazon\CloudHSM\configure-jce.exe --disable-key-availability-check
  ```
For more information about the `--disable-key-availability-check` parameter, see [Parameters](#configure-tool-params5)\.

### Manage logging options<a name="ex5"></a>

**Example**  
This example uses the `log-file`, `log-level`, and `log-rotation` parameters to manage Client SDK 5 logging\.   

**To configure the name of the logging file**
+ Use the `log-file` option to change the name or location of the log file\. 

  ```
  $ sudo /opt/cloudhsm/bin/configure-pkcs11 --log-file /path/to/log
  ```

  For example, use the following command to set the log file name to *cloudhsm\-pkcs11\.log*\.

  ```
  $ sudo /opt/cloudhsm/bin/configure-pkcs11 --log-file /opt/cloudhsm/run/cloudhsm-pkcs11.log
  ```

  If you do not specify a location for the file, the system writes logs to the default location\.
  + Linux: 

    ```
    /opt/cloudhsm/run
    ```
  + Windows: 

    ```
    C:\Program Files\Amazon\CloudHSM
    ```

**To configure the logging level**
+ Use the `log-level` option to establish how much information Client SDK 5 should log\. 

  ```
  $ sudo /opt/cloudhsm/bin/configure-pkcs11 --log-level <error | warn | info | debug | trace>
  ```

  For example, use the following command to set the log level to receive error and warning messages in logs\.

  ```
  $ sudo /opt/cloudhsm/bin/configure-pkcs11 --log-level warn
  ```

**To configure log rotation**
+ Use the `log-rotation` option to establish how often Client SDK 5 should rotate the logs\. 

  ```
  $ sudo /opt/cloudhsm/bin/configure-pkcs11 --log-rotation <daily | hourly | never>
  ```

  For example, use the following command to rotate the logs daily\. Each day the system creates a new log and appends a time stamp to the file name\.

  ```
  $ sudo /opt/cloudhsm/bin/configure-pkcs11 --log-rotation daily
  ```

**To configure the name of the logging file**
+ Use the `log-file` option to change the name or location of the log file\. 

  ```
  $ sudo /opt/cloudhsm/bin/configure-dyn --log-file /path/to/log
  ```

  For example, use the following command to set the log file name to *cloudhsm\-dyn\.log*\.

  ```
  $ sudo /opt/cloudhsm/bin/configure-dyn --log-file /opt/cloudhsm/run/cloudhsm-dyn.log
  ```

  If you do not specify a location for the file, the system writes logs to `stderr`

**To configure the logging level**
+ Use the `log-level` option to establish how much information Client SDK 5 should log\. 

  ```
  $ sudo /opt/cloudhsm/bin/configure-dyn --log-level <error | warn | info | debug | trace>
  ```

  For example, use the following command to set the log level to receive error and warning messages in logs\.

  ```
  $ sudo /opt/cloudhsm/bin/configure-dyn --log-level warn
  ```

**To configure log rotation**
+ Use the `log-rotation` option to establish how often Client SDK 5 should rotate the logs\. 

  ```
  $ sudo /opt/cloudhsm/bin/configure-dyn --log-rotation <daily | hourly | never>
  ```

  For example, use the following command to rotate the logs daily\. Each day the system creates a new log and appends a time stamp to the file name\.

  ```
  $ sudo /opt/cloudhsm/bin/configure-dyn --log-rotation daily
  ```

**To configure the name of the logging file**
+ Use the `log-file` option to change the name or location of the log file\. 

  ```
  $ sudo /opt/cloudhsm/bin/configure-jce --log-file /path/to/log
  ```

  For example, use the following command to set the log file name to *cloudhsm\-dyn\.log*\.

  ```
  $ sudo /opt/cloudhsm/bin/configure-jce --log-file /opt/cloudhsm/run/cloudhsm-jce.log
  ```

  If you do not specify a location for the file, the system writes logs to the default location:
  + Linux: 

    ```
    /opt/cloudhsm/run
    ```
  + Windows: 

    ```
    C:\ProgramData\Amazon\CloudHSM
    ```

**To configure the logging level**
+ Use the `log-level` option to establish how much information Client SDK 5 should log\. 

  ```
  $ sudo /opt/cloudhsm/bin/configure-jce --log-level <error | warn | info | debug | trace>
  ```

  For example, use the following command to set the log level to receive error and warning messages in logs\.

  ```
  $ sudo /opt/cloudhsm/bin/configure-jce --log-level warn
  ```

**To configure log rotation**
+ Use the `log-rotation` option to establish how often Client SDK 5 should rotate the logs\. 

  ```
  $ sudo /opt/cloudhsm/bin/configure-jce --log-rotation <daily | hourly | never>
  ```

  For example, use the following command to rotate the logs daily\. Each day the system creates a new log and appends a time stamp to the file name\.

  ```
  $ sudo /opt/cloudhsm/bin/configure-jce --log-rotation daily
  ```
For more information about the `log-file`, `log-level`, and `log-rotation` parameters, see [Parameters](#configure-tool-params5)\.

### Place the issuing certificate for Client SDK 5<a name="ex6"></a>

**Example**  
This example uses the `--hsm-ca-cert` parameter to update the location of the issuing certificate for Client SDK 5\.   

**To place the issuing certificate on Linux for Client SDK 5**
+  Use the configure tool to specify a location for the issuing certificate\. 

  ```
  $ sudo /opt/cloudhsm/bin/configure-pkcs11 --hsm-ca-cert <customerCA certificate file>
  ```

**To place the issuing certificate on Windows for Client SDK 5**
+  Use the configure tool to specify a location for the issuing certificate\. 

  ```
  C:\Program Files\Amazon\CloudHSM\configure-pkcs11.exe --hsm-ca-cert <customerCA certificate file>
  ```

**To place the issuing certificate on Linux for Client SDK 5**
+  Use the configure tool to specify a location for the issuing certificate\. 

  ```
  $ sudo /opt/cloudhsm/bin/configure-dyn --hsm-ca-cert <customerCA certificate file>
  ```

**To place the issuing certificate on Linux for Client SDK 5**
+  Use the configure tool to specify a location for the issuing certificate\. 

  ```
  $ sudo /opt/cloudhsm/bin/configure-jce --hsm-ca-cert <customerCA certificate file>
  ```

**To place the issuing certificate on Windows for Client SDK 5**
+  Use the configure tool to specify a location for the issuing certificate\. 

  ```
  C:\Program Files\Amazon\CloudHSM\configure-jce.exe --hsm-ca-cert <customerCA certificate file>
  ```
For more information about the `--hsm-ca-cert` parameter, see [Parameters](#configure-tool-params5)\.

## Parameters<a name="configure-tool-params5"></a>

**\-a *<ENI IP address>***  
Adds the specified IP address to Client SDK 5 configuration files\. Enter any ENI IP address of a HSM from the cluster\. For more information about how to use this option, see [Bootstrap Client SDK 5](cluster-connect.md#sdk8-connect)\.  
Required: Yes

**\-\-hsm\-ca\-cert *<CustomerCA/certificate/file>***  
 Path to the directory storing the certificate authority \(CA\) certificate use to connect EC2 client instances to the cluster\. You create this file when you initialize the cluster\. By default, the system looks for this file in the following location:   
Linux  

```
/opt/cloudhsm/etc/customerCA.crt
```
Windows  

```
C:\ProgramData\Amazon\CloudHSM\customerCA.crt
```
For more information about initializing the cluster or placing the certificate, see [Place the issuing certificate](cluster-connect.md#place-hsm-cert) and [Initialize the cluster](initialize-cluster.md)\.  
Required: No

**\-\-cluster\-id *<Cluster ID>***  
 Makes a `DescribeClusters` call to find all of the HSM elastic network interface \(ENI\) IP addresses in the cluster associated with the cluster ID\. The system adds the ENI IP addresses to the AWS CloudHSM configuration files\.  
If you use the `--cluster-id` parameter from an EC2 instance within a VPC that does not have access to the public internet, then you must create an interface VPC endpoint to connect with AWS CloudHSM\. For more information about VPC endpoints, see [AWS CloudHSM and VPC endpoints](cloudhsm-vpc-endpoint.md)\.
Required: No

**\-\-endpoint *<Endpoint>***  
Specify the AWS CloudHSM API endpoint used for making the `DescribeClusters` call\. You must set this option in combination with `--cluster-id`\.   
Required: No

**\-\-region *<Region>***  
Specify the region of your cluster\. You must set this option in combination with `--cluster-id`\.  
If you don’t supply the `--region` parameter, the system chooses the region by attempting to read the `AWS_DEFAULT_REGION` or `AWS_REGION` environment variables\. If those variables aren’t set, then the system checks the region associated with your profile in your AWS config file \(typically `~/.aws/config`\) unless you specified a different file in the `AWS_CONFIG_FILE` environment variable\. If none of the above are set, the system defaults to the `us-east-1` region\.  
Required: No

**\-\-server\-client\-cert\-file *<Client/certificate/file>***  
 Path to the client certificate used for TLS client\-server mutual authentication\.   
 Only use this option if you don’t wish to use the default key and SSL/TLS certificate we include with Client SDK 5\. You must set this option in combination with `--server-client-key-file`\.   
Required: No

**\-\-server\-client\-key\-file *<Client/key/file>***  
 Path to the client key used for TLS client\-server mutual authentication   
 Only use this option if you don’t wish to use the default key and SSL/TLS certificate we include with Client SDK 5\. You must set this option in combination with `--server-client-cert-file`\.   
Required: No

**\-\-log\-level *<error \| warn \| info \| debug \| trace>***  
Specifies the minimum logging level the system should write to the log file\. Each level includes the previous levels, with error as the minimum level and trace the maximum level\. This means that if you specify errors, the system only writes errors to the log\. If you specify trace, the system writes errors, warnings, informational \(info\) and debug messages to the log\. For more information, see [Client SDK 5 Logging](hsm-client-logs.md#sdk5-logging)\.  
Required: No

**\-\-log\-rotation *<weekly \| daily>***  
Specifies the frequency with which the system rotates logs\. For more information, see [Client SDK 5 Logging](hsm-client-logs.md#sdk5-logging)\.  
Required: No

**\-\-log\-file *<Path/log/file>***  
Specifies where the system will write the log file\. For more information, see [Client SDK 5 Logging](hsm-client-logs.md#sdk5-logging)\.  
Required: No

**\-h \| \-\-help**  
Displays help\.  
Required: No

**\-v \| \-\-version**  
Displays version\.  
Required: No

**\-\-disable\-key\-availability\-check **  
Flag to disable key availability quorum\. Use this flag to indicate AWS CloudHSM should disable key availability quorum and you can use keys that exist on only one HSM in the cluster\. For more information about using this flag to set key availability quorum, see [Managing client key durability settings](manage-key-sync.md#setting-file-sdk8)\.  
Required: No

**\-\-enable\-key\-availability\-check **  
Flag to enable key availability quorum\. Use this flag to indicate AWS CloudHSM should use key availability quorum and not allow you to use keys until those keys exist on two HSMs in the cluster\. For more information about using this flag to set key availability quorum, see [Managing client key durability settings](manage-key-sync.md#setting-file-sdk8)\.  
Enabled by default\.  
Required: No

**\-\-disable\-validate\-key\-at\-init **  
Improves performance by specifying that you can skip an initialization call to verify permissions on a key for subsequent calls\. Use with caution\.  
Background: Some mechanisms in the PKCS \#11 library support multi\-part operations where an initialization call verifies if you can use the key for subsequent calls\. This requires a verification call to the HSM, which adds latency to the overall operation\. This option enables you to disable the subsequent call and potentially improve performance\.  
Required: No

**\-\-enable\-validate\-key\-at\-init **  
Specifies that you should use an initialization call to verify permissions on a key for subsequent calls\. This is the default option\. Use `enable-validate-key-at-init` to resume these initialization calls after you use `disable-validate-key-at-init` to suspend them\.  
Required: No

## Related topics<a name="configure-tool-seealso5"></a>
+ [DescribeClusters](https://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_DescribeClusters.html) API operation
+ [describe\-clusters](https://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/describe-clusters.html) AWS CLI
+ [Get\-HSM2Cluster](https://docs.aws.amazon.com/powershell/latest/reference/items/Get-HSM2Cluster.html) PowerShell cmdlet
+ [Bootstrap Client SDK 5](cluster-connect.md#sdk8-connect)
+ [AWS CloudHSM VPC endpoints](cloudhsm-vpc-endpoint.md)
+ [Managing Client SDK 5 Key Durability Settings](manage-key-sync.md#setting-file-sdk8)
+ [Client SDK 5 Logging](hsm-client-logs.md#sdk5-logging)