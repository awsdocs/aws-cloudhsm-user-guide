# Web Server SSL/TLS Offload: Set Up the Prerequisites<a name="ssl-offload-prerequisites"></a>

To accomplish web server SSL/TLS offload with AWS CloudHSM, you need the following prerequisites:

+ An active AWS CloudHSM cluster with at least one HSM\.

+ An Amazon EC2 instance running the Amazon Linux operating system with the following software installed:

  + The AWS CloudHSM client and command line tools\.

  + The Apache or Nginx web server software\.

  + The AWS CloudHSM software library for OpenSSL\.

+ A crypto user \(CU\) to own and manage the private key on the HSMs in your cluster\.

Complete the following steps to prepare your environment for web server SSL/TLS offload with AWS CloudHSM\.

**To prepare your environment for web server SSL/TLS offload**

1. Complete the steps in [Getting Started: Create A Cluster](getting-started.md)\. After you complete these steps, you have an active cluster with one HSM\. You also have an Amazon EC2 instance, known as a *client instance*, running the Amazon Linux operating system\. The AWS CloudHSM client and command line tools are also installed and configured\.

1. \(Optional\) Add more HSMs to your cluster\. For more information, see [Adding an HSM](add-remove-hsm.md#add-hsm)\.

1. Connect to the client instance that you created previously\. On the client instance, do the following:

   1. Start the AWS CloudHSM client\.

   1. Update the configuration file for the command line tool known as cloudhsm\_mgmt\_util\.

   1. Use the command line tool known as cloudhsm\_mgmt\_util to create a crypto user \(CU\) on your cluster\. For more information, see [Managing HSM Users](manage-hsm-users.md)\.

   1. Install and configure the AWS CloudHSM software library for OpenSSL\.

   1. Choose whether to install the Apache or Nginx web server software\. Then complete one of the following steps:

      + Use the following command to install the Apache web server\.

        ```
        $ sudo yum install -y httpd24 mod24_ssl
        ```

      + Use the following command to install the Nginx web server\.

        ```
        $ sudo yum install -y nginx
        ```

After you complete these steps, you can import or generate a private key and certificate\.