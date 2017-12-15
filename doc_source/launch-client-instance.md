# Getting Started with AWS CloudHSM: Launch a Client Instance and Add the Cluster Security Group<a name="launch-client-instance"></a>

Complete the following steps to create an Amazon EC2 instance, known as a client instance\. Later you install and configure the AWS CloudHSM client software, which you use to access your cluster from the client instance\.

**Important**  
Make sure that you assign the cluster's security group, named **cloudhsm\-*<cluster ID>*\-sg**, to your client instance as described in the following procedure\. If you don't, you won't be able to connect to your cluster from the client instance\.  
Ensure that only trusted administrators can access this client instance and all instances in the cluster's security group\. For more information, see the warning in [Create a Cluster](create-cluster-and-hsm.md#create-cluster)\.


+ [Launching a Client Instance](#launch-client-instance-steps)
+ [Connecting to Your Client Instance](#connect-to-client-instance)

## Launching a Client Instance<a name="launch-client-instance-steps"></a>

Complete the following steps to launch a client instance\.

**To launch a client instance**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. On the navigation bar, choose the AWS Region where you created your cluster\.

1. Choose **Launch Instance**\.

1. In the row for the newest **Amazon Linux AMI**, choose **Select**\.

1. Select an instance type, and then choose **Next: Configure Instance Details**\.

1. For **Step 3: Configure Instance Details**, do the following:

   1. For **Network**, choose the VPC that you created previously\.

   1. For **Subnet**, choose the public subnet that you created previously\.

   1. For **Auto\-assign Public IP**, choose **Enable**\.

   1. Change the remaining instance details as preferred\. Then choose **Next: Add Storage**\.

1. Change the storage settings as preferred\. Then choose **Next: Add Tags**\.

1. Add tags as preferred\. Then choose **Next: Configure Security Group**\.

1. For **Step 6: Configure Security Group**, do the following:

   1. For **Assign a security group**, choose **Select an existing security group**\.

   1. Select the check box next to the security group named **cloudhsm\-*<cluster ID>*\-sg**\. AWS CloudHSM created this security group on your behalf when you created the cluster\. You must choose this security group to allow the client instance to connect to the HSMs in the cluster\.

   1. Select the check box next to a security group that allows inbound SSH traffic from your network\. That is, the security group must allow inbound TCP traffic on port 22\. Otherwise, you cannot connect to your client instance with SSH\. If you don't have a security group like this, you must create one and then assign it to your client instance later\. For more information about creating a security group, see [Create a Security Group](http://docs.aws.amazon.com/AmazonVPC/latest/GettingStartedGuide/getting-started-ipv4.html#getting-started-create-security-group) in the *Amazon VPC Getting Started Guide*\.

   Then choose **Review and Launch**\.

1. Review your instance details, and then choose **Launch**\.

1. Choose whether to launch your instance with an existing key pair or create a new key pair\.

   + To use an existing key pair, do the following:

     1. Choose **Choose an existing key pair**\.

     1. For **Select a key pair**, choose the key pair to use\.

     1. Select the check box next to **I acknowledge that I have access to the selected private key file \(*private key file name*\.pem\), and that without this file, I won't be able to log into my instance\.**

   + To create a new key pair, do the following:

     1. Choose **Create a new key pair**\.

     1. For **Key pair name**, type an identifiable key pair name such as **CloudHSM client instance key pair**\.

     1. Choose **Download Key Pair** and save the private key file in a secure and accessible location\.
**Warning**  
You cannot download the private key file again after this point\. If you do not download the private key file now, you will be unable to access the client instance\.

   Then choose **Launch Instances**\.

## Connecting to Your Client Instance<a name="connect-to-client-instance"></a>

When the client instance is running, you can connect to it using SSH\. For more information, see one of the following topics\.

+ If your computer's operating system is Linux or Apple macOS, see [Connecting to Your Linux Instance Using SSH](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstancesLinux.html) in the *Amazon EC2 User Guide for Linux Instances*\.

+ If your computer's operating system is Microsoft Windows, see [Connecting to Your Linux Instance from Windows Using PuTTY](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/putty.html) in the *Amazon EC2 User Guide for Linux Instances*\.

After you connect to your client instance, proceed to [Install and Configure the Client](install-and-configure-client.md)\.