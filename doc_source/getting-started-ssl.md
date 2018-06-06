# Reconfigure SSL with a New Certificate and Private Key \(Optional\)<a name="getting-started-ssl"></a>

AWS CloudHSM uses an SSL certificate to establish a connection to an HSM\. A default key and SSL certificate are included when you install the client\. You can, however, create and use your own\. Note that you will need the self–signed certificate \(*`customerCA.crt`*\) that you created when you [initialized](initialize-cluster.md#sign-csr-create-cert) your cluster\. 

**To reconfigure SSL with a new certificate and private key**

1. Create a private key using the following OpenSSL command:

   ```
   genrsa -aes256 -out ssl-client.key 2048
   Generating RSA private key, 2048 bit long modulus
   ........+++
   ............+++
   e is 65537 (0x10001)
   Enter pass phrase for customerCA.key:
   Verifying - Enter pass phrase for customerCA.key:
   ```

1. Use the following OpenSSL command to create a certificate signing request \(CSR\)\. You will be asked a series of questions for your certificate\. 

   ```
   req -new -sha256 -key ssl-client.key -out ssl-client.csr
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
   x509 -req -days 3652 -in ssl-client.csr \
           -CA customerCA.crt \
           -CAkey customerCA.key \
           -CAcreateserial \
           -out ssl-client.crt
   Signature ok
   subject=/C=US/ST=WA/L=Seattle/O=Example Company/OU=sales
   Getting CA Private Key
   ```

1. Copy your key and certificate to the appropriate directory\. In Linux, use the following commands\. The `configure --ssl` option became available with version 1\.0\.14 of the AWS CloudHSM client\. 

   ```
   sudo cp ssl-client.crt /opt/cloudhsm/etc/
   sudo cp ssl-client.key /opt/cloudhsm/etc/
   sudo /opt/cloudhsm/bin/configure --ssl --pkey /opt/cloudhsm/etc/ssl-client.key --cert /opt/cloudhsm/etc/ssl-client.crt
   ```

1. Add the `customerCA.crt` certificate to the trust store\. Create a hash of the certificate subject name\. This creates an index to allow the certificate to be looked up by that name\. Create a file that contains the certificate with the hash name\. 

   ```
   x509 -in /opt/cloudhsm/etc/customerCA.crt -hash | head -n 1
   1234abcd
   sudo cp /opt/cloudhsm/etc/customerCA.crt /opt/cloudhsm/etc/certs/1234abcd.0
   ```