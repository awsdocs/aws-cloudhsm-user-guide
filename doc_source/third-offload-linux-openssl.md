# Using NGINX or Apache with AWS CloudHSM for SSL/TLS offload on Linux<a name="third-offload-linux-openssl"></a>

This tutorial provides step\-by\-step instructions for setting up SSL/TLS offload with AWS CloudHSM on a Linux web server\.

**Topics**
+ [Overview](#ssl-offload-linux-openssl-overview)
+ [Step 1: Set up the prerequisites](ssl-offload-prerequisites.md)
+ [Step 2: Generate or import a private key and SSL/TLS certificate](ssl-offload-import-or-generate-private-key-and-certificate.md)
+ [Step 3: Configure the web server](ssl-offload-configure-web-server.md)
+ [Step 4: Enable HTTPS traffic and verify the certificate](ssl-offload-enable-traffic-and-verify-certificate.md)

## Overview<a name="ssl-offload-linux-openssl-overview"></a>

On Linux, the [NGINX](https://nginx.org/en/) and [Apache HTTP Server](https://httpd.apache.org/) web server software integrate with [OpenSSL](https://www.openssl.org/) to support HTTPS\. The [AWS CloudHSM dynamic engine for OpenSSL](openssl-library.md) provides an interface that enables the web server software to use the HSMs in your cluster for cryptographic offloading and key storage\. The OpenSSL engine is the bridge that connects the web server to your AWS CloudHSM cluster\.

To complete this tutorial, you must first choose whether to use the NGINX or Apache web server software on Linux\. Then the tutorial shows you how to do the following:
+ Install the web server software on an Amazon EC2 instance\.
+ Configure the web server software to support HTTPS with a private key stored in your AWS CloudHSM cluster\.
+ \(Optional\) Use Amazon EC2 to create a second web server instance and Elastic Load Balancing to create a load balancer\. Using a load balancer can increase performance by distributing the load across multiple servers\. It can also provide redundancy and higher availability if one or more servers fail\.

When you're ready to get started, go to [Step 1: Set up the prerequisites](ssl-offload-prerequisites.md)\.