# Web Server SSL/TLS Offload Step 2: Import or Generate a Private Key and Certificate<a name="ssl-offload-import-or-generate-private-key-and-certificate"></a>

To enable HTTPS, your web server application \(Nginx or Apache HTTP Server\) needs a private key and a corresponding public key certificate\. To use web server SSL/TLS offload with AWS CloudHSM, you must store the private key in the HSMs in your AWS CloudHSM cluster\. You can accomplish this in one of the following ways:

+ If you don't yet have a private key and corresponding certificate, you can generate a private key in the HSMs\. Then you can use the private key to create a certificate signing request \(CSR\), which is then signed to produce a certificate\.

   

+ If you already have a private key and corresponding certificate, you can import the private key into the HSMs\.

Regardless of which method you choose, you then export a *fake PEM private key* from the HSMs and save it to a file\. The file doesn't contain the actual private key\. It contains a reference to the private key that is stored on the HSMs\. Your web server application uses the fake PEM private key file, along with the AWS CloudHSM software library for OpenSSL, to offload SSL or TLS processing to the HSMs in your cluster\.

## Generate a Private Key and Certificate<a name="ssl-offload-generate-private-key-and-certificate"></a>

If you don't have a private key and a corresponding certificate to use for HTTPS on your web server, you can generate a private key on the HSMs\. Then you use the private key to create a certificate signing request \(CSR\), which is then signed to produce a certificate\.

**To generate a private key on the HSMs**

1. Connect to the client instance that you created previously\.

1. Run the following command to set an environment variable named `n3fips_password` that contains the user name and password of the crypto user \(CU\) that you created previously\. Replace *<CU user name>* with the CU's user name, and replace *<password>* with the CU's password\.

   ```
   $ export n3fips_password=<CU user name>:<password>
   ```

1. Run the following command to use the AWS CloudHSM software library for OpenSSL to generate a private key on the HSMs\. This command also exports the fake PEM private key and saves it in a file\. Replace *<web\_server\_fake\_PEM\.key>* with the preferred file name for your exported fake PEM private key\.

   ```
   $ openssl genrsa -engine cloudhsm -out <web_server_fake_PEM.key> 2048
   ```

**To create a CSR**

+ Run the following command to use the AWS CloudHSM software library for OpenSSL to create a CSR\. Replace *<web\_server\_fake\_PEM\.key>* with the name of the file that contains your fake PEM private key\. Replace *<web\_server\.csr>* with the preferred file name for your CSR\.

  This is an interactive command\. Respond to each instruction, providing information for each field of the CSR\. This same information appears in the certificate after it's signed\.

  ```
  $ openssl req -engine cloudhsm -new -key <web_server_fake_PEM.key> -out <web_server.csr>
  ```

To produce a certificate, this CSR must be signed\. To sign the CSR, you typically use a certificate authority \(CA\)\. You give the CSR file \(*<web\_server\.csr>*\) to a CA\. The CA signs the CSR to create a signed certificate, and then gives you the certificate\. Your web server application uses the signed certificate for HTTPS\.

As an alternative to using a CA, you can use the AWS CloudHSM software library for OpenSSL to create a self\-signed certificate\. When you use a self\-signed certificate for HTTPS, your web server's clients \(web browsers\) typically don't trust the web server\. You shouldn't use a self\-signed certificate in production, but this kind of certificate might be adequate for testing\.

**To create a self\-signed certificate**
**Important**  
The following example is a proof\-of\-concept demonstration only\. For production systems, use a more secure method \(such as a CA\) to sign the CSR\.

+ Run the following command to use the AWS CloudHSM software library for OpenSSL to sign your CSR with your private key on the HSM\. This creates a self\-signed certificate\. Replace the following values with your own:

  + *<web\_server\.csr>* – The name of the file that contains the CSR that you created previously\.

  + *<web\_server\_fake\_PEM\.key>* – The name of the file that contains your fake PEM private key\.

  + *<web\_server\.crt>* – The preferred file name for your web server certificate\.

  ```
  $ openssl x509 -engine cloudhsm -req -days 365 -in <web_server.csr> -signkey <web_server_fake_PEM.key> -out <web_server.crt>
  ```

After you complete these steps, you can configure your web server for SSL/TLS offload with AWS CloudHSM\.

## Import an Existing Private Key<a name="ssl-offload-import-private-key"></a>

If you already have a private key and a corresponding certificate that you use for HTTPS on your web server, you can import the private key into the HSMs\.

**To import a private key into the HSMs**

1. Connect to the client instance that you created previously\. If necessary, copy your existing private key and certificate to the instance\.

1. Run the following command to start the AWS CloudHSM client\.

   ```
   $ sudo start cloudhsm-client
   ```

1. Run the following command to start the command line tool known as key\_mgmt\_util\.

   ```
   $ /opt/cloudhsm/bin/key_mgmt_util
   ```

1. Run the following command to log in to the HSM\. Replace *<user name>* and *<password>* with the user name and password of the crypto user \(CU\) that you created previously\.

   ```
   Command:  loginHSM -u CU -s <user name> -p <password>
   ```

1. Run the following commands to import your private key into the HSM\.

   1. Run the following command to create a symmetric wrapping key that is valid only for the current session\.

      ```
      Command:  genSymKey -t 31 -s 16 -sess -l wrapping_key_for_import
      
      Cfm3GenerateSymmetricKey returned: 0x00 : HSM Return: SUCCESS
      
      Symmetric Key Created.  Key Handle: 6
      
      Cluster Error Status
      Node id 0 and err state 0x00000000 : HSM Return: SUCCESS
      ```

   1. Run the following command to import your existing private key into the HSMs\. Replace the following values with your own:

      + *<web\_server\_existing\.key<* – The name of the file that contains your private key\.

      + *<web\_server\_imported\_key>* – The preferred label for your imported private key\.

      + *<wrapping\_key\_handle>* – The wrapping key handle that was generated in the previous command\. In the previous example, the wrapping key handle is 6\.

      ```
      Command:  importPrivateKey -f <web_server_existing.key> -l <web_server_imported_key> -w <wrapping_key_handle>
      BER encoded key length is 1219
      
      Cfm3WrapHostKey returned: 0x00 : HSM Return: SUCCESS
      
      Cfm3CreateUnwrapTemplate returned: 0x00 : HSM Return: SUCCESS
      
      Cfm3UnWrapKey returned: 0x00 : HSM Return: SUCCESS
      
      Private Key Unwrapped.  Key Handle: 8
      
      Cluster Error Status
      Node id 0 and err state 0x00000000 : HSM Return: SUCCESS
      ```

1. Run the following command to export the private key in fake PEM format and save it to a file\. Replace the following values with your own:

   + *<private\_key\_handle>* – The handle of the imported private key\. This handle was generated by the second command in the preceding step\. In the preceding example, the handle of the private key is 8\.

   + *<web\_server\_fake\_PEM\.key>* – The preferred file name for your exported fake PEM private key\.

   ```
   Command:  getCaviumPrivKey -k <private_key_handle> -out <web_server_fake_PEM.key>
   ```

1. Run the following command to stop key\_mgmt\_util\.

   ```
   Command:  exit
   ```

After you complete these steps, go to [Step 3: Configure the Web Server](ssl-offload-configure-web-server.md)\.