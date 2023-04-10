# Tutorial: Using OpenSSL Dynamic Engine with AWS CloudHSM for SSL/TLS offload on Windows<a name="ssl-offload-windows"></a>

This tutorial provides step\-by\-step instructions for setting up SSL/TLS offload with AWS CloudHSM on a Windows web server\.

**Topics**
+ [Overview](#ssl-offload-windows-overview)
+ [Step 1: Set up the prerequisites](ssl-offload-prerequisites-windows.md)
+ [Step 2: Create a certificate signing request \(CSR\) and certificate](ssl-offload-windows-create-csr-and-certificate.md)
+ [Step 3: Configure the web server](ssl-offload-configure-web-server-windows.md)
+ [Step 4: Enable HTTPS traffic and verify the certificate](ssl-offload-enable-traffic-and-verify-certificate-windows.md)
+ [\(Optional\) Step 5: Add a load balancer with Elastic Load Balancing](ssl-offload-add-load-balancing-windows.md)

## Overview<a name="ssl-offload-windows-overview"></a>

On Windows, the [Internet Information Services \(IIS\) for Windows Server](https://www.iis.net/) web server application natively supports HTTPS\. The [AWS CloudHSM key storage provider \(KSP\) for Microsoft's Cryptography API: Next Generation \(CNG\)](ksp-library.md) provides the interface that allows IIS to use the HSMs in your cluster for cryptographic offloading and key storage\. The AWS CloudHSM KSP is the bridge that connects IIS to your AWS CloudHSM cluster\.

This tutorial shows you how to do the following:
+ Install the web server software on an Amazon EC2 instance\.
+ Configure the web server software to support HTTPS with a private key stored in your AWS CloudHSM cluster\.
+ \(Optional\) Use Amazon EC2 to create a second web server instance and Elastic Load Balancing to create a load balancer\. Using a load balancer can increase performance by distributing the load across multiple servers\. It can also provide redundancy and higher availability if one or more servers fail\.

When you're ready to get started, go to [Step 1: Set up the prerequisites](ssl-offload-prerequisites-windows.md)\.