# Using CloudHSM KeyStore Java Class<a name="alternative-keystore"></a>

The AWS CloudHSM `KeyStore` class provides a special\-purpose PKCS12 key store that allows access to AWS CloudHSM keys through applications such as **keytool** and **jarsigner**\. This key store can store certificates along with your key data and correlate them to key data stored on AWS CloudHSM\. 

**Note**  
Because certificates are public information, and to maximize storage capacity for cryptographic keys, AWS CloudHSM does not support storing certificates on HSMs\.

The AWS CloudHSM `KeyStore` class implements the `KeyStore` Service Provider Interface \(SPI\) of the Java Cryptography Extension \(JCE\)\. For more information about using `KeyStore`, see [Class KeyStore](https://docs.oracle.com/javase/8/docs/api/java/security/KeyStore.html)\.

## Choosing the Appropriate Key Store<a name="choosing_keystore"></a>

The AWS CloudHSM Java SDK comes with a default pass\-through, read\-only key store that passes all transactions to the HSM\. This default key store is distinct from the special\-purpose AWS CloudHSM KeyStore\. In most situations, you will obtain better runtime performance and throughput by using the default\. You should only use the AWS CloudHSM KeyStore for applications where you require support for certificates and certificate\-based operations in addition to offloading key operations to the HSM\.

Although both key stores use the Cavium JCE provider for operations, they are independent entities and do not exchange information with each other\. 

Load the default key store for your Java application as follows:

```
KeyStore ks = KeyStore.getInstance("Cavium");
```

Load the special\-purpose CloudHSM KeyStore as follows:

```
KeyStore ks = KeyStore.getInstance("CloudHSM")
```

## Initializing AWS CloudHSM KeyStore<a name="initialize_cloudhsm_keystore"></a>

Log into the AWS CloudHSM KeyStore the same way that you log into the JCE provider for AWS CloudHSM\. You can use either environment variables or the system property file, and you should log in before you start using the CloudHSM KeyStore\. For an example of logging into an HSM using the JCE, see [Login to an HSM](https://github.com/aws-samples/aws-cloudhsm-jce-examples/blob/master/src/main/java/com/amazonaws/cloudhsm/examples/LoginRunner.java)\.

If desired, you can specify a password to encrypt the local PKCS12 file which holds key store data\. When you create the AWS CloudHSM Keystore, you set the password and provide it when using the load, set and get methods\.

Instantiate a new CloudHSM KeyStore object as follows:

```
ks.load(null, null);
```

Write keystore data to a file using the `store` method\. From that point on, you can load the existing keystore using the `load` method with the source file and password as follows: 

```
ks.load(inputStream, password);
```

## Using CloudHSM KeyStore<a name="using_cloudhsm_keystore"></a>

A CloudHSM KeyStore object is generally used through a third\-party application such as [jarsigner](https://docs.oracle.com/javase/8/docs/technotes/tools/windows/jarsigner.html) or [keytool](https://docs.oracle.com/javase/8/docs/technotes/tools/unix/keytool.html)\. You can also access the object directly with code\. 

AWS CloudHSM KeyStore complies with the JCE [Class KeyStore](https://docs.oracle.com/javase/8/docs/api/java/security/KeyStore.html) specification and provides the following functions\.
+ `load`

  Loads the key store from the given input stream\. If a password was set when saving the key store, this same password must be provided for the load to succeed\. Set both parameters to null to initialize an new empty key store\.

  ```
  KeyStore ks = KeyStore.getInstance("CloudHSM");
  ks.load(inputStream, password);
  ```
+ `aliases`

  Returns an enumeration of the alias names of all entries in the given key store instance\. Results include objects stored locally in the PKCS12 file and objects resident on the HSM\. 

  **Sample code:**

  ```
  KeyStore ks = KeyStore.getInstance("CloudHSM");
  for(Enumeration<String> entry = ks.aliases(); entry.hasMoreElements();) 
  	{    
  		String label = entry.nextElement();    
  		System.out.println(label);
  	}
  ```
+ `ContainsAlias`

  Returns true if the key store has access to at least one object with the specified alias\. The key store checks objects stored locally in the PKCS12 file and objects resident on the HSM\.
+ `DeleteEntry`

  Deletes a certificate entry from the local PKCS12 file\. Deleting key data stored in an HSM is not supported using the AWS CloudHSM KeyStore\. You can delete keys with CloudHSM’s [key\_mgmt\_util](https://docs.aws.amazon.com/cloudhsm/latest/userguide/key_mgmt_util.html) tool\.
+ `GetCertificate`

  Returns the certificate associated with an alias if available\. If the alias does not exist or references an object which is not a certificate, the function returns NULL\. 

  ```
  KeyStore ks = KeyStore.getInstance("CloudHSM");
  Certificate cert = ks.getCertificate(alias)
  ```
+ `GetCertificateAlias`

  Returns the name \(alias\) of the first key store entry whose data matches the given certificate\. 

  ```
  KeyStore ks = KeyStore.getInstance("CloudHSM");
  String alias = ks.getCertificateAlias(cert)
  ```
+ `GetCertificateChain`

  Returns the certificate chain associated with the given alias\. If the alias does not exist or references an object which is not a certificate, the function returns NULL\. 
+ `GetCreationDate`

  Returns the creation date of the entry identified by the given alias\. If a creation date is not available, the function returns the date on which the certificate became valid\.
+ `GetKey`

  GetKey is passed to the HSM and returns a key object corresponding to the given label\. As `getKey` directly queries the HSM, it can be used for any key on the HSM regardless of whether it was generated by the KeyStore\. 

  ```
  Key key = ks.getKey(keyLabel, null);
  ```
+ `IsCertificateEntry`

  Checks if the entry with the given alias represents a certificate entry\. 
+ `IsKeyEntry`

  Checks if the entry with the given alias represents a key entry\. The action searches both the PKCS12 file and the HSM for the alias\. 
+ `SetCertificateEntry`

  Assigns the given certificate to the given alias\. If the given alias is already being used to identify a key or certificate, a `KeyStoreException` is thrown\. You can use JCE code to get the key object and then use the KeyStore `SetKeyEntry` method to associate the certificate to the key\.
+ `SetKeyEntry` with `byte[]` key

  Assigns the given byte array key to the given alias by storing it inside HSM as a generic key with the given alias\. 
+ `SetKeyEntry` with `Key` object

  Assigns the given key to the given alias and stores it inside the HSM\. If the `Key` object is not of type `CaviumKey`, the key is imported into the HSM as an extractable session key\. 

  If the `Key` object is of type `PrivateKey`, it must be accompanied by a corresponding certificate chain\. 

  If the alias already exists, the `SetKeyEntry` call throws a `KeyStoreException` and prevents the key from being overwritten\. If the key must be overwritten, use KMU or JCE for that purpose\. 
+ `EngineSize`

  Returns the number of entries in the keystore\.
+ `Store`

  Stores the key store to the given output stream as a PKCS12 file and secures it with the given password\. In addition, it persists all loaded keys \(which are set using `setKey` calls\)\.