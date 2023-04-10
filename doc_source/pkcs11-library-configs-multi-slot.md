# Connecting to multiple slots with PKCS\#11<a name="pkcs11-library-configs-multi-slot"></a>

A single slot in Client SDK 5 PKCS \#11 library represents a single connection to a cluster in AWS CloudHSM\. With Client SDK 5, you can configure your PKCS11 library to allow multiple slots to connect users to multiple CloudHSM clusters from a single PKCS\#11 application\. 

Use the instructions in this topic to make your application use multi\-slot functionality to connect with multiple clusters\.

**Topics**
+ [Multi\-slot prerequisites](#pkcs11-multi-slot-prereqs)
+ [Configure the PKCS \#11 Library for multi\-slot functionality](#pkcs11-multi-slot-config-run)
+ [configure\-pkcs11 add\-cluster](#pkcs11-multi-slot-add-cluster)
+ [configure\-pkcs11 remove\-cluster](#pkcs11-multi-slot-remove-cluster)

## Multi\-slot prerequisites<a name="pkcs11-multi-slot-prereqs"></a>
+ Two or more AWS CloudHSM clusters to which you’d like to connect to, along with their cluster certificates \(that is, the customerCA\.crt file for each cluster\)
+ An EC2 instance with Security Groups correctly configured to connect to all of the clusters above\. For more information about how to set up a cluster and the client instance, refer to [Getting started with AWS CloudHSM](getting-started.md)\.
+ To set up multi\-slot functionality, you must have already downloaded and installed the PKCS \#11 library\. If you have not already done this, refer to the instructions in [Install Client SDK 5 for PKCS \#11 library](pkcs11-library-install.md)\.

## Configure the PKCS \#11 Library for multi\-slot functionality<a name="pkcs11-multi-slot-config-run"></a>

To configure your PKCS \#11 library for multi\-slot functionality, follow these steps:

1. Identify the clusters you want to connect to using multi\-slot functionality\.

1. Add these clusters to your PKCS \#11 configuration by following the instructions in [configure\-pkcs11 add\-cluster](#pkcs11-multi-slot-add-cluster)

1. Run the PKCS \#11 application to activate multi\-slot functionality\.

## configure\-pkcs11 add\-cluster<a name="pkcs11-multi-slot-add-cluster"></a>

When [connecting to multiple slots with PKCS \#11](#pkcs11-library-configs-multi-slot), use the configure\-pkcs11 add\-cluster command to add a cluster to your configuration\.

### Syntax<a name="pkcs11-multi-slot-add-cluster-syntax"></a>

```
configure-pkcs11 add-cluster [OPTIONS]
        --cluster-id <CLUSTER ID> 
        [--region <REGION>]
        [--endpoint <ENDPOINT>]
        [--hsm-ca-cert <HSM CA CERTIFICATE FILE>]
        [--server-client-cert-file <CLIENT CERTIFICATE FILE>]
        [--server-client-key-file <CLIENT KEY FILE>]
        [-h, --help]
```

### Examples<a name="pkcs11-multi-slot-add-cluster-examples"></a>

#### Add a cluster using the `cluster-id` parameter<a name="w13aac21c13c31b7c13b7b3b1"></a>

**Example**  
 Use the configure\-pkcs11 add\-cluster along with the `cluster-id` parameter to add a cluster \(with the ID of `cluster-1234567`\) to your configuration\.   

```
$ sudo /opt/cloudhsm/bin/configure-pkcs11 add-cluster --cluster-id cluster-1234567
```

```
C:\Program Files\Amazon\CloudHSM\> .\configure-pkcs11.exe add-cluster --cluster-id cluster-1234567
```

**Tip**  
If using configure\-pkcs11 add\-cluster with the `cluster-id` parameter doesn't result in the cluster being added, refer to the following example for a longer version of this command that also requires `--region` and `endpoint` parameters to identify the cluster being added\. If, for example, the region of the cluster is different than the one configured as your AWS CLI default, you should use the `--region` parameter to use the correct region\. Additionally, you have the ability to specify the AWS CloudHSM API endpoint to use for the call, which may be necessary for various network setups, such as using VPC interface endpoints that don’t use the default DNS hostname for AWS CloudHSM\.

#### Add a cluster using `cluster-id`, `endpoint`, and `region` parameters<a name="w13aac21c13c31b7c13b7b3b3"></a>

**Example**  
 Use the configure\-pkcs11 add\-cluster along with the `cluster-id`, `endpoint`, and `region` parameters to add a cluster \(with the ID of `cluster-1234567`\) to your configuration\.   

```
        $ sudo /opt/cloudhsm/bin/configure-pkcs11 add-cluster --cluster-id cluster-1234567 --region us-east-1 --endpoint https://cloudhsmv2.us-east-1.amazonaws.com
```

```
        C:\Program Files\Amazon\CloudHSM\> .\configure-pkcs11.exe add-cluster --cluster-id cluster-1234567--region us-east-1 --endpoint https://cloudhsmv2.us-east-1.amazonaws.com
```

For more information about the `--cluster-id`, `--region`, and `--endpoint` parameters, see [Parameters](configure-sdk-5.md#configure-tool-params5)\.

### Parameters<a name="pkcs11-multi-slot-add-cluster-parameters"></a>

**\-\-cluster\-id *<Cluster ID>***  
 Makes a `DescribeClusters` call to find all of the HSM elastic network interface \(ENI\) IP addresses in the cluster associated with the cluster ID\. The system adds the ENI IP addresses to the AWS CloudHSM configuration files\.  
If you use the `--cluster-id` parameter from an EC2 instance within a VPC that does not have access to the public internet, then you must create an interface VPC endpoint to connect with AWS CloudHSM\. For more information about VPC endpoints, see [AWS CloudHSM and VPC endpoints](cloudhsm-vpc-endpoint.md)\.
Required: Yes

**\-\-endpoint *<Endpoint>***  
Specify the AWS CloudHSM API endpoint used for making the `DescribeClusters` call\. You must set this option in combination with `--cluster-id`\.   
Required: No

**\-\-hsm\-ca\-cert *<HsmCA/Certificate/file>***  
Specifies the HSM CA certificate file  
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

## configure\-pkcs11 remove\-cluster<a name="pkcs11-multi-slot-remove-cluster"></a>

When [connecting to multiple slots with PKCS\#11](#pkcs11-library-configs-multi-slot), use the configure\-pkcs11 remove\-cluster command to remove a cluster from available PKCS \#11 slots\.

### Syntax<a name="pkcs11-multi-slot-remove-cluster-syntax"></a>

```
configure-pkcs11 remove-cluster [OPTIONS]
        --cluster-id <CLUSTER ID>
        [-h, --help]
```

### Examples<a name="pkcs11-multi-slot-remove-cluster-examples"></a>

#### Remove a cluster using the `cluster-id` parameter<a name="w13aac21c13c31b7c15b7b3b1"></a>

**Example**  
 Use the configure\-pkcs11 remove\-cluster along with the `cluster-id` parameter to remove a cluster \(with the ID of `cluster-1234567`\) from your configuration\.   

```
$ sudo /opt/cloudhsm/bin/configure-pkcs11 remove-cluster --cluster-id cluster-1234567
```

```
C:\Program Files\Amazon\CloudHSM\> .\configure-pkcs11.exe remove-cluster --cluster-id cluster-1234567
```

For more information about the `--cluster-id` parameter, see [Parameters](configure-sdk-5.md#configure-tool-params5)\.

### Parameter<a name="pkcs11-multi-slot-remove-cluster-parameters"></a>

**\-\-cluster\-id *<Cluster ID>***  
 The ID of the cluster to remove from the configuration  
Required: Yes