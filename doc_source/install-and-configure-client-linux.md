# Install and Configure the AWS CloudHSM Client \(Linux\)<a name="install-and-configure-client-linux"></a>

To interact with the HSM in your AWS CloudHSM cluster, you need the AWS CloudHSM client software for Linux\. You should install it on the Linux EC2 client instance that you created previously\. You can also install a client if you are using Windows\. For more information, see [Install and Configure the AWS CloudHSM Client \(Windows\)](install-and-configure-client-win.md)\. 

**Topics**
+ [Install the AWS CloudHSM Client and Command Line Tools](#install-client)
+ [Edit the Client Configuration](#edit-client-configuration)

## Install the AWS CloudHSM Client and Command Line Tools<a name="install-client"></a>

Connect to your client instance and [install the latest AWS CloudHSM client and command line tools](client-history.md)\.

## Edit the Client Configuration<a name="edit-client-configuration"></a>

Before you can use the AWS CloudHSM client to connect to your cluster, you must edit the client configuration\.

**To edit the client configuration**

1. Copy your issuing certificate—[the one that you used to sign the cluster's certificate](initialize-cluster.md#sign-csr)—to the following location on the client instance: `/opt/cloudhsm/etc/customerCA.crt`\. You need instance root user permissions on the client instance to copy your certificate to this location\. 

1. Use the following [configure](configure-tool.md) command to update the configuration files for the AWS CloudHSM client and command line tools, specifying the IP address of the HSM in your cluster\. To get the HSM's IP address, view your cluster in the [AWS CloudHSM console](https://console.aws.amazon.com/cloudhsm/), or run the [describe\-clusters](https://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/describe-clusters.html) AWS CLI command\. In the command's output, the HSM's IP address is the value of the `EniIp` field\. If you have more than one HSM, choose the IP address for any of the HSMs; it doesn't matter which one\. 

   ```
   sudo /opt/cloudhsm/bin/configure -a <IP address>
   	
   Updating server config in /opt/cloudhsm/etc/cloudhsm_client.cfg
   Updating server config in /opt/cloudhsm/etc/cloudhsm_mgmt_util.cfg
   ```

1. Go to [Activate the Cluster](activate-cluster.md)\.