# Step 2: Generate or import a private key and SSL/TLS certificate<a name="third-offload-linux-jsse-gen"></a>

To enable HTTPS, your Tomcat web server application needs a private key and a corresponding SSL/TLS certificate\. To use web server SSL/TLS offload with AWS CloudHSM, you must store the private key in an HSM in your AWS CloudHSM cluster\. 

**Note**  
If you don't yet have a private key and a corresponding certificate, generate a private key in an HSM\. You use the private key to create a certificate signing request \(CSR\), which you use to create the SSL/TLS certificate\.

You create a local AWS CloudHSM KeyStore file that contains a reference to your private key on the HSM and the associated certificate\. Your web server uses the AWS CloudHSM KeyStore file to identify the private key on the HSM during SSL/TLS offload\.

**Topics**
+ [Generate a private key](#jsse-ssl-offload-generate-private-key)
+ [Generate a self\-signed certificate](#jsse-ssl-offload-generate-certificate)

## Generate a private key<a name="jsse-ssl-offload-generate-private-key"></a>

This section shows you how to generate a keypair using the KeyTool from JDK\. Once you have a key pair generated inside the HSM, you can export it as a KeyStore file, and generate the corresponding certificate\.

Depending on your use case, you can either generate an RSA or an EC key pair\. The following steps show how to generate an RSA key pair\.

**Use the `genkeypair` command in KeyTool to generate an RSA key pair**

1. After replacing the *<VARIABLES>* below with your specific data, use the following command to generate a keystore file named `jsse_keystore.keystore`, which will have a reference of your private key on the HSM\.

   ```
   $ keytool -genkeypair -alias <UNIQUE ALIAS FOR KEYS> -keyalg <KEY ALGORITHM> -keysize <KEY SIZE> -sigalg <SIGN ALGORITHM> \
           -keystore <PATH>/<JSSE KEYSTORE NAME>.keystore -storetype CLOUDHSM \
           -dname CERT_DOMAIN_NAME \
           -J-classpath '-J'$JAVA_LIB'/*:/opt/cloudhsm/java/*:./*' \
           -provider "com.amazonaws.cloudhsm.jce.provider.CloudHsmProvider" \
           -providerpath "$CLOUDHSM_JCE_LOCATION" \
           -keypass <KEY PASSWORD> -storepass <KEYSTORE PASSWORD>
   ```
   + ***<PATH>***: The path that you want to generate your keystore file\.
   + ***<UNIQUE ALIAS FOR KEYS>***: This is used to uniquely identify your key on the HSM\. This alias will be set as the LABEL attribute for the key\.
   + ***<KEY PASSWORD>***: We store reference to your key in the local keystore file, and this password protects that local reference\.
   + ***<KEYSTORE PASSWORD>***: This is the password for your local keystore file\.
   + ***<JSSE KEYSTORE NAME>***: Name of the Keystore file\.
   + ***<CERT DOMAIN NAME>***: X\.500 Distinguished name\.
   + ***<KEY ALGORITHM>***: Key algorithm to generate key pair \(For example, RSA and EC\)\.
   + ***<KEY SIZE>***: Key size to generate key pair \(for example, 2048, 3072, and 4096\)\.
   + ***<SIGN ALGORITHM>***: Key size to generate key pair \(for example, SHA1withRSA, SHA224withRSA, SHA256withRSA, SHA384withRSA, and SHA512withRSA\)\.

1. To confirm the command was successful, enter the following command and verify you have successfully generated an RSA key pair\.

   ```
   $ ls <PATH>/<JSSE KEYSTORE NAME>.keystore
   ```

## Generate a self\-signed certificate<a name="jsse-ssl-offload-generate-certificate"></a>

Once you have generated a private key along with the keystore file, you can use this file to generate a certificate signing request \(CSR\) and certificate\.

In a production environment, you typically use a certificate authority \(CA\) to create a certificate from a CSR\. A CA is not necessary for a test environment\. If you do use a CA, send the CSR file to them and use signed SSL/TLS certificate that they provide you in your web server for HTTPS\.

As an alternative to using a CA, you can use the KeyTool to create a self\-signed certificate\. Self\-signed certificates are not trusted by browsers and should not be used in production environments\. They can be used in test environments\.

**Warning**  
Self\-signed certificates should be used in a test environment only\. For a production environment, use a more secure method, such as a certificate authority to create a certificate\.

**Topics**<a name="jsse-ssl-procedure-offload-generate-certificate"></a>

**Generate a certificate**

1. Obtain a copy of your keystore file generated in an earlier step\.

1. Run the following command to use the KeyTool to create a certificate signing request \(CSR\)\.

   ```
   $ keytool -certreq -keyalg RSA -alias unique_alias_for_key -file certreq.csr \
           -keystore <JSSE KEYSTORE NAME>.keystore -storetype CLOUDHSM \
           -J-classpath '-J$JAVA_LIB/*:/opt/cloudhsm/java/*:./*' \
           -keypass <KEY PASSWORD> -storepass <KEYSTORE PASSWORD>
   ```
**Note**  
The output file of the certificate signing request is `certreq.csr`\.<a name="jsse-ssl-procedure-offload-sign-certificate"></a>

**Sign a certificate**
+ After replacing the *<VARIABLES>* below with your specific data, run the following command to sign your CSR with your private key on your HSM\. This creates a self\-signed certificate\.

  ```
  $ keytool -gencert -infile certreq.csr -outfile certificate.crt \
      -alias <UNIQUE ALIAS FOR KEYS> -keypass <KEY_PASSWORD> -storepass <KEYSTORE_PASSWORD> -sigalg SIG_ALG \
      -storetype CLOUDHSM -J-classpath '-J$JAVA_LIB/*:/opt/cloudhsm/java/*:./*' \
      -keystore jsse_keystore.keystore
  ```
**Note**  
`certificate.crt` is the signed certificate that uses the alias’s private key\.<a name="jsse-ssl-procedure-offload-import-certificate"></a>

**Import a certificate in Keystore**
+ After replacing the *<VARIABLES>* below with your specific data, run the following command to import a signed certificate as a trusted certificate\. This step will store certificate in the keystore entry identified by alias\.

  ```
  $ keytool -import -alias <UNIQUE ALIAS FOR KEYS> -keystore jsse_keystore.keystore \
      -file certificate.crt -storetype CLOUDHSM \
      -v -J-classpath '-J$JAVA_LIB/*:/opt/cloudhsm/java/*:./*' \
      -keypass <KEY PASSWORD> -storepass <KEYSTORE_PASSWORD>
  ```<a name="jsse-ssl-procedure-offload-convert-certificate"></a>

**Convert a certificate to a PEM**
+ Run following command to convert the signed certificate file \(\.crt\) to a PEM\. The PEM file will be used to send the request from the http client\.

  ```
  $ openssl x509 -inform der -in certificate.crt -out certificate.pem
  ```

After you complete these steps, go to [Step 3: Configure the web server](third-offload-linux-jsse-config.md)\.