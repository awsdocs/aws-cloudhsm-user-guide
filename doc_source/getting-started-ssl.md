# Reconfigure SSL with a new certificate and private key \(optional\)<a name="getting-started-ssl"></a>

AWS CloudHSM uses an SSL certificate to establish a connection to an HSM\. A default key and SSL certificate are included when you install the client\. You can, however, create and use your own\. Note that you will need the self–signed certificate \(*`customerCA.crt`*\) that you created when you [initialized](initialize-cluster.md#sign-csr-create-cert) your cluster\. 

At a high level, this is a two\-step process: 

1. First, you create a private key, then use that key to create a certificate signing request \(CSR\)\. Use the issuing certificate, the certificate you created when you initialized the cluster, to sign the CSR\. 

1. Next, you use the configure tool to copy the key and certificate to the appropriate directories\.

## Create a key, a CSR, and then sign the CSR<a name="ssl-openssl"></a>

The steps are the same for Client SDK 3 or Client SDK 5\.

**To reconfigure SSL with a new certificate and private key**

1. Create a private key using the following OpenSSL command:

   ```
   openssl genrsa -out ssl-client.key 2048
   Generating RSA private key, 2048 bit long modulus
   ........+++
   ............+++
   e is 65537 (0x10001)
   ```

1. Use the following OpenSSL command to create a certificate signing request \(CSR\)\. You will be asked a series of questions for your certificate\. 

   ```
   openssl req -new -sha256 -key ssl-client.key -out ssl-client.csr
   Enter pass phrase for ssl-client.key:
   You are about to be asked to enter information that will be incorporated
   into your certificate request.
   What you are about to enter is what is called a Distinguished Name or a DN.
   There are quite a few fields but you can leave some blank
   For some fields there will be a default value,
   If you enter '.', the field will be left blank.
   -----
   Country Name (2 letter code) [XX]:
   State or Province Name (full name) []:
   Locality Name (eg, city) [Default City]:
   Organization Name (eg, company) [Default Company Ltd]:
   Organizational Unit Name (eg, section) []:
   Common Name (eg, your name or your server's hostname) []:
   Email Address []:
    
   Please enter the following 'extra' attributes
   to be sent with your certificate request
   A challenge password []:
   An optional company name []:
   ```

1. Sign the CSR with the *`customerCA.crt`* certificate that you created when you initialized your cluster\. 

   ```
   openssl x509 -req -days 3652 -in ssl-client.csr \
           -CA customerCA.crt \
           -CAkey customerCA.key \
           -CAcreateserial \
           -out ssl-client.crt
   Signature ok
   subject=/C=US/ST=WA/L=Seattle/O=Example Company/OU=sales
   Getting CA Private Key
   ```

## Enable custom SSL for AWS CloudHSM<a name="ssl-sdk"></a>

The steps are different for Client SDK 3 or Client SDK 5\. For more information about working with the configure command line tool, see [Configure tool](configure-tool.md)\.

**Topics**
+ [Custom SSL for Client SDK 3](#enable-ssl-3)
+ [Custom SSL for Client SDK 5](#enable-ssl-5)

### Custom SSL for Client SDK 3<a name="enable-ssl-3"></a>

Use the configure tool for Client SDK 3 to enable custom SSL\. For more information about configure tool for Client SDK 3, see [Client SDK 3 configure tool](configure-sdk-3.md)\.

**To use a custom certificate and key for TLS client\-server mutual authentication with Client SDK 3 on Linux**

1. Copy your key and certificate to the appropriate directory\. 

   ```
   sudo cp ssl-client.crt /opt/cloudhsm/etc
   sudo cp ssl-client.key /opt/cloudhsm/etc
   ```

1.  Use the configure tool to specify `ssl-client.crt` and `ssl-client.key`\.

   ```
   sudo /opt/cloudhsm/bin/configure --ssl \
   	--pkey /opt/cloudhsm/etc/ssl-client.key \
   	--cert /opt/cloudhsm/etc/ssl-client.crt
   ```

1. Add the `customerCA.crt` certificate to the trust store\. Create a hash of the certificate subject name\. This creates an index to allow the certificate to be looked up by that name\. 

   ```
   openssl x509 -in /opt/cloudhsm/etc/customerCA.crt -hash | head -n 1
   1234abcd
   ```

   Create a directory\.

   ```
   mkdir /opt/cloudhsm/etc/certs
   ```

   Create a file that contains the certificate with the hash name\. 

   ```
   sudo cp /opt/cloudhsm/etc/customerCA.crt /opt/cloudhsm/etc/certs/1234abcd.0
   ```

### Custom SSL for Client SDK 5<a name="enable-ssl-5"></a>

Use any of the Client SDK 5 configure tools to enable custom SSL\. For more information about configure tool for Client SDK 5, see [Client SDK 5 configure tool](configure-sdk-5.md)\.

------
#### [ PKCS \#11 library ]

**To use a custom certificate and key for TLS client\-server mutual authentication with Client SDK 5 on Linux**

1. Copy your key and certificate to the appropriate directory\.

   ```
   $ sudo cp ssl-client.crt /opt/cloudhsm/etc
   sudo cp ssl-client.key /opt/cloudhsm/etc
   ```

1.  Use the configure tool to specify `ssl-client.crt` and `ssl-client.key`\.

   ```
   $ sudo /opt/cloudhsm/bin/configure-pkcs11 \
               --server-client-cert-file /opt/cloudhsm/etc/ssl-client.crt \
               --server-client-key-file /opt/cloudhsm/etc/ssl-client.key
   ```

**To use a custom certificate and key for TLS client\-server mutual authentication with Client SDK 5 on Windows**

1. Copy your key and certificate to the appropriate directory\.

   ```
   cp ssl-client.crt C:\ProgramData\Amazon\CloudHSM\ssl-client.crt
   cp ssl-client.key C:\ProgramData\Amazon\CloudHSM\ssl-client.key
   ```

1.  With a PowerShell interpreter, use the configure tool to specify `ssl-client.crt` and `ssl-client.key`\.

   ```
   & "C:\Program Files\Amazon\CloudHSM\bin\configure-pkcs11.exe" `
               --server-client-cert-file C:\ProgramData\Amazon\CloudHSM\ssl-client.crt `
               --server-client-key-file C:\ProgramData\Amazon\CloudHSM\ssl-client.key
   ```

------
#### [ OpenSSL Dynamic Engine ]

**To use a custom certificate and key for TLS client\-server mutual authentication with Client SDK 5 on Linux**

1. Copy your key and certificate to the appropriate directory\.

   ```
   $ sudo cp ssl-client.crt /opt/cloudhsm/etc
   sudo cp ssl-client.key /opt/cloudhsm/etc
   ```

1.  Use the configure tool to specify `ssl-client.crt` and `ssl-client.key`\.

   ```
   $ sudo /opt/cloudhsm/bin/configure-dyn \
               --server-client-cert-file /opt/cloudhsm/etc/ssl-client.crt \
               --server-client-key-file /opt/cloudhsm/etc/ssl-client.key
   ```

------
#### [ JCE provider ]

**To use a custom certificate and key for TLS client\-server mutual authentication with Client SDK 5 on Linux**

1. Copy your key and certificate to the appropriate directory\.

   ```
   $ sudo cp ssl-client.crt /opt/cloudhsm/etc
   sudo cp ssl-client.key /opt/cloudhsm/etc
   ```

1.  Use the configure tool to specify `ssl-client.crt` and `ssl-client.key`\.

   ```
   $ sudo /opt/cloudhsm/bin/configure-jce \
               --server-client-cert-file /opt/cloudhsm/etc/ssl-client.crt \
               --server-client-key-file /opt/cloudhsm/etc/ssl-client.key
   ```

**To use a custom certificate and key for TLS client\-server mutual authentication with Client SDK 5 on Windows**

1. Copy your key and certificate to the appropriate directory\.

   ```
   cp ssl-client.crt C:\ProgramData\Amazon\CloudHSM\ssl-client.crt
   cp ssl-client.key C:\ProgramData\Amazon\CloudHSM\ssl-client.key
   ```

1.  With a PowerShell interpreter, use the configure tool to specify `ssl-client.crt` and `ssl-client.key`\.

   ```
   & "C:\Program Files\Amazon\CloudHSM\bin\configure-jce.exe" `
               --server-client-cert-file C:\ProgramData\Amazon\CloudHSM\ssl-client.crt `
               --server-client-key-file C:\ProgramData\Amazon\CloudHSM\ssl-client.key
   ```

------