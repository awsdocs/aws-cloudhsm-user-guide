# Step 1: Set up the prerequisites<a name="third-offload-linux-jsse-prereqs"></a>

Follow these prerequisites to use a Tomcat web server with AWS CloudHSM for SSL/TLS offload on Linux\. These prerequisites must be met to set up web server SSL/TLS offload with Client SDK 5 and a Tomcat web server\.

**Note**  
Different platforms require different prerequisites\. Always follow the correct installation steps for your platform\.

## Prerequisites<a name="new-versions-jsse"></a>

### <a name="w13aac23b7c17b9b9b7b3"></a>
+ An Amazon EC2 instance running a Linux operating system with A tomcat web server installed\.
+ A [crypto user](manage-hsm-users-chsm-cli.md#crypto-user-chsm-cli) \(CU\) to own and manage the web server's private key on the HSM\.
+ An active AWS CloudHSM cluster with at least two hardware security modules \(HSMs\) that have [JCE for Client SDK 5](java-library-install_5.md) installed and configured\.
**Note**  
You can use a single HSM cluster, but you must first disable client key durability\. For more information, see [Manage Client Key Durability Settings](manage-key-sync.md#client-sync-sdk8) and [Client SDK 5 Configure Tool](configure-sdk-5.md)\.

#### How to meet the prerequisites<a name="jsse-prereqs-how-to"></a>

1. Install and configure the JCE for AWS CloudHSM on an active AWS CloudHSM cluster with at least two hardware security modules \(HSMs\)\. For more information about installation, see [JCE for Client SDK 5](java-library-install_5.md)\.

1. On an EC2 Linux instance that has access to your AWS CloudHSM cluster, follow the [Apache Tomcat instructions](https://tomcat.apache.org/download-90.cgi ) to download and install the Tomcat web server\.

1. Use [CloudHSM CLI](cloudhsm_cli.md) to create a crypto user \(CU\)\. For more information about managing HSM users, see [Managing HSM users with CloudHSM CLI](manage-hsm-users-chsm-cli.md)\. 
**Tip**  
Keep track of the CU user name and password\. You will need them later when you generate or import the HTTPS private key and certificate for your web server\.

1. To setup JCE with Java Keytool, follow the instructions in [Using Client SDK 5 to integrate with Java Keytool and Jarsigner](keystore-third-party-tools_5.md)\.

After you complete these steps, go to [Step 2: Generate or import a private key and SSL/TLS certificate](third-offload-linux-jsse-gen.md)\.

#### Notes<a name="jsse-prereqs-notes"></a>
+ To use Security\-Enhanced Linux \(SELinux\) and web servers, you must allow outbound TCP connections on port 2223, which is the port Client SDK 5 uses to communicate with the HSM\.
+ To create and activate a cluster and give an EC2 instance access to the cluster, complete the steps in [Getting Started with AWS CloudHSM](getting-started.md)\. This section offers step\-by\-step instructions for creating an active cluster with one HSM and an Amazon EC2 client instance\. You can use this client instance as your web server\. 
+ To avoid disabling client key durability, add more than one HSM to your cluster\. For more information, see [Adding an HSM](add-remove-hsm.md#add-hsm)\.
+ To connect to your client instance, you can use SSH or PuTTY\. For more information, see [Connecting to Your Linux Instance Using SSH](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstancesLinux.html) or [Connecting to Your Linux Instance from Windows Using PuTTY](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/putty.html) in the Amazon EC2 documentation\. 