# Using Client SDK 5 to integrate with Java Keytool and Jarsigner<a name="keystore-third-party-tools_5"></a>

AWS CloudHSM key store is a special\-purpose JCE key store that utilizes certificates associated with keys on your HSM through third\-party tools such as `keytool` and `jarsigner`\. AWS CloudHSM does not store certificates on the HSM, as certificates are public, non\-confidential data\. The AWS CloudHSM key store stores the certificates in a local file and maps the certificates to corresponding keys on your HSM\. 

When you use the AWS CloudHSM key store to generate new keys, no entries are generated in the local key store file – the keys are created on the HSM\. Similarly, when you use the AWS CloudHSM key store to search for keys, the search is passed on to the HSM\. When you store certificates in the AWS CloudHSM key store, the provider verifies that a key pair with the corresponding alias exists on the HSM, and then associates the certificate provided with the corresponding key pair\. 

**Topics**
+ [Prerequisites](#keystore-prerequisites_5)
+ [Using AWS CloudHSM key store with keytool](#using_keystore_with_keytool_5)
+ [Using AWS CloudHSM key store with Jarsigner](#using_keystore_jarsigner)
+ [Known issues](#known-issues-keytool-jarsigner_5)

## Prerequisites<a name="keystore-prerequisites_5"></a>

To use the AWS CloudHSM key store, you must first initialize and configure the AWS CloudHSM JCE SDK\. 

### Step 1: Install the JCE<a name="prereq-step-one_5"></a>

To install the JCE, including the AWS CloudHSM client prerequisites, follow the steps for [installing the Java library](java-library-install_5.md)\. 

### Step 2: Add HSM login credentials to environment variables<a name="prereq-step-two_5"></a>

Set up environment variables to contain your HSM login credentials\. 

------
#### [ Linux ]

```
$ export HSM_USER=<HSM user name>
```

```
$ export HSM_PASSWORD=<HSM password>
```

------
#### [ Windows ]

```
PS C:\> $Env:HSM_USER=<HSM user name>
```

```
PS C:\> $Env:HSM_PASSWORD=<HSM password>
```

------

**Note**  
The AWS CloudHSM JCE offers various login options\. To use the AWS CloudHSM key store with third\-party applications, you must use implicit login with environment variables\. If you want to use explicit login through application code, you must build your own application using the AWS CloudHSM key store\. For additional information, see the article on [Using AWS CloudHSM Key Store](alternative-keystore_5.md)\. 

### Step 3: Registering the JCE provider<a name="prereq-step-three_5"></a>

To register the JCE provider in the Java CloudProvider configuration, follow these steps: 

1. Open the `java.security` configuration file in your Java installation for editing\.

1. In the `java.security` configuration file, add `com.amazonaws.cloudhsm.jce.provider.CloudHsmProvider` as the last provider\. For example, if there are nine providers in the `java.security` file, add the following provider as the last provider in the section:

   `security.provider.10=com.amazonaws.cloudhsm.jce.provider.CloudHsmProvider`

**Note**  
Adding the AWS CloudHSM provider as a higher priority may negatively impact your system's performance because the AWS CloudHSM provider will be prioritized for operations that may be safely offloaded to software\. As a best practice, **always** specify the provider you wish to use for an operation, whether it is the AWS CloudHSM or a software\-based provider\.

**Note**  
Specifying `-providerName`, `-providerclass`, and `-providerpath` command line options when generating keys using keytool with the AWS CloudHSM key store may cause errors\.

## Using AWS CloudHSM key store with keytool<a name="using_keystore_with_keytool_5"></a>

 [ Keytool](https://docs.oracle.com/javase/8/docs/technotes/tools/unix/keytool.html) is a popular command line utility for common key and certificate tasks\. A complete tutorial on keytool is out of scope for AWS CloudHSM documentation\. This article explains the specific parameters you should use with various keytool functions when utilizing AWS CloudHSM as the root of trust through the AWS CloudHSM key store\.

When using keytool with the AWS CloudHSM key store, specify the following arguments to any keytool command:

------
#### [ Linux ]

```
-storetype CLOUDHSM -J-classpath< '-J/opt/cloudhsm/java/*'>
```

------
#### [ Windows ]

```
-storetype CLOUDHSM -J-classpath<'-J"C:\Program Files\Amazon\CloudHSM\java\*"'>
```

------

If you want to create a new key store file using AWS CloudHSM key store, see [Using AWS CloudHSM KeyStore](alternative-keystore.md#using_cloudhsm_keystore)\. To use an existing key store, specify its name \(including path\) using the –keystore argument to keytool\. If you specify a non\-existent key store file in a keytool command, the AWS CloudHSM key store creates a new key store file\.

### Create new keys with keytool<a name="create_key_keytool_5"></a>

You can use keytool to generate RSA, AES, and DESede type of key supported by AWS CloudHSM's JCE SDK\.

**Important**  
A key generated through keytool is generated in software, and then imported into AWS CloudHSM as an extractable, persistent key\.

We strongly recommend generating non\-exportable keys outside of keytool, and then importing corresponding certificates to the key store\. If you use extractable RSA or EC keys through keytool and Jarsigner, the providers export keys from the AWS CloudHSM and then use the key locally for signing operations\.

If you have multiple client instances connected to your AWS CloudHSM cluster, be aware that importing a certificate on one client instance’s key store won't automatically make the certificates available on other client instances\. To register the key and associated certificates on each client instance you need to run a Java application as described in [Generate a CSR using keytool](#generate_csr_using_keytool_5)\. Alternatively, you can make the necessary changes on one client and copy the resulting key store file to every other client instance\.

**Example 1: **To generate a symmetric AES\-256 key and save it in a key store file named, "my\_keystore\.store", in the working directory\. Replace *<secret label>* with a unique label\.

------
#### [ Linux ]

```
$ keytool -genseckey -alias <secret label> -keyalg aes \
	-keysize 256 -keystore my_keystore.store \
	-storetype CloudHSM -J-classpath '-J/opt/cloudhsm/java/*' \
```

------
#### [ Windows ]

```
PS C:\> keytool -genseckey -alias <secret label> -keyalg aes `
	-keysize 256 -keystore my_keystore.store `
	-storetype CloudHSM -J-classpath '-J"C:\Program Files\Amazon\CloudHSM\java\*"'
```

------

**Example 2: **To generate an RSA 2048 key pair and save it in a key store file named, "my\_keystore\.store" in the working directory\. Replace *<RSA key pair label>* with a unique label\.

------
#### [ Linux ]

```
$ keytool -genkeypair -alias <RSA key pair label> \
	-keyalg rsa -keysize 2048 \
	-sigalg sha512withrsa \
	-keystore my_keystore.store \
	-storetype CLOUDHSM \
	-J-classpath '-J/opt/cloudhsm/java/*'
```

------
#### [ Windows ]

```
PS C:\> keytool -genkeypair -alias <RSA key pair label> `
	-keyalg rsa -keysize 2048 `
	-sigalg sha512withrsa `
	-keystore my_keystore.store `
	-storetype CLOUDHSM `
	-J-classpath '-J"C:\Program Files\Amazon\CloudHSM\java\*"'
```

------

You can find a list of [supported signature algorithms](java-lib-supported_5.md#java-sign-verify_5) in the Java library\.

### Delete a key using keytool<a name="delete_key_using_keytool"></a>

The AWS CloudHSM key store doesn't support deleting keys\. You can delete keys using the destroy method of the [Destroyable interface](https://devdocs.io/openjdk%7E8/javax/security/auth/destroyable#destroy--)\.

```
((Destroyable) key).destroy();
```

### Generate a CSR using keytool<a name="generate_csr_using_keytool_5"></a>

You receive the greatest flexibility in generating a certificate signing request \(CSR\) if you use the [OpenSSL Dynamic Engine](openssl-library.md)\. The following command uses keytool to generate a CSR for a key pair with the alias, `my-key-pair`\.

------
#### [ Linux ]

```
$ keytool -certreq -alias <key pair label> \
	-file my_csr.csr \
	-keystore my_keystore.store \
	-storetype CLOUDHSM \
	-J-classpath '-J/opt/cloudhsm/java/*'
```

------
#### [ Windows ]

```
PS C:\> keytool -certreq -alias <key pair label> `
	-file my_csr.csr `
	-keystore my_keystore.store `
	-storetype CLOUDHSM `
	-J-classpath '-J"C:\Program Files\Amazon\CloudHSM\java\*"'
```

------

**Note**  
To use a key pair from keytool, that key pair must have an entry in the specified key store file\. If you want to use a key pair that was generated outside of keytool, you must import the key and certificate metadata into the key store\. For instructions on importing the keystore data see [Using keytool to import intermediate and root certificates into AWS CloudHSM key store ](#import_cert_using_keytool_5)\.

### Using keytool to import intermediate and root certificates into AWS CloudHSM key store<a name="import_cert_using_keytool_5"></a>

To import a CA certificate you must enable verification of a full certificate chain on a newly imported certificate\. The following command shows an example\. 

------
#### [ Linux ]

```
$ keytool -import -trustcacerts -alias rootCAcert \
	-file rootCAcert.cert -keystore my_keystore.store \
	-storetype CLOUDHSM \
	-J-classpath '-J/opt/cloudhsm/java/*'
```

------
#### [ Windows ]

```
PS C:\> keytool -import -trustcacerts -alias rootCAcert `
	-file rootCAcert.cert -keystore my_keystore.store `
	-storetype CLOUDHSM `
	-J-classpath '-J"C:\Program Files\Amazon\CloudHSM\java\*"'
```

------

If you connect multiple client instances to your AWS CloudHSM cluster, importing a certificate on one client instance’s key store won't automatically make the certificate available on other client instances\. You must import the certificate on each client instance\.

### Using keytool to delete certificates from AWS CloudHSM key store<a name="delete_cert_using_keytool_5"></a>

The following command shows an example of how to delete a certificate from a Java keytool key store\. 

------
#### [ Linux ]

```
$ keytool -delete -alias mydomain -keystore \
	-keystore my_keystore.store \
	-storetype CLOUDHSM \
	-J-classpath '-J/opt/cloudhsm/java/*'
```

------
#### [ Windows ]

```
PS C:\> keytool -delete -alias mydomain -keystore `
	-keystore my_keystore.store `
	-storetype CLOUDHSM `
	-J-classpath '-J"C:\Program Files\Amazon\CloudHSM\java\*"'
```

------

If you connect multiple client instances to your AWS CloudHSM cluster, deleting a certificate on one client instance’s key store won't automatically remove the certificate from other client instances\. You must delete the certificate on each client instance\.

### Importing a working certificate into AWS CloudHSM key store using keytool<a name="import_working_cert_using_keytool_5"></a>

Once a certificate signing request \(CSR\) is signed, you can import it into the AWS CloudHSM key store and associate it with the appropriate key pair\. The following command provides an example\. 

------
#### [ Linux ]

```
$ keytool -importcert -noprompt -alias <key pair label> \
	-file my_certificate.crt \
	-keystore my_keystore.store \
	-storetype CLOUDHSM \
	-J-classpath '-J/opt/cloudhsm/java/*'
```

------
#### [ Windows ]

```
PS C:\> keytool -importcert -noprompt -alias <key pair label> `
	-file my_certificate.crt `
	-keystore my_keystore.store `
	-storetype CLOUDHSM `
	-J-classpath '-J"C:\Program Files\Amazon\CloudHSM\java\*"'
```

------

The alias should be a key pair with an associated certificate in the key store\. If the key is generated outside of keytool, or is generated on a different client instance, you must first import the key and certificate metadata into the key store\. 

The certificate chain must be verifiable\. If you can't verify the certificate, you might need to import the signing \(certificate authority\) certificate into the key store so the chain can be verified\.

### Exporting a certificate using keytool<a name="export_cert_using_keytool_5"></a>

The following example generates a certificate in binary X\.509 format\. To export a human readable certificate, add `-rfc` to the `-exportcert` command\. 

------
#### [ Linux ]

```
$ keytool -exportcert -alias <key pair label> \
	-file my_exported_certificate.crt \
	-keystore my_keystore.store \
	-storetype CLOUDHSM \
	-J-classpath '-J/opt/cloudhsm/java/*'
```

------
#### [ Windows ]

```
PS C:\> keytool -exportcert -alias <key pair label> `
	-file my_exported_certificate.crt `
	-keystore my_keystore.store `
	-storetype CLOUDHSM `
	-J-classpath '-J"C:\Program Files\Amazon\CloudHSM\java\*"'
```

------

## Using AWS CloudHSM key store with Jarsigner<a name="using_keystore_jarsigner"></a>

Jarsigner is a popular command line utility for signing JAR files using a key securely stored on a HSM\. A complete tutorial on Jarsigner is out of scope for the AWS CloudHSM documentation\. This section explains the Jarsigner parameters you should use to sign and verify signatures with AWS CloudHSM as the root of trust through the AWS CloudHSM key store\. 

### Setting up keys and certificates<a name="jarsigner_set_up_certificates_5"></a>

Before you can sign JAR files with Jarsigner, make sure you have set up or completed the following steps: 

1. Follow the guidance in the [AWS CloudHSM key store prerequisites ](#keystore-prerequisites_5)\.

1. Set up your signing keys and the associated certificates and certificate chain which should be stored in the AWS CloudHSM key store of the current server or client instance\. Create the keys on the AWS CloudHSM and then import associated metadata into your AWS CloudHSM key store\. If you want to use keytool to set up the keys and certificates, see [Create new keys with keytool](#create_key_keytool_5)\. If you use multiple client instances to sign your JARs, create the key and import the certificate chain\. Then copy the resulting key store file to each client instance\. If you frequently generate new keys, you may find it easier to individually import certificates to each client instance\.

1. The entire certificate chain should be verifiable\. For the certificate chain to be verifiable, you may need to add the CA certificate and intermediate certificates to the AWS CloudHSM key store\. See the code snippet in [Sign a JAR file using AWS CloudHSM and Jarsigner](#jarsigner_sign_jar_using_hsm_jarsigner_5) for instruction on using Java code to verify the certificate chain\. If you prefer, you can use keytool to import certificates\. For instructions on using keytool, see [Using keytool to import intermediate and root certificates into AWS CloudHSM key store ](#import_cert_using_keytool_5)\. 

### Sign a JAR file using AWS CloudHSM and Jarsigner<a name="jarsigner_sign_jar_using_hsm_jarsigner_5"></a>

Use the following command to sign a JAR file: 

------
#### [ Amazon Linux ]

For OpenJDK8

```
jarsigner -keystore my_keystore.store \
	-signedjar signthisclass_signed.jar \
	-sigalg sha512withrsa \
	-storetype CloudHSM \
	-J-classpath '-J/opt/cloudhsm/java/*:/usr/lib/jvm/java-1.8.0/lib/tools.jar' \
	-J-Djava.library.path=/opt/cloudhsm/lib \
	signthisclass.jar <key pair label>
```

For OpenJDK11

```
jarsigner -keystore my_keystore.store \
	-signedjar signthisclass_signed.jar \
	-sigalg sha512withrsa \
	-storetype CloudHSM \
	-J-classpath '-J/opt/cloudhsm/java/*' \
	-J-Djava.library.path=/opt/cloudhsm/lib \
	signthisclass.jar <key pair label>
```

------
#### [ Windows ]

For OpenJDK8

```
jarsigner -keystore my_keystore.store `
	-signedjar signthisclass_signed.jar `
	-sigalg sha512withrsa `
	-storetype CloudHSM `
	-J-classpath '-JC:\Program Files\Amazon\CloudHSM\java\*;C:\Program Files\Java\jdk1.8.0_331\lib\tools.jar' `
	 "-J-Djava.library.path='C:\Program Files\Amazon\CloudHSM\lib\'" `
	signthisclass.jar <key pair label>
```

For OpenJDK11

```
jarsigner -keystore my_keystore.store `
	-signedjar signthisclass_signed.jar `
	-sigalg sha512withrsa `
	-storetype CloudHSM `
	-J-classpath '-JC:\Program Files\Amazon\CloudHSM\java\*'`
	 "-J-Djava.library.path='C:\Program Files\Amazon\CloudHSM\lib\'" `
	signthisclass.jar <key pair label>
```

------

Use the following command to verify a signed JAR: 

------
#### [ Amazon Linux ]

For OpenJDK8

```
jarsigner -verify \
	-keystore my_keystore.store \
	-sigalg sha512withrsa \
	-storetype CloudHSM \
	-J-classpath '-J/opt/cloudhsm/java/*:/usr/lib/jvm/java-1.8.0/lib/tools.jar' \
	-J-Djava.library.path=/opt/cloudhsm/lib \
	signthisclass_signed.jar <key pair label>
```

For OpenJDK11

```
jarsigner -verify \
	-keystore my_keystore.store \
	-sigalg sha512withrsa \
	-storetype CloudHSM \
	-J-classpath '-J/opt/cloudhsm/java/*' \
	-J-Djava.library.path=/opt/cloudhsm/lib \
	signthisclass_signed.jar <key pair label>
```

------
#### [ Windows ]

For OpenJDK8

```
jarsigner -verify `
	-keystore my_keystore.store `
	-sigalg sha512withrsa `
	-storetype CloudHSM `
	-J-classpath '-JC:\Program Files\Amazon\CloudHSM\java\*;C:\Program Files\Java\jdk1.8.0_331\lib\tools.jar' `
	"-J-Djava.library.path='C:\Program Files\Amazon\CloudHSM\lib\'" `
	signthisclass_signed.jar <key pair label>
```

For OpenJDK11

```
jarsigner -verify `
	-keystore my_keystore.store `
	-sigalg sha512withrsa `
	-storetype CloudHSM `
	-J-classpath '-JC:\Program Files\Amazon\CloudHSM\java\*`
	"-J-Djava.library.path='C:\Program Files\Amazon\CloudHSM\lib\'" `
	signthisclass_signed.jar <key pair label>
```

------

## Known issues<a name="known-issues-keytool-jarsigner_5"></a>



1. We do not support EC keys with Keytool and Jarsigner\.