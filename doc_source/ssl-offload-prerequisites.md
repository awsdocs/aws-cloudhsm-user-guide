# Step 1: Set Up the Prerequisites<a name="ssl-offload-prerequisites"></a>

To set up web server SSL/TLS offload with AWS CloudHSM, you need the following:
+ An active AWS CloudHSM cluster with at least one HSM\.
+ An Amazon EC2 instance running a Linux operating system with the following software installed:
  + The AWS CloudHSM client and command line tools\.
  + The NGINX or Apache web server application\.
  + The AWS CloudHSM dynamic engine for OpenSSL\.
+ A [crypto user](hsm-users.md#crypto-user) \(CU\) to own and manage the web server's private key on the HSM\.

**To set up a Linux web server instance and create a CU on the HSM**

1. Complete the steps in [Getting Started](getting-started.md)\. You will then have an active cluster with one HSM and an Amazon EC2 client instance\. Your EC2 instance will be configured with the command line tools\. Use this client instance as your web server\. 

1. Connect to your client instance\. For more information, see [Connecting to Your Linux Instance Using SSH](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstancesLinux.html) or [Connecting to Your Linux Instance from Windows Using PuTTY](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/putty.html) in the Amazon EC2 documentation\. Then do the following:

   1. Choose whether to install the NGINX or Apache web server application\. Then complete one of the following steps:
      + To install NGINX, run the following command\.

        ```
        sudo yum install -y nginx
        ```
      + To install Apache, run the following command\.

        ```
        sudo yum install -y httpd24 mod24_ssl
        ```

   1. [Install and configure the OpenSSL engine](openssl-library-install.md#install-openssl-library)\.

1. \(Optional\) Add more HSMs to your cluster\. For more information, see [Adding an HSM](add-remove-hsm.md#add-hsm)\.

1. To create a [crypto user](hsm-users.md#crypto-user) \(CU\) on your HSM, do the following:

   1. [Start the AWS CloudHSM client](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-start-cloudhsm-client)\.

   1. [Update the cloudhsm\_mgmt\_util configuration file](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-update-configuration)\.

   1. Use cloudhsm\_mgmt\_util to create a CU\. For more information, see [Managing HSM Users](manage-hsm-users.md)\. Keep track of the CU user name and password\. You will need them later when you generate or import the HTTPS private key and certificate for your web server\.

After you complete these steps, go to [Step 2: Generate or Import a Private Key and SSL/TLS Certificate](ssl-offload-import-or-generate-private-key-and-certificate.md)\.