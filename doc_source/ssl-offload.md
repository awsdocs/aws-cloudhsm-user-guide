# Improve Your Web Server's Security with SSL/TLS Offload with AWS CloudHSM<a name="ssl-offload"></a>

Web servers and their clients \(web browsers\) can use Secure Sockets Layer \(SSL\) or Transport Layer Security \(TLS\)\. These protocols confirm the identity of the web server and establish a secure connection to send and receive data over the internet\. This is known as HTTPS\. The web server uses a public–private key pair and a public key certificate to establish an HTTPS session with each client\. This process involves a lot of computation for the web server, but you can offload some of this computation to the HSMs in your AWS CloudHSM cluster\. This is sometimes known as SSL acceleration\. This offloading reduces the burden on your web server and provides extra security by storing the server's private key in the HSMs\.

In this solution, you use either the [Apache HTTP Server](https://httpd.apache.org/) or [Nginx](https://nginx.org/en/) web server software, running on Amazon Linux\. Both of these web server programs natively integrate with [OpenSSL](https://www.openssl.org/) to support HTTPS using SSL or TLS\. The AWS CloudHSM software library for OpenSSL provides an interface that enables these web servers to use the HSMs in your cluster for cryptographic processing and key storage\. The AWS CloudHSM software library for OpenSSL is the bridge that connects the web server software with your AWS CloudHSM cluster\.

Complete the following steps to accomplish SSL/TLS offload with the Apache or Nginx web servers\.

**To configure SSL/TLS offload with Apache or Nginx**

1. Follow the steps in [Set Up the Prerequisites](ssl-offload-prerequisites.md) to prepare your environment\.

1. Follow the steps in [Import or Generate a Private Key and Certificate](ssl-offload-import-or-generate-private-key-and-certificate.md) to import or generate a private key and certificate\.

1. Follow the steps in [Configure the Web Server](ssl-offload-configure-web-server.md) to configure the Apache or Nginx web server and verify that SSL/TLS offload is working\.

After you complete these steps, you have a web server running the Apache or Nginx web server software\. Your server has a certificate and a corresponding private key which is stored in the HSMs in your AWS CloudHSM cluster\.

**SSL/TLS offload illustration**

To establish an HTTPS connection, your server performs a handshake process with clients\. As part of this process, the server offloads some of the cryptographic processing to the HSMs, as shown in the following figure\. Each step of the process is explained below the figure\.

**Note**  
The following image and process assumes that RSA is used for server verification and key exchange\. The process is slightly different when Diffie–Hellman is used instead of RSA\.

![\[Drawing that illustrates the TLS handshake process between a client and server
        including cryptographic offload to an HSM.\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/ssl-offload-handshake-process.png)

1. The client sends a hello message to the server\.

1. The server responds with a hello message and sends the server's certificate\.

1. The client performs the following actions:

   1. Verifies that the server's certificate is signed by one of the root certificates that the client trusts\.

   1. Extracts the public key from the server's certificate\.

   1. Generates a premaster secret and encrypts it with the server's public key\.

   1. Sends the encrypted premaster secret to the server\.

1. To decrypt the client's premaster secret, the server sends it to the HSM\. The private key in the HSM is used to decrypt the premaster secret, which is then returned to the server\.

   Independently, the client and server each use the premaster secret and some information from the hello messages to calculate a master secret\.

1. The handshake process ends\. For the rest of the session, all messages sent between the client and the server are encrypted with derivatives of the master secret\.