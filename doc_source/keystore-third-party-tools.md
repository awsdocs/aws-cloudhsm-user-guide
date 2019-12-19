# Using AWS CloudHSM Key Store with Third\-Party Tools<a name="keystore-third-party-tools"></a>

AWS CloudHSM key store is a special\-purpose JCE key store that utilizes certificates associated with keys on your HSM through third\-party tools such as `keytool` and `jarsigner`\. AWS CloudHSM does not store certificates on the HSM, as certificates are public, non\-confidential data\. The AWS CloudHSM key store stores the certificates in a local file and maps the certificates to corresponding keys on your HSM\. 

When you use the AWS CloudHSM key store to generate new keys, no entries are generated in the local key store file – the keys are created on the HSM\. Similarly, when you use the AWS CloudHSM key store to search for keys, the search is passed on to the HSM\. When you store certificates in the AWS CloudHSM key store, the provider verifies that a key pair with the corresponding alias exists on the HSM, and then associates the certificate provided with the corresponding key pair\. 

**Topics**
+ [Prerequisites](#keystore-prerequisites)
+ [Using AWS CloudHSM Key Store with Keytool](#using_keystore_with_keytool)
+ [Using AWS CloudHSM Key Store with Jarsigner](#using_keystore_jarsigner)
+ [Known Issues](#known-issues-keytool-jarsigner)
+ [Registering Pre\-existing Keys with AWS CloudHSM Key Store](#register-pre-existing-keys-with-keystore)

## Prerequisites<a name="keystore-prerequisites"></a>

To use the AWS CloudHSM key store, you must first initialize and configure the AWS CloudHSM JCE SDK\. 

### Step 1: Install the JCE<a name="prereq-step-one"></a>

To install the JCE, including the AWS CloudHSM client prerequisites, follow the steps for [installing the Java library](java-library-install.md)\. 

### Step 2: Add HSM login credentials to environment variables<a name="prereq-step-two"></a>

Set up environment variables to contain your HSM login credentials\. 

```
export HSM_PARTITION=PARTITION_1
export HSM_USER=<HSM user name> 
export HSM_PASSWORD=<HSM password>
```

**Note**  
The CloudHSM JCE offers various login options\. To use the AWS CloudHSM key store with third\-party applications, you must use implicit login with environment variables\. If you want to use explicit login through application code, you must build your own application using the AWS CloudHSM key store\. For additional information, see the article on [Using AWS CloudHSM Key Store](alternative-keystore.md)\. 

### Step 3: Registering the JCE provider<a name="prereq-step-three"></a>

To register the JCE provider, in the Java CloudProvider configuration\. 

1. Open the java\.security configuration file in your Java installation, for editing\.

1. In the java\.security configuration file, add `com.cavium.provider.CaviumProvider` as the last provider\. For example, if there are nine providers in the java\.security file, add the following provider as the last provider in the section\. Adding the Cavium provider as a higher priority may negatively impact your system's performance\.

   `security.provider.10=com.cavium.provider.CaviumProvider`
**Note**  
Power users may be accustomed to specifying `-providerName`, `-providerclass`, and `-providerpath` command line options when using keytool, instead of updating the security configuration file\. If you attempt to specify command line options when generating keys with AWS CloudHSM key store, it will cause errors\. 

## Using AWS CloudHSM Key Store with Keytool<a name="using_keystore_with_keytool"></a>

 [ Keytool](https://docs.oracle.com/javase/8/docs/technotes/tools/unix/keytool.html) is a popular command line utility for common key and certificate tasks on Linux systems\. A complete tutorial on keytool is out of scope for AWS CloudHSM documentation\. This article explains the specific parameters you should use with various keytool functions when utilizing AWS CloudHSM as the root of trust through the AWS CloudHSM key store\.

When using keytool with the AWS CloudHSM key store, specify the following arguments to any keytool command:

```
-storetype CLOUDHSM \
		-J-classpath '-J/opt/cloudhsm/java/*' \
		-J-Djava.library.path=/opt/cloudhsm/lib
```

If you want to create a new key store file using AWS CloudHSM key store, see Using AWS CloudHSM Key Store\. To use an existing key store, specify its name \(including path\) using the –keystore argument to keytool\. If you specify a non\-existent key store file in a keytool command, the AWS CloudHSM key store creates a new key store file\.

### Create New Keys with Keytool<a name="create_key_keytool"></a>

You can use keytool to generate any type of key supported by AWS CloudHSM's JCE SDK\. See a full list of keys and lengths in the  Supported Keys article in the Java Library\.

**Important**  
A key generated through keytool is generated in software, and then imported into AWS CloudHSM as an extractable, persistent key\.

Instructions for creating non\-extractable keys directly on the HSM, and then using them with keytool or Jarsigner, are shown in the code sample in [Registering Pre\-existing Keys with AWS CloudHSM Key Store](#register-pre-existing-keys-with-keystore)\. We strongly recommend generating non\-exportable keys outside of keytool, and then importing corresponding certificates to the key store\. If you use extractable RSA or EC keys through keytool and jarsigner, the providers export keys from the AWS CloudHSM and then use the key locally for signing operations\.

If you have multiple client instances connected to your CloudHSM cluster, be aware that importing a certificate on one client instance’s key store won't automatically make the certificates available on other client instances\. To register the key and associated certificates on each client instance you need to run a Java application as described in [Generate a CSR using Keytool](#generate_csr_using_keytool)\. Alternatively, you can make the necessary changes on one client and copy the resulting key store file to every other client instance\.

**Example 1: **To generate a symmetric AES\-256 key with label, "my\_secret" and save it in a key store file named, "my\_keystore\.store", in the working directory\.

```
keytool -genseckey -alias my_secret -keyalg aes \
		-keysize 256 -keystore my_keystore.store \
		-storetype CloudHSM -J-classpath '-J/opt/cloudhsm/java/*' \
		-J-Djava.library.path=/opt/cloudhsm/lib/
```

**Example 2: **To generate an RSA 2048 key pair with label "my\_rsa\_key\_pair" and save it in a key store file named, "my\_keystore\.store" in the working directory\.

```
keytool -genkeypair -alias my_rsa_key_pair \
        -keyalg rsa -keysize 2048 \
        -sigalg sha512withrsa \
        -keystore my_keystore.store \
        -storetype CLOUDHSM \
        -J-classpath ‘-J/opt/cloudhsm/java/*’ \
        -J-Djava.library.path=/opt/cloudhsm/lib/
```

**Example 3: **To generate a p256 ED key with label "my\_ec\_key\_pair" and save it in a key store file named, "my\_keystore\.store" in the working directory\.

```
keytool -genkeypair -alias my_ec_key_pair \
        -keyalg ec -keysize 256 \
        -sigalg SHA512withECDSA \
        -keystore my_keystore.store \
        -storetype CLOUDHSM \
        -J-classpath ‘-J/opt/cloudhsm/java/*’ \
        -J-Djava.library.path=/opt/cloudhsm/lib/
```

You can find a list of supported signature algorithms in the Java library\.

### Delete a Key using Keytool<a name="delete_key_using_keytool"></a>

The AWS CloudHSM key store doesn't support deleting keys\. To delete key, you must use the `deleteKey` function of AWS CloudHSM's command line tool, [ deleteKey Use the deleteKey command in AWS CloudHSM key\_mgmt\_util to delete a key from the HSM\.  The deleteKey command in key\_mgmt\_util deletes a key from the HSM\. You can only delete one key at a time\. Deleting one key in a key pair has no effect on the other key in the pair\. Only the key owner can delete a key\. Users who share the key can use it in cryptographic operations, but not delete it\.  Before you run any key\_mgmt\_util command, you must [start key\_mgmt\_util](key_mgmt_util-getting-started.md#key_mgmt_util-start) and [log in](key_mgmt_util-getting-started.md#key_mgmt_util-log-in) to the HSM as a crypto user \(CU\)\.   Syntax  

```
deleteKey -h 

deleteKey -k
```   Examples  These examples show how to use deleteKey to delete keys from your HSMs\. 

**Example : Delete a Key**  
This command deletes the key with key handle `6`\. When the command succeeds, deleteKey returns success messages from each HSM in the cluster\.  

```
Command: deleteKey -k 6

        Cfm3DeleteKey returned: 0x00 : HSM Return: SUCCESS

        Cluster Error Status
        Node id 1 and err state 0x00000000 : HSM Return: SUCCESS
        Node id 2 and err state 0x00000000 : HSM Return: SUCCESS
``` 

**Example : Delete a Key \(Failure\)**  
When the command fails because no key has the specified key handle, deleteKey returns an invalid object handle error message\.  

```
Command: deleteKey -k 252126

        Cfm3FindKey returned: 0xa8 : HSM Error: Invalid object handle is passed to this operation

        Cluster Error Status
        Node id 1 and err state 0x000000a8 : HSM Error: Invalid object handle is passed to this operation
        Node id 2 and err state 0x000000a8 : HSM Error: Invalid object handle is passed to this operation
```
When the command fails because the current user is not the owner of the key, the command returns an access denied error\.  

```
Command:  deleteKey -k 262152

Cfm3DeleteKey returned: 0xc6 : HSM Error: Key Access is denied.
```   Parameters   

**\-h**  
Displays command line help for the command\.   
Required: Yes 

**\-k**  
Specifies the key handle of the key to delete\. To find the key handles of keys in the HSM, use [findKey](key_mgmt_util-findKey.md)\.  
Required: Yes    Related Topics    [findKey](key_mgmt_util-findKey.md)    ](key_mgmt_util-deleteKey.md)\.

### Generate a CSR using Keytool<a name="generate_csr_using_keytool"></a>

You receive the greatest flexibility in generating a certificate signing request \(CSR\) if you use the [AWS CloudHSM Dynamic Engine for OpenSSL](openssl-library.md)\. The following command uses keytool to generate a CSR for a key pair with the alias, `my-key-pair`\.

```
keytool -certreq -alias my_key_pair \
        -file my_csr.csr \
        -keystore my_keystore.store \
        -storetype CLOUDHSM \
        -J-classpath ‘-J/opt/cloudhsm/java/*’ \
        -J-Djava.library.path=/opt/cloudhsm/lib/
```

**Note**  
To use a key pair from keytool, that key pair must have an entry in the specified key store file\. If you want to use a key pair that was generated outside of keytool, you must import the key and certificate metadata into the key store\. For instructions on importing the keystore data see [Importing Intermediate and root certificates into AWS CloudHSM Key Store using Keytool](#import_cert_using_keytool)\.

### Using Keytool to import intermediate and root certificates into AWS CloudHSM Key Store<a name="import_cert_using_keytool"></a>

To import a CA certificate you must enable verification of a full certificate chain on a newly imported certificate\. The following command shows an example\. 

```
keytool -import -trustcacerts -alias rootCAcert \
        -file rootCAcert.cert -keystore my_keystore.store \
        -storetype CLOUDHSM \
        -J-classpath ‘-J/opt/cloudhsm/java/*’ \
        -J-Djava.library.path=/opt/cloudhsm/lib/
```

If you connect multiple client instances to your AWS CloudHSM cluster, importing a certificate on one client instance’s key store won't automatically make the certificate available on other client instances\. You must import the certificate on each client instance\.

### Using Keytool to Delete Certificates from AWS CloudHSM Key Store<a name="delete_cert_using_keytool"></a>

The following command shows an example of how to delete a certificate from a Java keytool key store\. 

```
keytool -delete -alias mydomain -keystore \
        -keystore my_keystore.store \
        -storetype CLOUDHSM \
        -J-classpath ‘-J/opt/cloudhsm/java/*’ \
        -J-Djava.library.path=/opt/cloudhsm/lib/
```

If you connect multiple client instances to your AWS CloudHSM cluster, deleting a certificate on one client instance’s key store won't automatically remove the certificate from other client instances\. You must delete the certificate on each client instance\.

### Importing a Working Certificate into AWS CloudHSM Key Store using Keytool<a name="import_working_cert_using_keytool"></a>

Once a certificate signing request \(CSR\) is signed, you can import it into the AWS CloudHSM key store and associate it with the appropriate key pair\. The following command provides an example\. 

```
keytool -importcert -noprompt -alias my_key_pair \
        -file my_certificate.crt \
        -keystore my_keystore.store
        -storetype CLOUDHSM \
        -J-classpath ‘-J/opt/cloudhsm/java/*’ \
        -J-Djava.library.path=/opt/cloudhsm/lib/
```

The alias should be a key pair with an associated certificate in the key store\. If the key is generated outside of keytool, or is generated on a different client instance, you must first import the key and certificate metadata into the key store\. For instructions on importing the certificate metadata, see the code sample in[Registering Pre\-existing Keys with AWS CloudHSM Key Store](#register-pre-existing-keys-with-keystore)\. 

The certificate chain must be verifiable\. If you can't verify the certificate, you might need to import the signing \(certificate authority\) certificate into the key store so the chain can be verified\.

### Exporting a certificate using Keytool<a name="export_cert_using_keytool"></a>

The following example generates a certificate in binary X\.509 format\. To export a human readable certificate, add `-rfc` to the `-exportcert` command\. 

```
keytool -exportcert -alias my_key_pair \
        -file my_exported_certificate.crt \
        -keystore my_keystore.store \
        -storetype CLOUDHSM \
        -J-classpath ‘-J/opt/cloudhsm/java/*’ \
        -J-Djava.library.path=/opt/cloudhsm/lib/
```

## Using AWS CloudHSM Key Store with Jarsigner<a name="using_keystore_jarsigner"></a>

Jarsigner is a popular command line utility for signing JAR files using a key securely stored on a HSM\. A complete tutorial on Jarsigner is out of scope for the AWS CloudHSM documentation\. This section explains the Jarsigner parameters you should use to sign and verify signatures with AWS CloudHSM as the root of trust through the AWS CloudHSM key store\. 

### Setting up keys and certificates<a name="jarsigner_set_up_certificates"></a>

Before you can sign JAR files with Jarsigner, make sure you have set up or completed the following steps: 

1. Follow the guidance in the [AWS CloudHSM Key store prerequisites ](#keystore-prerequisites)\.

1. Set up your signing keys and the associated certificates and certificate chain which should be stored in the AWS CloudHSM key store of the current server or client instance\. Create the keys on the AWS CloudHSM and then import associated metadata into your AWS CloudHSM key store\. Use the code sample in [Registering Pre\-existing Keys with AWS CloudHSM Key Store](#register-pre-existing-keys-with-keystore) to import metadata into the key store\. If you want to use keytool to set up the keys and certificates, see [Create New Keys with Keytool](#create_key_keytool)\. If you use multiple client instances to sign your JARs, create the key and import the certificate chain\. Then copy the resulting key store file to each client instance\. If you frequently generate new keys, you may find it easier to individually import certificates to each client instance\.

1. The entire certificate chain should be verifiable\. For the certificate chain to be verifiable, you may need to add the CA certificate and intermediate certificates to the AWS CloudHSM key store\. See the code snippet in [Sign a JAR file using AWS CloudHSM and Jarsigner](#jarsigner_sign_jar_using_hsm_jarsigner) for instruction on using Java code to verify the certificate chain\. If you prefer, you can use keytool to import certificates\. For instructions on using keytool, see [Using Keytool to import intermediate and root certificates into AWS CloudHSM Key Store](#import_cert_using_keytool)\. 

### Sign a JAR file using AWS CloudHSM and Jarsigner<a name="jarsigner_sign_jar_using_hsm_jarsigner"></a>

Use the following command to sign a JAR file: 

```
jarsigner -keystore my_keystore.store \
        -signedjar signthisclass_signed.jar \
        -sigalg sha512withrsa \
        -storetype CloudHSM \
        -J-classpath ‘-J/opt/cloudhsm/java/*:/usr/lib/jvm/java-1.8.0/lib/tools.jar' \
        -J-Djava.library.path=/opt/cloudhsm/lib \
        signthisclass.jar my_key_pair
```

Use the following command to verify a signed JAR: 

```
jarsigner -verify \
        -keystore my_keystore.store \
        -sigalg sha512withrsa \
        -storetype CloudHSM \
        -J-classpath ‘-J/opt/cloudhsm/java/*:/usr/lib/jvm/java-1.8.0/lib/tools.jar' \
        -J-Djava.library.path=/opt/cloudhsm/lib \
        signthisclass_signed.jar my_key_pair
```

## Known Issues<a name="known-issues-keytool-jarsigner"></a>

The following list provides the current list of known issues\. 
+ When generating keys using keytool, the first provider in provider configuration cannot be CaviumProvider\. 
+ When generating keys using keytool, the first \(supported\) provider in the security configuration file is used to generate the key\. This is generally a software provider\. The generated key is then given an alias and imported into the AWS CloudHSM HSM as a persistent \(token\) key during the key addition process\. 
+  When using keytool with AWS CloudHSM key store, do not specify `-providerName`, `-providerclass`, or `-providerpath` options on the command line\. Specify these options in the security provider file as described in the [Key store prerequisites](#keystore-prerequisites)\. 
+ When using non\-extractable EC keys through keytool and Jarsigner, the SunEC provider needs to be removed/disabled from the list of providers in the java\.security file\. If you use extractable EC keys through keytool and Jarsigner, the providers export key bits from the AWS CloudHSM HSM and use the key locally for signing operations\. We do not recommend you use exportable keys with keytool or Jarsigner\.

## Registering Pre\-existing Keys with AWS CloudHSM Key Store<a name="register-pre-existing-keys-with-keystore"></a>

For maximum security and flexibility in attributes and labeling, we recommend you generate your signing keys using [key\_mgmt\_util](manage-keys.md#generate-keys)\. You can also use a Java application to generate the key in AWS CloudHSM\.

The following section provides a code sample that demonstrates how to generate a new key pair on the HSM and register it using existing keys imported to the AWS CloudHSM key store\. The imported keys are available for use with third\-party tools such as keytool and Jarsigner\. 

To use a pre\-existing key, modify the code sample to look up a key by label instead of generating a new key\. Sample code for looking up a key by label is available in the [KeyUtilitiesRunner\.java sample](https://docs.aws.amazon.com/https://github.com/aws-samples/aws-cloudhsm-jce-examples/blob/master/src/main/java/com/amazonaws/cloudhsm/examples/KeyUtilitiesRunner.java) on GitHub\. 

**Important**  
Registering a key stored on AWS CloudHSM with a local key store does not export the key\. When the key is registered, the key store registers the key's alias \(or label\) and correlates locally store certificate objects with a key pair on the AWS CloudHSM\. As long as the key pair is created as non\-exportable, the key bits won't leave the HSM\. 

```
                      	
                      	
                      	//
 // Copyright 2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.
 //
 // Permission is hereby granted, free of charge, to any person obtaining a copy of this
 // software and associated documentation files (the "Software"), to deal in the Software
 // without restriction, including without limitation the rights to use, copy, modify,
 // merge, publish, distribute, sublicense, and/or sell copies of the Software, and to
 // permit persons to whom the Software is furnished to do so.
 //
 // THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED,
 // INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A
 // PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT
 // HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION
 // OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
 // SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
 //
 
package com.amazonaws.cloudhsm.examples;

import com.cavium.key.CaviumKey;
import com.cavium.key.parameter.CaviumAESKeyGenParameterSpec;
import com.cavium.key.parameter.CaviumRSAKeyGenParameterSpec;
import com.cavium.asn1.Encoder;
import com.cavium.cfm2.Util;

import javax.crypto.KeyGenerator;

import java.io.ByteArrayInputStream;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.FileNotFoundException;

import java.math.BigInteger;

import java.security.*;
import java.security.cert.Certificate;
import java.security.cert.CertificateException;
import java.security.cert.CertificateFactory;
import java.security.cert.X509Certificate;
import java.security.interfaces.RSAPrivateKey;
import java.security.interfaces.RSAPublicKey;
import java.security.KeyStore.PasswordProtection;
import java.security.KeyStore.PrivateKeyEntry;
import java.security.KeyStore.Entry;

import java.util.Calendar;
import java.util.Date;
import java.util.Enumeration;

//
// KeyStoreExampleRunner demonstrates how to load a keystore, and associate a certificate with a
// key in that keystore.
//
// This example relies on implicit credentials, so you must setup your environment correctly.
//
// https://docs.aws.amazon.com/cloudhsm/latest/userguide/java-library-install.html#java-library-credentials
//

public class KeyStoreExampleRunner {

     private static byte[] COMMON_NAME_OID = new byte[] { (byte) 0x55, (byte) 0x04, (byte) 0x03 };
     private static byte[] COUNTRY_NAME_OID = new byte[] { (byte) 0x55, (byte) 0x04, (byte) 0x06 };
     private static byte[] LOCALITY_NAME_OID = new byte[] { (byte) 0x55, (byte) 0x04, (byte) 0x07 };
     private static byte[] STATE_OR_PROVINCE_NAME_OID = new byte[] { (byte) 0x55, (byte) 0x04, (byte) 0x08 };
     private static byte[] ORGANIZATION_NAME_OID = new byte[] { (byte) 0x55, (byte) 0x04, (byte) 0x0A };
     private static byte[] ORGANIZATION_UNIT_OID = new byte[] { (byte) 0x55, (byte) 0x04, (byte) 0x0B };

     private static String helpString = "KeyStoreExampleRunner%n" +
            "This sample demonstrates how to load and store keys using a keystore.%n%n" +
            "Options%n" +
            "\t--help\t\t\tDisplay this message.%n" +
            "\t--store <filename>\t\tPath of the keystore.%n" +
            "\t--password <password>\t\tPassword for the keystore (not your CU password).%n" +
            "\t--label <label>\t\t\tLabel to store the key and certificate under.%n" +
            "\t--list\t\t\tList all the keys in the keystore.%n%n";

    public static void main(String[] args) throws Exception {
        Security.addProvider(new com.cavium.provider.CaviumProvider());
        KeyStore keyStore = KeyStore.getInstance("CloudHSM");

        String keystoreFile = null;
        String password = null;
        String label = null;
        boolean list = false;
        for (int i = 0; i < args.length; i++) {
            String arg = args[i];
            switch (args[i]) {
                case "--store":
                    keystoreFile = args[++i];
                    break;
                case "--password":
                    password = args[++i];
                    break;
                case "--label":
                    label = args[++i];
                    break;
                case "--list":
                    list = true;
                    break;
                case "--help":
                    help();
                    return;
            }
        }

        if (null == keystoreFile || null == password) {
            help();
            return;
        }

        if (list) {
            listKeys(keystoreFile, password);
            return;
        }

        if (null == label) {
            label = "Keystore Example Keypair";
        }

        //
        // This call to keyStore.load() will open the pkcs12 keystore with the supplied
        // password and connect to the HSM. The CU credentials must be specified using
        // standard CloudHSM login methods.
        //
        try {
            FileInputStream instream = new FileInputStream(keystoreFile);
            keyStore.load(instream, password.toCharArray());
        } catch (FileNotFoundException ex) {
            System.err.println("Keystore not found, loading an empty store");
            keyStore.load(null, null);
        }

        PasswordProtection passwd = new PasswordProtection(password.toCharArray());
        System.out.println("Searching for example key and certificate...");

        PrivateKeyEntry keyEntry = (PrivateKeyEntry) keyStore.getEntry(label, passwd);
        if (null == keyEntry) {
            //
            // No entry was found, so we need to create a key pair and associate a certificate.
            // The private key will get the label passed on the command line. The keystore alias
            // needs to be the same as the private key label. The public key will have ":public"
            // appended to it. The alias used in the keystore will We associate the certificate
            // with the private key.
            //
            System.out.println("No entry found, creating...");
            KeyPair kp = generateRSAKeyPair(2048, label + ":public", label);
            System.out.printf("Created a key pair with the handles %d/%d%n", ((CaviumKey) kp.getPrivate()).getHandle(), ((CaviumKey) kp.getPublic()).getHandle());

            //
            // Generate a certificate and associate the chain with the private key.
            //
            Certificate self_signed_cert = generateCert(kp);
            Certificate[] chain = new Certificate[1];
            chain[0] = self_signed_cert;
            PrivateKeyEntry entry = new PrivateKeyEntry(kp.getPrivate(), chain);

            //
            // Set the entry using the label as the alias and save the store.
            // The alias must match the private key label.
            //
            keyStore.setEntry(label, entry, passwd);

            FileOutputStream outstream = new FileOutputStream(keystoreFile);
            keyStore.store(outstream, password.toCharArray());
            outstream.close();

            keyEntry = (PrivateKeyEntry) keyStore.getEntry(label, passwd);
        }

        long handle = ((CaviumKey) keyEntry.getPrivateKey()).getHandle();
        String name = keyEntry.getCertificate().toString();
        System.out.printf("Found private key %d with certificate %s%n", handle, name);
    }

    private static void help() {
        System.out.println(helpString);
    }

    //
    // Generate a non-extractable / non-persistent RSA keypair.
    // This method allows us to specify the public and private labels, which
    // will make KeyStore alises easier to understand.
    //
    public static KeyPair generateRSAKeyPair(int keySizeInBits, String publicLabel, String privateLabel)
            throws InvalidAlgorithmParameterException, NoSuchAlgorithmException, NoSuchProviderException {

        boolean isExtractable = false;
        boolean isPersistent = false;
        KeyPairGenerator keyPairGen = KeyPairGenerator.getInstance("rsa", "Cavium");
        CaviumRSAKeyGenParameterSpec spec = new CaviumRSAKeyGenParameterSpec(keySizeInBits, new BigInteger("65537"), publicLabel, privateLabel, isExtractable, isPersistent);

        keyPairGen.initialize(spec);

        return keyPairGen.generateKeyPair();
    }

    //
    // Generate a certificate signed by a given keypair.
    //
    private static Certificate generateCert(KeyPair kp) throws CertificateException {
        CertificateFactory cf = CertificateFactory.getInstance("X509");
        PublicKey publicKey = kp.getPublic();
        PrivateKey privateKey = kp.getPrivate();
        byte[] version = Encoder.encodeConstructed((byte) 0, Encoder.encodePositiveBigInteger(new BigInteger("2"))); // version 1
        byte[] serialNo = Encoder.encodePositiveBigInteger(new BigInteger(1, Util.computeKCV(publicKey.getEncoded())));

        // Use the SHA512 OID and algorithm.
        byte[] signatureOid = new byte[] {
            (byte) 0x2A, (byte) 0x86, (byte) 0x48, (byte) 0x86, (byte) 0xF7, (byte) 0x0D, (byte) 0x01, (byte) 0x01, (byte) 0x0D };
        String sigAlgoName = "SHA512WithRSA";

         byte[] signatureId = Encoder.encodeSequence(
                                         Encoder.encodeOid(signatureOid),
                                         Encoder.encodeNull());

         byte[] issuer = Encoder.encodeSequence(
                                     encodeName(COUNTRY_NAME_OID, "<Country>"),
                                     encodeName(STATE_OR_PROVINCE_NAME_OID, "<State>"),
                                     encodeName(LOCALITY_NAME_OID, "<City>"),
                                     encodeName(ORGANIZATION_NAME_OID, "<Organization>"),
                                     encodeName(ORGANIZATION_UNIT_OID, "<Unit>"),
                                     encodeName(COMMON_NAME_OID, "<CN>")
                                 );

         Calendar c = Calendar.getInstance();
         c.add(Calendar.DAY_OF_YEAR, -1);
         Date notBefore = c.getTime();
         c.add(Calendar.YEAR, 1);
         Date notAfter = c.getTime();
         byte[] validity = Encoder.encodeSequence(
                                         Encoder.encodeUTCTime(notBefore),
                                         Encoder.encodeUTCTime(notAfter)
                                     );
         byte[] key = publicKey.getEncoded();

         byte[] certificate = Encoder.encodeSequence(
                                         version,
                                         serialNo,
                                         signatureId,
                                         issuer,
                                         validity,
                                         issuer,
                                         key);
         Signature sig;
         byte[] signature = null;
         try {
             sig = Signature.getInstance(sigAlgoName, "Cavium");
             sig.initSign(privateKey);
             sig.update(certificate);
             signature = Encoder.encodeBitstring(sig.sign());

         } catch (Exception e) {
             System.err.println(e.getMessage());
             return null;
         }

         byte [] x509 = Encoder.encodeSequence(
                         certificate,
                         signatureId,
                         signature
                         );
         return cf.generateCertificate(new ByteArrayInputStream(x509));
    }

     //
     // Simple OID encoder.
     // Encode a value with OID in ASN.1 format
     //
     private static byte[] encodeName(byte[] nameOid, String value) {
         byte[] name = null;
         name = Encoder.encodeSet(
                     Encoder.encodeSequence(
                             Encoder.encodeOid(nameOid),
                             Encoder.encodePrintableString(value)
                     )
                 );
         return name;
     }

    //
    // List all the keys in the keystore.
    //
    private static void listKeys(String keystoreFile, String password) throws Exception {
        KeyStore keyStore = KeyStore.getInstance("CloudHSM");

        try {
            FileInputStream instream = new FileInputStream(keystoreFile);
            keyStore.load(instream, password.toCharArray());
        } catch (FileNotFoundException ex) {
            System.err.println("Keystore not found, loading an empty store");
            keyStore.load(null, null);
        }

        for(Enumeration<String> entry = keyStore.aliases(); entry.hasMoreElements();) {
            System.out.println(entry.nextElement());
        }
    }

}
```