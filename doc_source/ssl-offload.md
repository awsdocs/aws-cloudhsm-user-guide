# Improve Your Web Server's Security with SSL/TLS Offload in AWS CloudHSM<a name="ssl-offload"></a>

Web servers and their clients \(web browsers\) can use Secure Sockets Layer \(SSL\) or Transport Layer Security \(TLS\)\. These protocols confirm the identity of the web server and establish a secure connection to send and receive webpages or other data over the internet\. This is commonly known as HTTPS\. The web server uses a public–private key pair and a public key certificate to establish an HTTPS session with each client\. This process involves a lot of computation for the web server, but you can offload some of this computation to the HSMs in your AWS CloudHSM cluster\. This is sometimes known as SSL acceleration\. This offloading reduces the computational burden on your web server and provides extra security by storing the server's private key in the HSMs\.

The [Nginx](https://nginx.org/en/) and [Apache HTTP Server](https://httpd.apache.org/) web server applications natively integrate with [OpenSSL](https://www.openssl.org/) to support HTTPS\. The AWS CloudHSM software library for OpenSSL provides an interface that enables the web server to use the HSMs in your cluster for cryptographic offloading and key storage\. The AWS CloudHSM software library for OpenSSL is the bridge that connects the web server to your AWS CloudHSM cluster\.

In this tutorial, you choose whether to use the Nginx or Apache web server\. You install the web server on a Linux instance in Amazon EC2, and then optionally use Amazon EC2 to create a second web server and Elastic Load Balancing to create a load balancer\. Using a load balancer can increase performance when both web servers are healthy, because the load is distributed across multiple servers\. It can also provide redundancy and higher availability in case one web server fails\.

**To configure SSL/TLS offload for Nginx or Apache with AWS CloudHSM**

1. Follow the steps in [Step 1: Set Up the Prerequisites](ssl-offload-prerequisites.md) to prepare your environment\.

1. Follow the steps in [Step 2: Import or Generate a Private Key and Certificate](ssl-offload-import-or-generate-private-key-and-certificate.md) to import or generate a private key and certificate\.

1. Follow the steps in [Step 3: Configure the Web Server](ssl-offload-configure-web-server.md) to configure the Nginx or Apache web server and verify that SSL/TLS offload is working\.

1. \(Optional\) Follow the steps in [Step 4: Add a Load Balancer](ssl-offload-add-load-balancing.md) to create a second web server and a load balancer that distributes HTTPS requests to your web servers\.

## How SSL/TLS Offload with AWS CloudHSM Works<a name="ssl-offload-how-it-works"></a>

To establish an HTTPS connection, your web server performs a handshake process with clients\. As part of this process, the server offloads some of the cryptographic processing to the HSMs, as shown in the following figure\. Each step of the process is explained below the figure\.

**Note**  
The following image and process assumes that RSA is used for server verification and key exchange\. The process is slightly different when Diffie–Hellman is used instead of RSA\.

![\[An illustration of the TLS handshake process between a client and server
          including cryptographic offload to an HSM.\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/ssl-offload-handshake-process.png)

1. The client sends a hello message to the server\.

1. The server responds with a hello message and sends the server's certificate\.

1. The client performs the following actions:

   1. Verifies that the server's certificate is signed by one of the root certificates that the client trusts\.

   1. Extracts the public key from the server's certificate\.

   1. Generates a premaster secret and encrypts it with the server's public key\.

   1. Sends the encrypted premaster secret to the server\.

1. To decrypt the client's premaster secret, the server sends it to the HSM\. The HSM uses the private key in the HSM to decrypt the premaster secret, and then it sends the premaster secret to the server\.

   Independently, the client and server each use the premaster secret and some information from the hello messages to calculate a master secret\.

1. The handshake process ends\. For the rest of the session, all messages sent between the client and the server are encrypted with derivatives of the master secret\.