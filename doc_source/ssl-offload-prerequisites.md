# Web Server SSL/TLS Offload Step 1: Set Up the Prerequisites<a name="ssl-offload-prerequisites"></a>

To set up web server SSL/TLS offload with AWS CloudHSM, you need the following prerequisites:

+ An active AWS CloudHSM cluster with at least one HSM\.

+ An Amazon EC2 instance running a Linux operating system with the following software installed:

  + The AWS CloudHSM client and command line tools\.

  + The Nginx or Apache web server application\.

  + The AWS CloudHSM software library for OpenSSL\.

+ A crypto user \(CU\) to own and manage the private key on the HSMs in your cluster\.

To set up all of these prerequisites, complete the following steps\.

**To set up the prerequisites for web server SSL/TLS offload with AWS CloudHSM**

1. Complete the steps in [Getting Started: Create A Cluster](getting-started.md)\. After you complete these steps, you have an active cluster with one HSM\. You also have an Amazon EC2 instance, known as a *client instance*, with the AWS CloudHSM client and command line tools installed and configured\. You use this client instance as your web server\.

1. Connect to the client instance\. On the client instance, do the following:

   1. Choose whether to install the Nginx or Apache web server application\. Then complete one of the following steps:

      + To install Nginx, run the following command\.

        ```
        $ sudo yum install -y nginx
        ```

      + To install Apache HTTP Server, run the following command\.

        ```
        $ sudo yum install -y httpd24 mod24_ssl
        ```

   1. Install and configure the AWS CloudHSM software library for OpenSSL\.

1. \(Optional\) Add more HSMs to your cluster\. For more information, see [Adding an HSM](add-remove-hsm.md#add-hsm)\.

1. To create a CU on the HSMs, on the client instance, do the following:

   1. Start the AWS CloudHSM client\.

   1. Update the configuration file for the command line tool known as cloudhsm\_mgmt\_util\.

   1. Use cloudhsm\_mgmt\_util to create a CU\. For more information, see [Managing HSM Users](manage-hsm-users.md)\. Keep track of the CU's user name and password\. You need them later when you generate or import the web server's HTTPS private key\.

After you complete these steps, go to [Step 2: Import or Generate a Private Key and Certificate](ssl-offload-import-or-generate-private-key-and-certificate.md)\.