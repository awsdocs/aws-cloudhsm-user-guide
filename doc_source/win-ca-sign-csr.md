# Windows Server CA step 3: Sign a certificate signing request \(CSR\) with your Windows Server CA with AWS CloudHSM<a name="win-ca-sign-csr"></a>

You can use your Windows Server CA with AWS CloudHSM to sign a certificate signing request \(CSR\)\. To complete these steps, you need a valid CSR\. You can create a CSR in several ways, including the following:
+ Using OpenSSL
+ Using the Windows Server Internet Information Services \(IIS\) Manager
+ Using the certificates snap\-in in the Microsoft Management Console
+ Using the **certreq** command line utility on Windows

The steps for creating a CSR are outside the scope of this tutorial\. When you have a CSR, you can sign it with your Windows Server CA\.

**To sign a CSR with your Windows Server CA**

1. If you haven't already done so, connect to your Windows server\. For more information, see [Connect to Your Instance](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/EC2_GetStarted.html#ec2-connect-to-instance-windows) in the *Amazon EC2 User Guide for Windows Instances*\.

1. On your Windows server, start **Server Manager**\.

1. In the **Server Manager** dashboard, in the top right corner, choose **Tools**, **Certification Authority**\.

1. In the **Certification Authority** window, choose your computer name\.

1. From the **Action** menu, choose **All Tasks**, **Submit new request**\.

1. Select your CSR file, and then choose **Open**\.

1. In the **Certification Authority** window, double\-click **Pending Requests**\.

1. Select the pending request\. Then, from the **Action** menu, choose **All Tasks**, **Issue**\.

1. In the **Certification Authority** window, double\-click **Issued Requests** to view the signed certificate\.

1. \(Optional\) To export the signed certificate to a file, complete the following steps:

   1. In the **Certification Authority** window, double\-click the certificate\.

   1. Choose the **Details** tab, and then choose **Copy to File**\.

   1. Follow the instructions in the **Certificate Export Wizard**\.

You now have a Windows Server CA with AWS CloudHSM, and a valid certificate signed by the Windows Server CA\.