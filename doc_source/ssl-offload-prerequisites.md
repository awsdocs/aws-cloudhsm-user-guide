# Step 1: Set up the prerequisites<a name="ssl-offload-prerequisites"></a>

Different platforms require different prerequisites\. Use the prerequisites section below that matches your platform\.

**Topics**
+ [Prerequisites for Client SDK 5](#new-versions)
+ [Prerequisites for Client SDK 3](#old-versions)

## Prerequisites for Client SDK 5<a name="new-versions"></a>

### <a name="openssl-v111"></a>

To set up web server SSL/TLS offload with Client SDK 5, you need the following:
+ An active AWS CloudHSM cluster with at least two hardware security modules \(HSM\)
**Note**  
You can use a single HSM cluster, but you must first disable client key durability\. For more information, see [Manage Client Key Durability Settings](manage-key-sync.md#client-sync-sdk8) and [Client SDK 5 Configure Tool](configure-sdk-5.md)\.
+ An Amazon EC2 instance running a Linux operating system with the following software installed:
  + A web server \(either NGINX or Apache\)
  + The OpenSSL Dynamic Engine for Client SDK 5
+ A [crypto user](manage-hsm-users-chsm-cli.md#crypto-user-chsm-cli) \(CU\) to own and manage the web server's private key on the HSM\.

**To set up a Linux web server instance and create a CU on the HSM**

1. Install and configure the OpenSSL Dynamic Engine for AWS CloudHSM\. For more information about installing OpenSSL Dynamic Engine, see [OpenSSL Dynamic Engine for Client SDK 5](openssl5-install.md)\.

1. On an EC2 Linux instance that has access to your cluster, install either NGINX or Apache web server:

------
#### [ Amazon Linux ]
   + NGINX

     ```
     $ sudo yum install nginx
     ```
   + Apache

     ```
     $ sudo yum install httpd24 mod24_ssl
     ```

------
#### [ Amazon Linux 2 ]
   + For information on how to download the latest version of NGINX on Amazon Linux 2, see the [NGINX website](https://nginx.org/en/linux_packages.html)\.

     The latest version of NGINX available for Amazon Linux 2 uses a version of OpenSSL that is newer than the system version of OpenSSL\. After installing NGINX, you need to create a symbolic link from the AWS CloudHSM OpenSSL Dynamic Engine library to the location that this version of OpenSSL expects 

     ```
     $ sudo ln -sf /opt/cloudhsm/lib/libcloudhsm_openssl_engine.so /usr/lib64/engines-1.1/cloudhsm.so
     ```
   + Apache

     ```
     $ sudo yum install httpd mod_ssl
     ```

------
#### [ CentOS 7 ]
   + For information on how to download the latest version of NGINX on CentOS 7, see the [NGINX website](https://nginx.org/en/linux_packages.html)\.

     The latest version of NGINX available for CentOS 7 uses a version of OpenSSL that is newer than the system version of OpenSSL\. After installing NGINX, you need to create a symbolic link from the AWS CloudHSM OpenSSL Dynamic Engine library to the location that this version of OpenSSL expects 

     ```
     $ sudo ln -sf /opt/cloudhsm/lib/libcloudhsm_openssl_engine.so /usr/lib64/engines-1.1/cloudhsm.so
     ```
   + Apache

     ```
     $ sudo yum install httpd mod_ssl
     ```

------
#### [ Red Hat 7 ]
   + For information on how to download the latest version of NGINX on Red Hat 7, see the [NGINX website](https://nginx.org/en/linux_packages.html)\.

     The latest version of NGINX available for Red Hat 7 uses a version of OpenSSL that is newer than the system version of OpenSSL\. After installing NGINX, you need to create a symbolic link from the AWS CloudHSM OpenSSL Dynamic Engine library to the location that this version of OpenSSL expects 

     ```
     $ sudo ln -sf /opt/cloudhsm/lib/libcloudhsm_openssl_engine.so /usr/lib64/engines-1.1/cloudhsm.so
     ```
   + Apache

     ```
     $ sudo yum install httpd mod_ssl
     ```

------
#### [ CentOS 8 ]
   + NGINX

     ```
     $ sudo yum install nginx
     ```
   + Apache

     ```
     $ sudo yum install httpd mod_ssl
     ```

------
#### [ Red Hat 8 ]
   + NGINX

     ```
     $ sudo yum install nginx
     ```
   + Apache

     ```
     $ sudo yum install httpd mod_ssl
     ```

------
#### [ Ubuntu 18\.04 ]
   + NGINX

     ```
     $ sudo apt install nginx
     ```
   + Apache

     ```
     $ sudo apt install apache2
     ```

------
#### [ Ubuntu 20\.04 ]
   + NGINX

     ```
     $ sudo apt install nginx
     ```
   + Apache

     ```
     $ sudo apt install apache2
     ```

------

1. Use CloudHSM CLI to create a CU\. For more information about managing HSM users, see [Managing HSM users with CloudHSM CLI](manage-chsm-cli-users.md)\.
**Tip**  
Keep track of the CU user name and password\. You will need them later when you generate or import the HTTPS private key and certificate for your web server\.

After you complete these steps, go to [Step 2: Generate or import a private key and SSL/TLS certificate](ssl-offload-import-or-generate-private-key-and-certificate.md)\.

#### Notes<a name="note-ssl5-pre"></a>
+ To use Security\-Enhanced Linux \(SELinux\) and web servers, you must allow outbound TCP connections on port 2223, which is the port Client SDK 5 uses to communicate with the HSM\.
+ To create and activate a cluster and give an EC2 instance access to the cluster, complete the steps in [Getting Started with AWS CloudHSM](getting-started.md)\. The getting started offers step\-by\-step instruction for creating an active cluster with one HSM and an Amazon EC2 client instance\. You can use this client instance as your web server\. 
+ To avoid disabling client key durability, add more than one HSM to your cluster\. For more information, see [Adding an HSM](add-remove-hsm.md#add-hsm)\.
+ To connect to your client instance, you can use SSH or PuTTY\. For more information, see [Connecting to Your Linux Instance Using SSH](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstancesLinux.html) or [Connecting to Your Linux Instance from Windows Using PuTTY](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/putty.html) in the Amazon EC2 documentation\. 

## Prerequisites for Client SDK 3<a name="old-versions"></a>

### <a name="openssl-v102"></a>

To set up web server SSL/TLS offload with Client SDK 3, you need the following:
+ An active AWS CloudHSM cluster with at least one HSM\.
+ An Amazon EC2 instance running a Linux operating system with the following software installed:
  + The AWS CloudHSM client and command line tools\.
  + The NGINX or Apache web server application\.
  + The AWS CloudHSM dynamic engine for OpenSSL\.
+ A [crypto user](manage-hsm-users-chsm-cli.md#crypto-user-chsm-cli) \(CU\) to own and manage the web server's private key on the HSM\.

**To set up a Linux web server instance and create a CU on the HSM**

1. Complete the steps in [Getting started](getting-started.md)\. You will then have an active cluster with one HSM and an Amazon EC2 client instance\. Your EC2 instance will be configured with the command line tools\. Use this client instance as your web server\. 

1. Connect to your client instance\. For more information, see [Connecting to Your Linux Instance Using SSH](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstancesLinux.html) or [Connecting to Your Linux Instance from Windows Using PuTTY](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/putty.html) in the Amazon EC2 documentation\.

1. On an EC2 Linux instance that has access to your cluster, install either NGINX or Apache web server:

------
#### [ Amazon Linux ]
   + NGINX

     ```
     $ sudo yum install nginx
     ```
   + Apache

     ```
     $ sudo yum install httpd24 mod24_ssl
     ```

------
#### [ Amazon Linux 2 ]
   + NGINX version 1\.19 is the latest version of NGINX compatible with the Client SDK 3 engine on Amazon Linux 2\.

     For more information and to download NGINX version 1\.19, see the [NGINX website](https://nginx.org/)\.
   + Apache

     ```
     $ sudo yum install httpd mod_ssl
     ```

------
#### [ CentOS 7 ]
   + NGINX version 1\.19 is the latest version of NGINX compatible with the Client SDK 3 engine on CentOS 7\.

     For more information and to download NGINX version 1\.19, see the [NGINX website](https://nginx.org/)\.
   + Apache

     ```
     $ sudo yum install httpd mod_ssl
     ```

------
#### [ Red Hat 7 ]
   + NGINX version 1\.19 is the latest version of NGINX compatible with the Client SDK 3 engine on Red Hat 7\.

     For more information and to download NGINX version 1\.19, see the [NGINX website](https://nginx.org/)\.
   + Apache

     ```
     $ sudo yum install httpd mod_ssl
     ```

------
#### [ Ubuntu 16\.04 ]
   + NGINX

     ```
     $ sudo apt install nginx
     ```
   + Apache

     ```
     $ sudo apt install apache2
     ```

------

1. \(Optional\) Add more HSMs to your cluster\. For more information, see [Adding an HSM](add-remove-hsm.md#add-hsm)\.

1. Use cloudhsm\_mgmt\_util to create a CU\. For more information, see [Managing HSM users](manage-hsm-users.md)\. Keep track of the CU user name and password\. You will need them later when you generate or import the HTTPS private key and certificate for your web server\.

After you complete these steps, go to [Step 2: Generate or import a private key and SSL/TLS certificate](ssl-offload-import-or-generate-private-key-and-certificate.md)\.