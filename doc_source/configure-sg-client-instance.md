# Configure the Client Amazon EC2 instance security groups<a name="configure-sg-client-instance"></a>

When you launched an Amazon EC2 instance, you associated it with a default Amazon VPC security group\. This topic explains how to associate the cluster security group with the EC2 instance\. This association allows the AWS CloudHSM client running on your EC2 instance to communicate with your HSMs\. To connect your EC2 instance to your AWS CloudHSM cluster, you must properly configure the VPC default security group *and* associate the cluster security group with the instance\.

## Modify the default security group<a name="configure-sg-client-instance-modify-default-security-group"></a>

You need to modify the default security group to permit the SSH or RDP connection so that you can download and install client software, and interact with your HSM\.

**To modify the default security group**

1. Open the **EC2 Dashboard** at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. Select **Instances \(running\)** and then select the check box for the EC2 instance on which you want to install the AWS CloudHSM client\.

1. Under the **Security** tab, choose the security group named **Default**\.

1. At the top of the page, choose **Actions**, and then **Edit Inbound Rules**\.

1. Select **Add Rule**\.

1. For **Type**, do one of the following:
   + For a Windows Server Amazon EC2 instance, choose **RDP**\. The port range `3389` is automatically populated\.
   + For a Linux Amazon EC2 instance, choose **SSH**\. The port range `22` is automatically populated\.

1. For either option, set **Source** to **My IP** to allow the client to communicate with the AWS CloudHSM cluster\.
**Important**  
Do not specify 0\.0\.0\.0/0 as the port range to avoid allowing anyone to access your instance\.

1. Choose **Save**\.

## Connect the Amazon EC2 instance to the AWS CloudHSM cluster<a name="configure-sg-client-instance-connect-the-ec2-instance-to-the-HSM-cluster"></a>

You must attach the cluster security group to the EC2 instance so that the EC2 instance can communicate with HSMs in your cluster\. The cluster security group contains a preconfigured rule that allows inbound communication over ports 2223\-2225\.

**To connect the EC2 instance to the AWS CloudHSM cluster**

1. Open the **EC2 Dashboard** at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. Select **Instances \(running\)** and then select the check box for the EC2 instance on which you want to install the AWS CloudHSM client\.

1. At the top of the page, choose **Actions**, **Security**, and then **Change Security Groups**\.

1. Select the security group with the group name that matches your cluster ID, such as `cloudhsm-cluster-clusterID-sg`\.

1. Choose **Add Security Groups**\.

1. Select **Save**\.

**Note**  
 You can assign a maximum of five security groups to an Amazon EC2 instance\. If you have reached the maximum limit, you must modify the default security group of the Amazon EC2 instance and the cluster security group:  
In the default security group, do the following:  
Add an outbound rule to permit traffic on all ports to `0.0.0.0/0`\.
Add an inbound rule to permit traffic using the TCP protocol over ports `2223-2225` from the cluster security group\.
In the cluster security group, do the following:  
Add an outbound rule to permit traffic on all ports to `0.0.0.0/0`\.
Add an inbound rule to permit traffic using the TCP protocol over ports `2223-2225` from the default security group\.