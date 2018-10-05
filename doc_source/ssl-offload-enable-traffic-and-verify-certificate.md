# Step 4: Enable HTTPS Traffic and Verify the Certificate<a name="ssl-offload-enable-traffic-and-verify-certificate"></a>

After you configure your web server for SSL/TLS offload with AWS CloudHSM, add your web server instance to a security group that allows inbound HTTPS traffic\. This allows clients, such as web browsers, to establish an HTTPS connection with your web server\. Then make an HTTPS connection to your web server and verify that it's using the certificate that you configured for SSL/TLS offload with AWS CloudHSM\.

**Topics**
+ [Enable Inbound HTTPS Connections](#ssl-offload-add-security-group-linux)
+ [Verify That HTTPS Uses the Certificate That You Configured](#ssl-offload-verify-https-connection-linux)

## Enable Inbound HTTPS Connections<a name="ssl-offload-add-security-group-linux"></a>

To connect to your web server from a client \(such as a web browser\), create a security group that allows inbound HTTPS connections\. Specifically, it should allow inbound TCP connections on port 443\. Assign this security group to your web server\. 

**To create a security group for HTTPS and assign it to your web server**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. Choose **Security Groups** in the navigation pane\.

1. Choose **Create Security Group**\.

1. For **Create Security Group**, do the following:

   1. For **Security group name**, type a name for the security group that you are creating\.

   1. \(Optional\) Type a description of the security group that you are creating\.

   1. For **VPC**, choose the VPC that contains your web server Amazon EC2 instance\.

   1. Choose **Add Rule**\.

   1. For **Type**, choose **HTTPS**\.

1. Choose **Create**\.

1. In the navigation pane, choose **Instances**\.

1. Select the check box next to your web server instance\. Then choose **Actions**, **Networking**, and **Change Security Groups**\.

1. Select the check box next to the security group that you created for HTTPS\. Then choose **Assign Security Groups**\.

## Verify That HTTPS Uses the Certificate That You Configured<a name="ssl-offload-verify-https-connection-linux"></a>

After you add the web server to a security group, you can verify that SSL/TLS offload with AWS CloudHSM is working\. You can do this with a web browser or with a tool such as [OpenSSL s\_client](https://www.openssl.org/docs/manmaster/man1/s_client.html)\.

**To verify SSL/TLS offload with a web browser**

1. Use a web browser to connect to your web server using the public DNS name or IP address of the server\. Ensure that the URL in the address bar begins with https://\. For example, **https://ec2\-52\-14\-212\-67\.us\-east\-2\.compute\.amazonaws\.com/**\.
**Tip**  
You can use a DNS service such as Amazon Route 53 to route your website's domain name \(for example, https://www\.example\.com/\) to your web server\. For more information, see [Routing Traffic to an Amazon EC2 Instance](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-to-ec2-instance.html) in the *Amazon Route 53 Developer Guide* or in the documentation for your DNS service\.

1. Use your web browser to view the web server certificate\. For more information, see the following:
   + For Mozilla Firefox, see [View a Certificate](https://support.mozilla.org/en-US/kb/secure-website-certificate#w_view-a-certificate) on the Mozilla Support website\.
   + For Google Chrome, see [Understand Security Issues](https://developers.google.com/web/tools/chrome-devtools/security) on the Google Tools for Web Developers website\.

   Other web browsers might have similar features that you can use to view the web server certificate\.

1. Ensure that the SSL/TLS certificate is the one that you configured your web server to use\.

**To verify SSL/TLS offload with OpenSSL s\_client**

1. Run the following OpenSSL command to connect to your web server using HTTPS\. Replace *<server name>* with the public DNS name or IP address of your web server\. 

   ```
   openssl s_client -connect <server name>:443
   ```
**Tip**  
You can use a DNS service such as Amazon Route 53 to route your website's domain name \(for example, https://www\.example\.com/\) to your web server\. For more information, see [Routing Traffic to an Amazon EC2 Instance](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-to-ec2-instance.html) in the *Amazon Route 53 Developer Guide* or in the documentation for your DNS service\.

1. Ensure that the SSL/TLS certificate is the one that you configured your web server to use\.

You now have a website that is secured with HTTPS\. The private key for the web server is stored in an HSM in your AWS CloudHSM cluster\. However, you have only one web server\. To set up a second web server and a load balancer for higher availability, go to [\(Optional\) Step 5: Add a Load Balancer with Elastic Load Balancing](ssl-offload-add-load-balancing.md)\.