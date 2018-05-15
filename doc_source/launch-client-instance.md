# Launch an Amazon EC2 Client Instance<a name="launch-client-instance"></a>

To interact with and manage your AWS CloudHSM cluster and HSM instances, you must be able to communicate with the elastic network interfaces \(ENIs\) of your HSMs\. The easiest way to do this is to use an Amazon EC2 instance in the same Amazon VPC as your cluster \(see below\)\. You can also use the following AWS resources to connect to your cluster\. 
+ [Amazon VPC Peering](http://docs.aws.amazon.com/AmazonVPC/latest/PeeringGuide/Welcome.html)
+ [AWS Direct Connect](https://aws.amazon.com/documentation/direct-connect/)
+ [VPN Connections](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpn-connections.html)

## Launch an EC2 Client<a name="launch-client-instance-ec2"></a>

 The AWS CloudHSM documentation typically assumes that you are using an Amazon EC2 instance in the same Amazon Virtual Private Cloud \(VPC\) and Availability Zone \(AZ\) in which you create your cluster\. 

**To create an Amazon EC2 client instance**

1. Open the EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. Choose **Launch instance** on the **EC2 Dashboard**\.

1. Select an Amazon machine image \(AMI\)\. Choose a Linux AMI or a Windows Server AMI\.

1. Choose an instance type and then choose **Next: Configure Instance Details**\.

1. For **Network**, choose the VPC you previously created for your cluster\.

1. For **Subnet**, choose the public subnet that you created for the VPC\.

1. For **Auto\-assign Public IP**, choose **Enable**\.

1. Choose **Next: Add Storage** and configure your storage\.

1. Choose **Next: Add Tags** and add any name–value pairs that you want to associate with the instance\. We recommend that you at least add a name\. Choose **Add Tag** and add **Name** for the **Key** and a maximum 255 character string for the **Value**\. 

1. Choose **Next: Configure Security Group**\.

1. Choose **Select an existing security group**\. Select the default security group that was created when your cluster was created, or select a security group that you created specifically for your client\. 
**Note**  
To be able to connect to a Windows Server EC2 instance, you must set one of your **Inbound Rules** to RDP\(3389\) to allow incoming TCP traffic on port 3389\. To be able to connect to a Linux EC2 instance, you must set one of your **Inbound Rules** to SSH\(22\) to allow incoming TCP traffic on port 22\. Specify the source IP addresses that can connect to your instance\. You should not specify `0.0.0.0/0` because that will open your instance to access by anyone\.   
If you want your EC2 instance to be able to connect to the internet, set the **Outbound Rules** on your security group to allow **ALL Traffic** on all ports to a destination of `0.0.0.0/0`\. 

1. Choose **Review and Launch**\.

For more information about creating a Linux Amazon EC2 client, see [Getting Started with Amazon EC2 Linux Instances](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html)\. For information about connecting to the running client, see the following topics: 
+ [Connecting to Your Linux Instance Using SSH](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstancesLinux.html)
+ [Connecting to Your Linux Instance from Windows Using PuTTY](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/putty.html)

For more information about creating a Windows Amazon EC2 client, see [Getting Started with Amazon EC2 Windows Instances](http://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/EC2_GetStarted.html)\. For more information about connecting to your Windows client, see [Connect to Your Windows Instance](http://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/EC2_GetStarted.html#ec2-connect-to-instance-windows)\. 

Note that you can use your Amazon EC2 instance to run all of the AWS CLI commands contained in this guide\. If the AWS CLI is not installed, you can download it from [AWS Command Line Interface](https://aws.amazon.com/cli/)\. If you are using Windows, you can download and run a 64\-bit or 32\-bit Windows installer\. If you are using Linux or own a Mac, you can install the CLI using pip\. 

To communicate with the HSMs in your cluster, you must install the AWS CloudHSM client software on your instance\. For more information if you are using Linux, see [Install the Client \(Linux\)](install-and-configure-client-linux.md)\. For more information if you are using Windows, see [Install the Client \(Windows\)](install-and-configure-client-win.md)\. 