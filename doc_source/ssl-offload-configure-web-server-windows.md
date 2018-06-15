# Step 3: Configure the Web Server<a name="ssl-offload-configure-web-server-windows"></a>

Update your IIS website's configuration to use the HTTPS certificate that you created at the end of the [previous step](ssl-offload-windows-create-csr-and-certificate.md)\. This will finish setting up your Windows web server software \(IIS\) for SSL/TLS offload with AWS CloudHSM\.

If you used a self\-signed certificate to sign your CSR, you must first import the self\-signed certificate into the Windows Trusted Root Certification Authorities\.

**To import your self\-signed certificate into the Windows Trusted Root Certification Authorities**

1. If you haven't already done so, connect to your Windows server\. For more information, see [Connect to Your Instance](http://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/EC2_GetStarted.html#ec2-connect-to-instance-windows) in the *Amazon EC2 User Guide for Windows Instances*\.

1. Copy your self\-signed certificate to your Windows server\.

1. On your Windows Server, open the **Control Panel**\.

1. For **Search Control Panel**, type **certificates**\. Then choose **Manage computer certificates**\.

1. In the **Certificates \- Local Computer** window, double\-click **Trusted Root Certification Authorities**\.

1. Right\-click on **Certificates** and then choose **All Tasks**, **Import**\.

1. In the **Certificate Import Wizard**, choose **Next**\.

1. Choose **Browse**, then find and select your self\-signed certificate\. If you created your self\-signed certificate by following the instructions in the [previous step of this tutorial](ssl-offload-windows-create-csr-and-certificate.md), your self\-signed certificate is named `SelfSignedCA.crt`\. Choose **Open**\.

1. Choose **Next**\.

1. For **Certificate Store**, choose **Place all certificates in the following store**\. Then ensure that **Trusted Root Certification Authorities** is selected for **Certificate store**\.

1. Choose **Next** and then choose **Finish**\.

**To update the IIS website's configuration**

1. If you haven't already done so, connect to your Windows server\. For more information, see [Connect to Your Instance](http://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/EC2_GetStarted.html#ec2-connect-to-instance-windows) in the *Amazon EC2 User Guide for Windows Instances*\.

1. [Start the AWS CloudHSM client](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-start-cloudhsm-client)\.

1. Copy your web server's signed certificate—the one that you created at the end of [this tutorial's previous step](ssl-offload-windows-create-csr-and-certificate.md)—to your Windows server\.

1. On your Windows Server, use the [Windows certreq command](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/certreq_1) to accept the signed certificate, as in the following example\. Replace *IISCert\.crt* with the name of the file that contains your web server's signed certificate\.

   ```
   C:\>certreq -accept IISCert.crt
           SDK Version: 2.03
   ```

1. On your Windows server, start **Server Manager**\.

1. In the **Server Manager** dashboard, in the top right corner, choose **Tools**, **Internet Information Services \(IIS\) Manager**\.

1. In the **Internet Information Services \(IIS\) Manager** window, double\-click your server name\. Then double\-click **Sites**\. Select your website\.

1. Select **SSL Settings**\. Then, on the right side of the window, choose **Bindings**\.

1. In the **Site Bindings** window, choose **Add**\.

1. For **Type**, choose **https**\. For **SSL certificate**, choose the HTTPS certificate that you created at the end of [this tutorial's previous step](ssl-offload-windows-create-csr-and-certificate.md)\.

1. Choose **OK**\.

After you update your website's configuration, go to [Step 4: Enable HTTPS Traffic and Verify the Certificate](ssl-offload-enable-traffic-and-verify-certificate-windows.md)\.