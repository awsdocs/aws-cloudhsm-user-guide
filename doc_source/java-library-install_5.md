# Install and use the AWS CloudHSM JCE provider for Client SDK 5<a name="java-library-install_5"></a>

The JCE provider is compatible with OpenJDK 8 and OpenJDK11\. You can download both from the [OpenJDK website](https://openjdk.java.net/)\.

**Topics**
+ [Install the JCE provider](#install-java-library_5)
+ [Provide credentials to the JCE provider](#java-library-credentials_5)
+ [Key management basics in the JCE provider](#java-library-key-basics_5)

## Install the JCE provider<a name="install-java-library_5"></a>

Use the following commands to download and install the JCE provider\. 

------
#### [ Amazon Linux ]

Amazon Linux on x86\_64 architecture:

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL6/cloudhsm-jce-latest.el6.x86_64.rpm
```

```
$ sudo yum install ./cloudhsm-jce-latest.el6.x86_64.rpm
```

------
#### [ Amazon Linux 2 ]

Amazon Linux 2 on x86\_64 architecture:

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL7/cloudhsm-jce-latest.el7.x86_64.rpm
```

```
$ sudo yum install ./cloudhsm-jce-latest.el7.x86_64.rpm
```

Amazon Linux 2 on ARM64 architecture:

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL7/cloudhsm-jce-latest.el7.aarch64.rpm
```

```
$ sudo yum install ./cloudhsm-jce-latest.el7.aarch64.rpm
```

------
#### [ CentOS 7 ]

CentOS 7 on x86\_64 architecture:

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL7/cloudhsm-jce-latest.el7.x86_64.rpm
```

```
$ sudo yum install ./cloudhsm-jce-latest.el7.x86_64.rpm
```

------
#### [ CentOS 8 ]

CentOS 8 on x86\_64 architecture:

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL8/cloudhsm-jce-latest.el8.x86_64.rpm
```

```
$ sudo yum install ./cloudhsm-jce-latest.el8.x86_64.rpm
```

------
#### [ RHEL 7 ]

RHEL 7 on x86\_64 architecture:

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL7/cloudhsm-jce-latest.el7.x86_64.rpm
```

```
$ sudo yum install ./cloudhsm-jce-latest.el7.x86_64.rpm
```

------
#### [ RHEL 8 ]

RHEL 8 on x86\_64 architecture:

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL8/cloudhsm-jce-latest.el8.x86_64.rpm
```

```
$ sudo yum install ./cloudhsm-jce-latest.el8.x86_64.rpm
```

------
#### [ Ubuntu 18\.04 LTS ]

Ubuntu 18\.04 LTS on x86\_64 architecture:

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/Bionic/cloudhsm-jce_latest_u18.04_amd64.deb
```

```
$ sudo apt install ./cloudhsm-jce_latest_u18.04_amd64.deb
```

------
#### [ Windows Server 2016 ]

For Windows Server 2016 on x86\_64 architecture, open PowerShell as an administrator and run the following command:

```
PS C:\> wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/Windows/AWSCloudHSMJCE-latest.msi -Outfile C:\AWSCloudHSMJCE-latest.msi
```

```
PS C:\> Start-Process msiexec.exe -ArgumentList '/i C:\AWSCloudHSMJCE-latest.msi /quiet /norestart /log C:\client-install.txt' -Wait
```

------
#### [ Windows Server 2019 ]

For Windows Server 2019 on x86\_64 architecture, open PowerShell as an administrator and run the following command:

```
PS C:\> wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/Windows/AWSCloudHSMJCE-latest.msi -Outfile C:\AWSCloudHSMJCE-latest.msi
```

```
PS C:\> Start-Process msiexec.exe -ArgumentList '/i C:\AWSCloudHSMJCE-latest.msi /quiet /norestart /log C:\client-install.txt' -Wait
```

------

You must bootstrap Client SDK 5\. For more information, see [Bootstrap the Client SDK](cluster-connect.md#connect-how-to)\.

After you run the preceding commands, you can find the following JCE provider files:

------
#### [ Linux ]
+ `/opt/cloudhsm/java/cloudhsm-version.jar`
+ `/opt/cloudhsm/bin/configure-jce`
+ `/opt/cloudhsm/bin/jce-info`

------
#### [ Windows ]
+ `C:\Program Files\Amazon\CloudHSM\java\cloudhsm-version.jar>`
+ `C:\Program Files\Amazon\CloudHSM\bin\configure-jce.exe`
+ `C:\Program Files\Amazon\CloudHSM\bin\jce_info.exe`

------

## Provide credentials to the JCE provider<a name="java-library-credentials_5"></a>

Before your Java application can use an HSM, the HSM needs to first authenticate the application\. HSMs authenticate using either an explicit login or implicit login method\.

**Explicit login** – This method lets you provide AWS CloudHSM credentials directly in the application\. It uses the method from the [https://docs.oracle.com/javase/8/docs/api/java/security/AuthProvider.html](https://docs.oracle.com/javase/8/docs/api/java/security/AuthProvider.html), where you pass a CU username and password in the pin pattern\. For more information, see [Login to an HSM](https://github.com/aws-samples/aws-cloudhsm-jce-examples/blob/sdk5/src/main/java/com/amazonaws/cloudhsm/examples/LoginRunner.java) code example\.

**Implicit login** – This method lets you set AWS CloudHSM credentials either in a new property file, system properties, or as environment variables\.
+ **System properties** – Set credentials through system properties when running your application\. The following examples show two different ways that you can do this:

------
#### [ Linux ]

  ```
  $ java -DHSM_USER=<HSM user name> -DHSM_PASSWORD=<password>
  ```

  ```
  System.setProperty("HSM_USER","<HSM user name>");
  System.setProperty("HSM_PASSWORD","<password>");
  ```

------
#### [ Windows ]

  ```
  PS C:\> java -DHSM_USER=<HSM user name> -DHSM_PASSWORD=<password>
  ```

  ```
  System.setProperty("HSM_USER","<HSM user name>");
  System.setProperty("HSM_PASSWORD","<password>");
  ```

------
+ **Environment variables** – Set credentials as environment variables\.

------
#### [ Linux ]

  ```
  $ export HSM_USER=<HSM user name>
  $ export HSM_PASSWORD=<password>
  ```

------
#### [ Windows ]

  ```
  PS C:\> $Env:HSM_USER="<HSM user name>"
  PS C:\> $Env:HSM_PASSWORD="<password>"
  ```

------

Credentials might not be available if the application does not provide them or if you attempt an operation before the HSM authenticates session\. In those cases, the CloudHSM software library for Java searches for the credentials in the following order:

1. System properties

1. Environment variables

## Key management basics in the JCE provider<a name="java-library-key-basics_5"></a>

The basics on key management in the JCE provider involve importing keys, exporting keys, loading keys by handle, or deleting keys\. For more information on managing keys, see the [Manage keys](https://github.com/aws-samples/aws-cloudhsm-jce-examples/blob/sdk5/src/main/java/com/amazonaws/cloudhsm/examples/KeyUtilitiesRunner.java) code example\.

You can also find more JCE provider code examples at [Java samples](java-samples_5.md)\.