# Step 2: Generate or import a private key and SSL/TLS certificate<a name="ssl-offload-import-or-generate-private-key-and-certificate"></a>

To enable HTTPS, your web server application \(NGINX or Apache\) needs a private key and a corresponding SSL/TLS certificate\. To use web server SSL/TLS offload with AWS CloudHSM, you must store the private key in an HSM in your AWS CloudHSM cluster\. You can accomplish this in one of the following ways: 
+ If you don't yet have a private key and a corresponding certificate, generate a private key in an HSM\. You use the private key to create a certificate signing request \(CSR\), which you use to create the SSL/TLS certificate\.
+ If you already have a private key and corresponding certificate, import the private key into an HSM\.

Regardless of which of the preceding methods you choose, you export a *fake PEM private key* from the HSM, which is a private key file in PEM format which contains a reference to the private key stored on the HSM \(it's not the actual private key\)\. Your web server uses the fake PEM private key file to identify the private key on the HSM during SSL/TLS offload\.

**Topics**
+ [Generate a private key and certificate](#ssl-offload-generate-private-key-and-certificate)
+ [Import an existing private key and certificate](#ssl-offload-import-private-key)

## Generate a private key and certificate<a name="ssl-offload-generate-private-key-and-certificate"></a>

### Generate a private key<a name="ssl-offload-generate-private-key"></a>

This section shows you how to generate a keypair using the [Key Management Utility \(KMU\)](key_mgmt_util.md) from Client SDK 3\. Once you have a key pair generated inside the HSM, you can export it as a fake PEM file, and generate the corresponding certificate\. 

Private keys generated with the Key Management Utility \(KMU\) can be used with both Client SDK 3 and Client SDK 5\.<a name="ssl-offload-generate-private-key-prerequisites"></a>

**Install and configure the Key Management Utility \(KMU\)**

1. Connect to your client instance\.

1. [Install and Configure](cmu-install-and-configure-client-linux.md) Client SDK 3\.

1. Run the following command to start the AWS CloudHSM client\.

------
#### [ Amazon Linux ]

   ```
   $ sudo start cloudhsm-client
   ```

------
#### [ Amazon Linux 2 ]

   ```
   $ sudo service cloudhsm-client start
   ```

------
#### [ CentOS 7 ]

   ```
   $ sudo service cloudhsm-client start
   ```

------
#### [ CentOS 8 ]

   ```
   $ sudo service cloudhsm-client start
   ```

------
#### [ RHEL 7 ]

   ```
   $ sudo service cloudhsm-client start
   ```

------
#### [ RHEL 8 ]

   ```
   $ sudo service cloudhsm-client start
   ```

------
#### [ Ubuntu 16\.04 LTS ]

   ```
   $ sudo service cloudhsm-client start
   ```

------
#### [ Ubuntu 18\.04 LTS ]

   ```
   $ sudo service cloudhsm-client start
   ```

------

1. Run the following command to start the key\_mgmt\_util command line tool\.

   ```
   $ /opt/cloudhsm/bin/key_mgmt_util
   ```

1. Run the following command to log in to the HSM\. Replace *<user name>* and *<password>* with the user name and password of the cryptographic user \(CU\)\.

   ```
   Command: loginHSM -u CU -s <user name> -p <password>>
   ```

**Generate a Private Key**

Depending on your use case, you can either generate an RSA or an EC key pair\. Do one of the following:
+ To generate an RSA private key on an HSM

  Use the `genRSAKeyPair` command to generate an RSA key pair\. This example generates an RSA key pair with a modulus of 2048, a public exponent of 65537, and a label of *tls\_rsa\_keypair*\.

  ```
  Command: genRSAKeyPair -m 2048 -e 65537 -l tls_rsa_keypair
  ```

  If the command was successful, you should see the following output indicating that you've successfully generated an RSA key pair\.

  ```
  Cfm3GenerateKeyPair returned: 0x00 : HSM Return: SUCCESS
  
                  Cfm3GenerateKeyPair:    public key handle: 7    private key handle: 8
  
                  Cluster Status:
                  Node id 1 status: 0x00000000 : HSM Return: SUCCESS
  ```
+ To generate an EC private key on an HSM

  Use the `genECCKeyPair` command to generate an EC key pair\. This example generates an EC key pair with a curve ID of 2 \(corresponding to the `NID_X9_62_prime256v1` curve\) and a label of *tls\_ec\_keypair*\.

  ```
  Command: genECCKeyPair -i 2 -l tls_ec_keypair
  ```

  If the command was successful, you should see the following output indicating that you've successfully generated an EC key pair\.

  ```
  Cfm3GenerateKeyPair returned: 0x00 : HSM Return: SUCCESS    
  
                  Cfm3GenerateKeyPair:    public key handle: 7    private key handle: 8
  
                  Cluster Status:
                  Node id 1 status: 0x00000000 : HSM Return: SUCCESS
  ```

**Export a fake PEM private key file**

Once you have a private key on the HSM, you must export a fake PEM private key file\. This file does not contain the actual key data, but it allows the OpenSSL Dynamic Engine to identify the private key on the HSM\. You can then you use the private key to create a certificate signing request \(CSR\) and sign the CSR to create the certificate\. 

**Note**  
Fake PEM files generated with the Key Management Utility \(KMU\) can be used with both Client SDK 3 and Client SDK 5\.

Identify the key handle that corresponds to the key that you would like to export as a fake PEM, then run the following command to export the private key in fake PEM format and save it to a file\. Replace the following values with your own\. 
+ *<private\_key\_handle>* – Handle of the generated private key\. This handle was generated by one of the key generation commands in the preceding step\. In the preceding example, the handle of the private key is 8\. 
+ *<web\_server\_fake\_PEM\.key>* – Name of the file that your fake PEM key will be written to\.

```
Command: getCaviumPrivKey -k <private_key_handle> -out <web_server_fake_PEM.key>
```

**Exit**

Run the following command to stop the key\_mgmt\_util\.

```
Command: exit
```

You should now have a new file on your system, located at the path specified by *<web\_server\_fake\_PEM\.key>* in the preceding command\. This file is the fake PEM private key file\.

### Generate a self\-signed certificate<a name="ssl-offload-generate-certificate"></a>

Once you have generated a fake PEM private key, you can use this file to generate a certificate signing request \(CSR\) and certificate\.

In a production environment, you typically use a certificate authority \(CA\) to create a certificate from a CSR\. A CA is not necessary for a test environment\. If you do use a CA, send the CSR file to them and use signed SSL/TLS certificate that they provide you in your web server for HTTPS\. 

As an alternative to using a CA, you can use the AWS CloudHSM OpenSSL Dynamic Engine to create a self\-signed certificate\. Self\-signed certificates are not trusted by browsers and should not be used in production environments\. They can be used in test environments\. 

**Warning**  
Self\-signed certificates should be used in a test environment only\. For a production environment, use a more secure method such as a certificate authority to create a certificate\. <a name="ssl-offload-generate-certificate-prerequisites"></a>

**Install and confiure the OpenSSL Dynamic Engine**

1. Connect to your client instance\.

1. To install and configure, do one of the following:
   + [Installing the OpenSSL Dynamic Engine](openssl5-install.md)
   + [Installing Client SDK 3 for OpenSSL Dynamic Engine](openssl3-install.md)<a name="ssl-offload-generate-certificate-steps"></a>

**Generate a certificate**

1. Obtain a copy of your fake PEM file generated in an earlier step\.

1. Create a CSR

   Run the following command to use the AWS CloudHSM OpenSSL Dynamic Engine to create a certificate signing request \(CSR\)\. Replace *<web\_server\_fake\_PEM\.key>* with the name of the file that contains your fake PEM private key\. Replace *<web\_server\.csr>* with the name of the file that contains your CSR\. 

   The `req` command is interactive\. Respond to each field\. The field information is copied into your SSL/TLS certificate\. 

   ```
   $ openssl req -engine cloudhsm -new -key <web_server_fake_PEM.key> -out <web_server.csr>
   ```

1. Create a self\-signed certificate

   Run the following command to use the AWS CloudHSM OpenSSL Dynamic Engine to sign your CSR with your private key on your HSM\. This creates a self\-signed certificate\. Replace the following values in the command with your own\. 
   + *<web\_server\.csr>* – Name of the file that contains the CSR\.
   + *<web\_server\_fake\_PEM\.key>* – Name of the file that contains the fake PEM private key\.
   + *<web\_server\.crt>* – Name of the file that will contain your web server certificate\.

   ```
   $ openssl x509 -engine cloudhsm -req -days 365 -in <web_server.csr> -signkey <web_server_fake_PEM.key> -out <web_server.crt>
   ```

After you complete these steps, go to [Step 3: Configure the web server](ssl-offload-configure-web-server.md)\.

## Import an existing private key and certificate<a name="ssl-offload-import-private-key"></a>

You might already have a private key and a corresponding SSL/TLS certificate that you use for HTTPS on your web server\. If so, you can import that key into an HSM by following the steps in this section\.

**Note**  
Some notes on private key imports and Client SDK compatibility:  
Importing an existing private key requires Client SDK 3\.
You can use private keys from Client SDK 3 with Client SDK 5\.
OpenSSL Dynamic Engine for Client SDK 3 does not support the latest Linux platforms, but the implementation of OpenSSL Dynamic Engine for Client SDK 5 does\. You can import an existing private key using the Key Management Utility \(KMU\) provided with Client SDK 3, then *use that private key* and the implementation of OpenSSL Dynamic Engine with Client SDK 5 to support SSL/TLS offload on the latest Linux platforms\.

**To import an existing private key into an HSM with Client SDK 3**

1. Connect to your Amazon EC2 client instance\. If necessary, copy your existing private key and certificate to the instance\. 

1. [Install and Configure](cmu-install-and-configure-client-linux.md) Client SDK 3

1. Run the following command to start the AWS CloudHSM client\.

------
#### [ Amazon Linux ]

   ```
   $ sudo start cloudhsm-client
   ```

------
#### [ Amazon Linux 2 ]

   ```
   $ sudo service cloudhsm-client start
   ```

------
#### [ CentOS 7 ]

   ```
   $ sudo service cloudhsm-client start
   ```

------
#### [ CentOS 8 ]

   ```
   $ sudo service cloudhsm-client start
   ```

------
#### [ RHEL 7 ]

   ```
   $ sudo service cloudhsm-client start
   ```

------
#### [ RHEL 8 ]

   ```
   $ sudo service cloudhsm-client start
   ```

------
#### [ Ubuntu 16\.04 LTS ]

   ```
   $ sudo service cloudhsm-client start
   ```

------
#### [ Ubuntu 18\.04 LTS ]

   ```
   $ sudo service cloudhsm-client start
   ```

------

1. Run the following command to start the key\_mgmt\_util command line tool\.

   ```
   $ /opt/cloudhsm/bin/key_mgmt_util
   ```

1. Run the following command to log in to the HSM\. Replace *<user name>* and *<password>* with the user name and password of the cryptographic user \(CU\)\. 

   ```
   Command: loginHSM -u CU -s <user name> -p <password>
   ```

1. Run the following commands to import your private key into an HSM\.

   1. Run the following command to create a symmetric wrapping key that is valid for the current session only\. The command and output are shown\. 

      ```
      Command: genSymKey -t 31 -s 16 -sess -l wrapping_key_for_import
      
      Cfm3GenerateSymmetricKey returned: 0x00 : HSM Return: SUCCESS
      Symmetric Key Created.  Key Handle: 6
      Cluster Error Status
      Node id 0 and err state 0x00000000 : HSM Return: SUCCESS
      ```

   1. Run the following command to import your existing private key into an HSM\. The command and output are shown\. Replace the following values with your own: 
      + *<web\_server\_existing\.key>* – Name of the file that contains your private key\.
      + *<web\_server\_imported\_key>* – Label for your imported private key\.
      + *<wrapping\_key\_handle>* – Wrapping key handle generated by the preceding command\. In the previous example, the wrapping key handle is 6\.

      ```
      Command: importPrivateKey -f <web_server_existing.key> -l <web_server_imported_key> -w <wrapping_key_handle>
      
      BER encoded key length is 1219
      Cfm3WrapHostKey returned: 0x00 : HSM Return: SUCCESS
      Cfm3CreateUnwrapTemplate returned: 0x00 : HSM Return: SUCCESS
      Cfm3UnWrapKey returned: 0x00 : HSM Return: SUCCESS
      Private Key Unwrapped.  Key Handle: 8
      Cluster Error Status
      Node id 0 and err state 0x00000000 : HSM Return: SUCCESS
      ```

1. Run the following command to export the private key in fake PEM format and save it to a file\. Replace the following values with your own\. 
   + *<private\_key\_handle>* – Handle of the imported private key\. This handle was generated by the second command in the preceding step\. In the preceding example, the handle of the private key is 8\. 
   + *<web\_server\_fake\_PEM\.key>* – Name of the file that contains your exported fake PEM private key\. 

   ```
   Command: getCaviumPrivKey -k <private_key_handle> -out <web_server_fake_PEM.key>
   ```

1. Run the following command to stop key\_mgmt\_util\.

   ```
   Command: exit
   ```

After you complete these steps, go to [Step 3: Configure the web server](ssl-offload-configure-web-server.md)\.