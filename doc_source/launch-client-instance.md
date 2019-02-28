# Launch an Amazon EC2 Client Instance<a name="launch-client-instance"></a>

 To interact with and manage your AWS CloudHSM cluster and HSM instances, you must be able to communicate with the elastic network interfaces of your HSMs\. The easiest way to do this is to use an EC2 instance in the same VPC as your cluster\. You can also use the following AWS resources to connect to your cluster: 
+ [Amazon VPC Peering](https://docs.aws.amazon.com/vpc/latest/peering/Welcome.html)
+ [AWS Direct Connect](https://docs.aws.amazon.com/directconnect/latest/UserGuide/Welcome.html)
+ [VPN Connections](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpn-connections.html)

 The AWS CloudHSM documentation typically assumes that you are using an EC2 instance in the same VPC and Availability Zone \(AZ\) in which you create your cluster\. 

**To create an EC2 instance**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. On the **EC2 Dashboard**, choose **Launch Instance**\.

1. Choose **Select** for an Amazon Machine Image \(AMI\)\. Choose a Linux AMI or a Windows Server AMI\.

1. Choose an instance type and then choose **Next: Configure Instance Details**\.

1. For **Network**, choose the VPC that you previously created for your cluster\.

1. For **Subnet**, choose the public subnet that you created for the VPC\.

1. For **Auto\-assign Public IP**, choose **Enable**\.

1. Choose **Next: Add Storage** and configure your storage\.

1. Choose **Next: Add Tags** and add any name–value pairs that you want to associate with the instance\. We recommend that you at least add a name\. Choose **Add Tag** and type a name for the **Key** and up to 255 characters for the **Value**\. 

1. Choose **Next: Configure Security Group**

1.  For **Assign a security group**, choose **Select an existing security group**\. 

1. Choose the default Amazon VPC security group from the list\.

1. Choose **Review and Launch**\.

   On the **Review Instance Launch** page, choose **Launch**\.

1.  When prompted for a key pair, choose **Create a new key pair**, enter a name for the key pair, and then choose **Download Key Pair**\. This is the only chance for you to save the private key file, so download it and store it in a safe place\. You must provide the name of your key pair when you launch an instance\. In addition, you must provide the corresponding private key each time that you connect to the instance\. Then choose the key pair that you created when getting set up\. 

   Alternatively, you can use an existing key pair\. Choose **Choose an existing key pair**, and then choose the desired key pair\. 
**Warning**  
Don't choose **Proceed without a key pair**\. If you launch your instance without a key pair, you won't be able to connect to it\.

   When you are ready, select the acknowledgement check box, and then choose **Launch Instances**\.

For more information about creating a Linux Amazon EC2 client, see [Getting Started with Amazon EC2 Linux Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html)\. For information about connecting to the running client, see the following topics: 
+ [Connecting to Your Linux Instance Using SSH](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstancesLinux.html)
+ [Connecting to Your Linux Instance from Windows Using PuTTY](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/putty.html)

 The Amazon EC2 user guide contains detailed instructions for setting up and using your Amazon EC2 instances\. The following list provides an overview of available documentation for Linux and Windows Amazon EC2 clients: 
+ To create a Linux Amazon EC2 client, see [Getting Started with Amazon EC2 Linux Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html)\.

  For information about connecting to the running client, see the following topics:
  + [Connecting to your Linux Instance Using SSH](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstancesLinux.html)
  + [Connecting to Your Linux Instance from Windows Using PuTTY](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/putty.html)
+  To create a Windows Amazon EC2 client, see [Getting Started with Amazon EC2 Windows Instances](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/EC2_GetStarted.html)\. For more information about connecting to your Windows client, see [Connect to Your Windows Instance](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/EC2_GetStarted.html#ec2-connect-to-instance-windows)\. 

**Note**  
 Your EC2 instance can run all of the AWS CLI commands contained in this guide\. If the AWS CLI is not installed, you can download it from [AWS Command Line Interface](https://aws.amazon.com/cli/)\. If you are using Windows, you can download and run a 64\-bit or 32\-bit Windows installer\. If you are using Linux or macOS, you can install the CLI using pip\. 