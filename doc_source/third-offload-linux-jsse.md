# Using Tomcat with AWS CloudHSM for SSL/TLS offload on Linux<a name="third-offload-linux-jsse"></a>

This topic provides step\-by\-step instructions for setting up SSL/TLS offload using Java Secure Socket Extension \(JSSE\) with the AWS CloudHSM JCE SDK\.

**Topics**
+ [Overview](#third-offload-linux-jsse-overview)
+ [Step 1: Set up the prerequisites](third-offload-linux-jsse-prereqs.md)
+ [Step 2: Generate or import a private key and SSL/TLS certificate](third-offload-linux-jsse-gen.md)
+ [Step 3: Configure the Tomcat web server](third-offload-linux-jsse-config.md)
+ [Step 4: Enable HTTPS traffic and verify the certificate](third-offload-linux-jsse-verify.md)

## Overview<a name="third-offload-linux-jsse-overview"></a>

 In AWS CloudHSM, Tomcat web servers work on Linux to support HTTPS\. The AWS CloudHSM JCE SDK provides an interface that can be used with JSSE \(Java Secure Socket Extension\) to enable use of HSMs for such web servers\. AWS CloudHSM JCE is the bridge that connects JSSE to your AWS CloudHSM cluster\. JSSE is a Java API for Secure Sockets Layer \(SSL\) and Transport Layer Security \(TLS\) protocols\. 