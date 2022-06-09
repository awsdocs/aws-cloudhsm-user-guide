# Improve your web server security with SSL/TLS offload in AWS CloudHSM<a name="ssl-offload"></a>

Web servers and their clients \(web browsers\) can use Secure Sockets Layer \(SSL\) or Transport Layer Security \(TLS\)\. These protocols confirm the identity of the web server and establish a secure connection to send and receive webpages or other data over the internet\. This is commonly known as HTTPS\. The web server uses a public–private key pair and an SSL/TLS public key certificate to establish an HTTPS session with each client\. This process involves a lot of computation for the web server, but you can offload some of this to the HSMs in your AWS CloudHSM cluster\. This is sometimes known as SSL acceleration\. Offloading reduces the computational burden on your web server and provides extra security by storing the server's private key in the HSMs\.

The following topics provide an overview of how SSL/TLS offload with AWS CloudHSM works and tutorials for setting up SSL/TLS offload with AWS CloudHSM on the following platforms:
+ **Linux** – Using the [NGINX](https://nginx.org/en/) or [Apache HTTP Server](https://httpd.apache.org/) web server software
+ **Windows** – Using the [Internet Information Services \(IIS\) for Windows Server](https://www.iis.net/) web server software

**Topics**
+ [How SSL/TLS offload with AWS CloudHSM works](ssl-offload-overview.md)
+ [Tutorial: Using SSL/TLS offload with AWS CloudHSM on Linux](ssl-offload-linux.md)
+ [Tutorial: Using SSL/TLS offload with AWS CloudHSM on Windows](ssl-offload-windows.md)