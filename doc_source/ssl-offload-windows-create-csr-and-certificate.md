# Step 2: Create a Certificate Signing Request \(CSR\) and Certificate<a name="ssl-offload-windows-create-csr-and-certificate"></a>

To enable HTTPS, your web server needs an SSL/TLS certificate and a corresponding private key\. To use SSL/TLS offload with AWS CloudHSM, you store the private key in the HSM in your AWS CloudHSM cluster\. To do this, you use the AWS CloudHSM key storage provider \(KSP\) for Microsoft's Cryptography API: Next Generation \(CNG\) to create a certificate signing request \(CSR\)\. Then you give the CSR to a certificate authority \(CA\), which signs the CSR to produce a certificate\.

**Topics**
+ [Create a CSR](#ssl-offload-windows-create-csr)
+ [Get a Signed Certificate and Import It](#ssl-offload-windows-create-certificate)

## Create a CSR<a name="ssl-offload-windows-create-csr"></a>

Use the AWS CloudHSM KSP on your Windows Server to create a CSR\.

**To create a CSR**

1. If you haven't already done so, connect to your Windows server\. For more information, see [Connect to Your Instance](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/EC2_GetStarted.html#ec2-connect-to-instance-windows) in the *Amazon EC2 User Guide for Windows Instances*\.

1. [Start the AWS CloudHSM client](cloudhsm_mgmt_util-getting-started.md#cloudhsm_mgmt_util-start-cloudhsm-client)\.

1. On your Windows Server, use a text editor to create a certificate request file named `IISCertRequest.inf`\. The following shows the contents of an example `IISCertRequest.inf` file\. For more information about the sections, keys, and values that you can specify in the file, see [Microsoft's documentation](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/certreq_1#BKMK_New)\. Do not change the `ProviderName` value\.

   ```
   [Version]
   Signature = "$Windows NT$"
   [NewRequest]
   Subject = "CN=example.com,C=US,ST=Washington,L=Seattle,O=ExampleOrg,OU=WebServer"
   HashAlgorithm = SHA256
   KeyAlgorithm = RSA
   KeyLength = 2048
   ProviderName = "Cavium Key Storage Provider"
   KeyUsage = 0xf0
   MachineKeySet = True
   [EnhancedKeyUsageExtension]
   OID=1.3.6.1.5.5.7.3.1
   ```

1. Use the [Windows certreq command](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/certreq_1) to create a CSR from the `IISCertRequest.inf` file that you created in the previous step\. The following example saves the CSR to a file named `IISCertRequest.csr`\. If you used a different file name for your certificate request file, replace *IISCertRequest\.inf* with the appropriate file name\. You can optionally replace *IISCertRequest\.csr* with a different file name for your CSR file\.

   ```
   C:\>certreq -new IISCertRequest.inf IISCertRequest.csr
           SDK Version: 2.03
   
   CertReq: Request Created
   ```

   The `IISCertRequest.csr` file contains your CSR\. You need this CSR to get a signed certificate\.

## Get a Signed Certificate and Import It<a name="ssl-offload-windows-create-certificate"></a>

In a production environment, you typically use a certificate authority \(CA\) to create a certificate from a CSR\. A CA is not necessary for a test environment\. If you do use a CA, send the CSR file \(`IISCertRequest.csr`\) to it and use the CA to create a signed SSL/TLS certificate\.

As an alternative to using a CA, you can use a tool like [OpenSSL](https://www.openssl.org/) to create a self\-signed certificate\.

**Warning**  
Self\-signed certificates are not trusted by browsers and should not be used in production environments\. They can be used in test environments\.

The following procedures show how to create a self\-signed certificate and use it to sign your web server's CSR\.

**To create a self\-signed certificate**

1. Use the following OpenSSL command to create a private key\. You can optionally replace *SelfSignedCA\.key* with the file name to contain your private key\.

   ```
   openssl genrsa -aes256 -out SelfSignedCA.key 2048
   Generating RSA private key, 2048 bit long modulus
   ......................................................................+++
   .........................................+++
   e is 65537 (0x10001)
   Enter pass phrase for SelfSignedCA.key:
   Verifying - Enter pass phrase for SelfSignedCA.key:
   ```

1. Use the following OpenSSL command to create a self\-signed certificate using the private key that you created in the previous step\. This is an interactive command\. Read the on\-screen instructions and follow the prompts\. Replace *SelfSignedCA\.key* with the name of the file that contains your private key \(if different\)\. You can optionally replace *SelfSignedCA\.crt* with the file name to contain your self\-signed certificate\.

   ```
   openssl req -new -x509 -days 365 -key SelfSignedCA.key -out SelfSignedCA.crt
   Enter pass phrase for SelfSignedCA.key:
   You are about to be asked to enter information that will be incorporated
   into your certificate request.
   What you are about to enter is what is called a Distinguished Name or a DN.
   There are quite a few fields but you can leave some blank
   For some fields there will be a default value,
   If you enter '.', the field will be left blank.
   -----
   Country Name (2 letter code) [AU]:
   State or Province Name (full name) [Some-State]:
   Locality Name (eg, city) []:
   Organization Name (eg, company) [Internet Widgits Pty Ltd]:
   Organizational Unit Name (eg, section) []:
   Common Name (e.g. server FQDN or YOUR name) []:
   Email Address []:
   ```

**To use your self\-signed certificate to sign your web server's CSR**
+ Use the following OpenSSL command to use your private key and self\-signed certificate to sign the CSR\. Replace the following with the names of the files that contain the corresponding data \(if different\)\.
  + *IISCertRequest\.csr* – The name of the file that contains your web server's CSR
  + *SelfSignedCA\.crt* – The name of the file that contains your self\-signed certificate
  + *SelfSignedCA\.key* – The name of the file that contains your private key
  + *IISCert\.crt* – The name of the file to contain your web server's signed certificate

  ```
  openssl x509 -req -days 365 -in IISCertRequest.csr \
                              -CA SelfSignedCA.crt \
                              -CAkey SelfSignedCA.key \
                              -CAcreateserial \
                              -out IISCert.crt
  Signature ok
  subject=/ST=IIS-HSM/L=IIS-HSM/OU=IIS-HSM/O=IIS-HSM/CN=IIS-HSM/C=IIS-HSM
  Getting CA Private Key
  Enter pass phrase for SelfSignedCA.key:
  ```

After you complete the previous step, you have a signed certificate for your web server \(`IISCert.crt`\) and a self\-signed certificate \(`SelfSignedCA.crt`\)\. When you have these files, go to [Step 3: Configure the Web Server](ssl-offload-configure-web-server-windows.md)\.