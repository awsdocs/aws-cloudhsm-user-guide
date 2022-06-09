# Install and use the AWS CloudHSM JCE provider for Client SDK 3<a name="java-library-install"></a>

Before you can use the JCE provider, you need the AWS CloudHSM client\. 

The client is a daemon that establishes end\-to\-end encrypted communication with the HSMs in your cluster\. The JCE provider communicates locally with the client\. If you haven't installed and configured the AWS CloudHSM client package, do that now by following the steps at [Install the client \(Linux\)](install-and-configure-client-linux.md)\. After you install and configure the client, use the following command to start it\. 

Note that the JCE provider is supported only on Linux and compatible operating systems\. 

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

**Topics**
+ [Installing the JCE provider](#install-java-library)
+ [Validating the installation](#validate-install)
+ [Providing credentials to the JCE provider](#java-library-credentials)
+ [Key management basics in the JCE provider](#java-library-key-basics)

## Installing the JCE provider<a name="install-java-library"></a>

Use the following commands to download and install the JCE provider\. This provider is supported only on Linux and compatible operating systems\. 

**Note**  
For upgrading, see [Upgrade your Client SDK 3](client-upgrade.md)\.

------
#### [ Amazon Linux ]

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL6/cloudhsm-client-jce-latest.el6.x86_64.rpm
```

```
$ sudo yum install ./cloudhsm-client-jce-latest.el6.x86_64.rpm
```

------
#### [ Amazon Linux 2 ]

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL7/cloudhsm-client-jce-latest.el7.x86_64.rpm
```

```
$ sudo yum install ./cloudhsm-client-jce-latest.el7.x86_64.rpm
```

------
#### [ CentOS 7 ]

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL7/cloudhsm-client-jce-latest.el7.x86_64.rpm
```

```
$ sudo yum install ./cloudhsm-client-jce-latest.el7.x86_64.rpm
```

------
#### [ CentOS 8 ]

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL8/cloudhsm-client-jce-latest.el8.x86_64.rpm
```

```
$ sudo yum install ./cloudhsm-client-jce-latest.el8.x86_64.rpm
```

------
#### [ RHEL 7 ]

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL7/cloudhsm-client-jce-latest.el7.x86_64.rpm
```

```
$ sudo yum install ./cloudhsm-client-jce-latest.el7.x86_64.rpm
```

------
#### [ RHEL 8 ]

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL8/cloudhsm-client-jce-latest.el8.x86_64.rpm
```

```
$ sudo yum install ./cloudhsm-client-jce-latest.el8.x86_64.rpm
```

------
#### [ Ubuntu 16\.04 LTS ]

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/Xenial/cloudhsm-client-jce_latest_amd64.deb
```

```
$ sudo apt install ./cloudhsm-client-jce_latest_amd64.deb
```

------
#### [ Ubuntu 18\.04 LTS ]

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/Bionic/cloudhsm-client-jce_latest_u18.04_amd64.deb
```

```
$ sudo apt install ./cloudhsm-client-jce_latest_u18.04_amd64.deb
```

------

After you run the preceding commands, you can find the following JCE provider files:
+ `/opt/cloudhsm/java/cloudhsm-version.jar`
+ `/opt/cloudhsm/java/cloudhsm-test-version.jar`
+ `/opt/cloudhsm/java/hamcrest-all-1.3.jar`
+ `/opt/cloudhsm/java/junit.jar`
+ `/opt/cloudhsm/java/log4j-api-2.17.1.jar`
+ `/opt/cloudhsm/java/log4j-core-2.17.1.jar`
+ `/opt/cloudhsm/lib/libcaviumjca.so`

## Validating the installation<a name="validate-install"></a>

Perform basic operations on the HSM to validate the installation\.

**To validate JCE provider installation**

1. \(Optional\) If you don't already have Java installed in your environment, use the following command to install it\. 

------
#### [ Linux \(and compatible libraries\) ]

   ```
   $ sudo yum install java-1.8.0-openjdk
   ```

------
#### [ Ubuntu ]

   ```
   $ sudo apt-get install openjdk-8-jre
   ```

------

1. Use the following commands to set the necessary environment variables\. Replace *<HSM user name>* and *<password>* with the credentials of a crypto user \(CU\)\.

   ```
   $ export LD_LIBRARY_PATH=/opt/cloudhsm/lib
   ```

   ```
   $ export HSM_PARTITION=PARTITION_1
   ```

   ```
   $ export HSM_USER=<HSM user name>
   ```

   ```
   $ export HSM_PASSWORD=<password>
   ```

1. Use the following command to run the basic functionality test\. If successful, the command's output should be similar to the one that follows\.

   ```
   $ java8 -classpath "/opt/cloudhsm/java/*" org.junit.runner.JUnitCore TestBasicFunctionality
   
   JUnit version 4.11
   .2018-08-20 17:53:48,514 DEBUG [main] TestBasicFunctionality (TestBasicFunctionality.java:33) - Adding provider.
   2018-08-20 17:53:48,612 DEBUG [main] TestBasicFunctionality (TestBasicFunctionality.java:42) - Logging in.
   2018-08-20 17:53:48,612 INFO [main] cfm2.LoginManager (LoginManager.java:104) - Looking for credentials in HsmCredentials.properties
   2018-08-20 17:53:48,612 INFO [main] cfm2.LoginManager (LoginManager.java:122) - Looking for credentials in System.properties
   2018-08-20 17:53:48,613 INFO [main] cfm2.LoginManager (LoginManager.java:130) - Looking for credentials in System.env
    SDK Version: 2.03
   2018-08-20 17:53:48,655 DEBUG [main] TestBasicFunctionality (TestBasicFunctionality.java:54) - Generating AES Key with key size 256.
   2018-08-20 17:53:48,698 DEBUG [main] TestBasicFunctionality (TestBasicFunctionality.java:63) - Encrypting with AES Key.
   2018-08-20 17:53:48,705 DEBUG [main] TestBasicFunctionality (TestBasicFunctionality.java:84) - Deleting AES Key.
   2018-08-20 17:53:48,707 DEBUG [main] TestBasicFunctionality (TestBasicFunctionality.java:92) - Logging out.
   
   Time: 0.205
   
   OK (1 test)
   ```

## Providing credentials to the JCE provider<a name="java-library-credentials"></a>

HSMs need to authenticate your Java application before the application can use them\. Each application can use one session\. HSMs authenticate a session by using either explicit login or implicit login method\.

**Explicit login** – This method lets you provide CloudHSM credentials directly in the application\. It uses the `LoginManager.login()` method, where you pass the CU user name, password, and the HSM partition ID\. For more information about using the explicit login method, see the [Login to an HSM](https://github.com/aws-samples/aws-cloudhsm-jce-examples/blob/master/src/main/java/com/amazonaws/cloudhsm/examples/LoginRunner.java) code example\. 

**Implicit login** – This method lets you set CloudHSM credentials either in a new property file, system properties, or as environment variables\.
+ **New property file** – Create a new file named `HsmCredentials.properties` and add it to your application's `CLASSPATH`\. The file should contain the following:

  ```
  HSM_PARTITION = PARTITION_1
  HSM_USER = <HSM user name>
  HSM_PASSWORD = <password>
  ```
+ **System properties** – Set credentials through system properties when running your application\. The following examples show two different ways that you can do this:

  ```
  $ java -DHSM_PARTITION=PARTITION_1 -DHSM_USER=<HSM user name> -DHSM_PASSWORD=<password>
  ```

  ```
  System.setProperty("HSM_PARTITION","PARTITION_1");
  System.setProperty("HSM_USER","<HSM user name>");
  System.setProperty("HSM_PASSWORD","<password>");
  ```
+ **Environment variables** – Set credentials as environment variables\.

  ```
  $ export HSM_PARTITION=PARTITION_1
  $ export HSM_USER=<HSM user name>
  $ export HSM_PASSWORD=<password>
  ```

Credentials might not be available if the application does not provide them or if you attempt an operation before the HSM authenticates session\. In those cases, the CloudHSM software library for Java searches for the credentials in the following order:

1. `HsmCredentials.properties`

1. System properties

1. Environment variables

**Error handling**  
The error handling is easier with the explicit login than the implicit login method\. When you use the `LoginManager` class, you have more control over how your application deals with failures\. The implicit login method makes error handling difficult to understand when the credentials are invalid or the HSMs are having problems in authenticating session\.

## Key management basics in the JCE provider<a name="java-library-key-basics"></a>

The basics on key management in the JCE provider involve importing keys, exporting keys, loading keys by handle, or deleting keys\. For more information on managing keys, see the [Manage keys](https://github.com/aws-samples/aws-cloudhsm-jce-examples/blob/master/src/main/java/com/amazonaws/cloudhsm/examples/KeyUtilitiesRunner.java) code example\.

You can also find more JCE provider code examples at [Java samples](java-samples.md)\.