# Upgrading Client SDK 3 on Linux<a name="client-upgrade"></a>

With AWS CloudHSM Client SDK 3\.1 and higher, the version of the client daemon and any components you install must match to upgrade\. For all Linux\-based systems, you must use a single command to batch upgrade the client daemon with the same version of the PKCS \#11 library, the Java Cryptographic Extension \(JCE\) provider, or the OpenSSL Dynamic Engine\. This requirement does not apply to Windows\-based systems because the binaries for the CNG and KSP providers are already included in the client daemon package\. 

## To check the client daemon version<a name="check-client-version"></a>
+ On a Red Hat\-based Linux system \(including Amazon Linux and CentOS\), use the following command:

  ```
  rpm -qa | grep ^cloudhsm
  ```
+ On an Debian\-based Linux system, use the following command:

  ```
  apt list --installed | grep ^cloudhsm
  ```
+ On a Windows system, use the following command:

  ```
  wmic product get name,version
  ```

**Topics**
+ [Prerequisites](#client-upgrade-prerequisites)
+ [Step 1: Stop the client daemon](#client-stop)
+ [Step 2: Upgrade the client SDK](#upgrade-commands)
+ [Step 3: Start the client daemon](#client-start)

## Prerequisites<a name="client-upgrade-prerequisites"></a>

Download the latest version of AWS CloudHSM client daemon and choose your components\.

**Note**  
You do not have to install all the components\. For every component you have installed, you must upgrade that component to match the version of the client daemon\.

### Latest Linux client daemon<a name="download-client"></a>

------
#### [ Amazon Linux ]

```
wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL6/cloudhsm-client-latest.el6.x86_64.rpm
```

------
#### [ Amazon Linux 2 ]

```
wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL7/cloudhsm-client-latest.el7.x86_64.rpm
```

------
#### [ CentOS 7 ]

```
sudo yum install wget
```

```
wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL7/cloudhsm-client-latest.el7.x86_64.rpm
```

------
#### [ CentOS 8 ]

```
sudo yum install wget
```

```
wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL8/cloudhsm-client-latest.el8.x86_64.rpm
```

------
#### [ RHEL 7 ]

```
sudo yum install wget
```

```
wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL7/cloudhsm-client-latest.el7.x86_64.rpm
```

------
#### [ RHEL 8 ]

```
sudo yum install wget
```

```
wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL8/cloudhsm-client-latest.el8.x86_64.rpm
```

------
#### [ Ubuntu 16\.04 LTS ]

```
wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/Xenial/cloudhsm-client_latest_amd64.deb
```

------
#### [ Ubuntu 18\.04 LTS ]

```
wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/Bionic/cloudhsm-client_latest_u18.04_amd64.deb
```

------

### Latest PKCS \#11 library<a name="download-pkcs11"></a>

------
#### [ Amazon Linux ]

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL6/cloudhsm-client-pkcs11-latest.el6.x86_64.rpm
```

------
#### [ Amazon Linux 2 ]

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL7/cloudhsm-client-pkcs11-latest.el7.x86_64.rpm
```

------
#### [ CentOS 7 ]

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL7/cloudhsm-client-pkcs11-latest.el7.x86_64.rpm
```

------
#### [ CentOS 8 ]

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL8/cloudhsm-client-pkcs11-latest.el8.x86_64.rpm
```

------
#### [ RHEL 7 ]

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL7/cloudhsm-client-pkcs11-latest.el7.x86_64.rpm
```

------
#### [ RHEL 8 ]

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL8/cloudhsm-client-pkcs11-latest.el8.x86_64.rpm
```

------
#### [ Ubuntu 16\.04 LTS ]

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/Xenial/cloudhsm-client-pkcs11_latest_amd64.deb
```

------
#### [ Ubuntu 18\.04 LTS ]

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/Bionic/cloudhsm-client-pkcs11_latest_u18.04_amd64.deb
```

------

### Latest OpenSSL Dynamic Engine<a name="download-openssl"></a>

------
#### [ Amazon Linux ]

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL6/cloudhsm-client-dyn-latest.el6.x86_64.rpm
```

------
#### [ Amazon Linux 2 ]

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL7/cloudhsm-client-dyn-latest.el7.x86_64.rpm
```

------
#### [ CentOS 7 ]

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL7/cloudhsm-client-dyn-latest.el7.x86_64.rpm
```

------
#### [ RHEL 7 ]

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL7/cloudhsm-client-dyn-latest.el7.x86_64.rpm
```

------
#### [ Ubuntu 16\.04 LTS ]

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/Xenial/cloudhsm-client-dyn_latest_amd64.deb
```

------

### Latest JCE provider<a name="download-java"></a>

------
#### [ Amazon Linux ]

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL6/cloudhsm-client-jce-latest.el6.x86_64.rpm
```

------
#### [ Amazon Linux 2 ]

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL7/cloudhsm-client-jce-latest.el7.x86_64.rpm
```

------
#### [ CentOS 7 ]

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL7/cloudhsm-client-jce-latest.el7.x86_64.rpm
```

------
#### [ CentOS 8 ]

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL8/cloudhsm-client-jce-latest.el8.x86_64.rpm
```

------
#### [ RHEL 7 ]

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL7/cloudhsm-client-jce-latest.el7.x86_64.rpm
```

------
#### [ RHEL 8 ]

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL8/cloudhsm-client-jce-latest.el8.x86_64.rpm
```

------
#### [ Ubuntu 16\.04 LTS ]

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/Xenial/cloudhsm-client-jce_latest_amd64.deb
```

------
#### [ Ubuntu 18\.04 LTS ]

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/Bionic/cloudhsm-client-jce_latest_u18.04_amd64.deb
```

------

## Step 1: Stop the client daemon<a name="client-stop"></a>

Use the following command to stop the client daemon\.

------
#### [ Amazon Linux ]

```
$ sudo stop cloudhsm-client
```

------
#### [ Amazon Linux 2 ]

```
$ sudo service cloudhsm-client stop
```

------
#### [ CentOS 7 ]

```
$ sudo service cloudhsm-client stop
```

------
#### [ CentOS 8 ]

```
$ sudo service cloudhsm-client stop
```

------
#### [ RHEL 7 ]

```
$ sudo service cloudhsm-client stop
```

------
#### [ RHEL 8 ]

```
$ sudo service cloudhsm-client stop
```

------
#### [ Ubuntu 16\.04 LTS ]

```
$ sudo service cloudhsm-client stop
```

------
#### [ Ubuntu 18\.04 LTS ]

```
$ sudo service cloudhsm-client stop
```

------

## Step 2: Upgrade the client SDK<a name="upgrade-commands"></a>

The following command shows the syntax required to upgrade the client daemon and components\. Before you run the command, remove any components you don't intend to upgrade\.

------
#### [ Amazon Linux ]

```
$ sudo yum install ./cloudhsm-client-latest.el6.x86_64.rpm \
              <./cloudhsm-client-pkcs11-latest.el6.x86_64.rpm> \
              <./cloudhsm-client-dyn-latest.el6.x86_64.rpm> \
              <./cloudhsm-client-jce-latest.el6.x86_64.rpm>
```

------
#### [ Amazon Linux 2 ]

```
$ sudo yum install ./cloudhsm-client-latest.el7.x86_64.rpm \
              <./cloudhsm-client-pkcs11-latest.el7.x86_64.rpm> \
              <./cloudhsm-client-dyn-latest.el7.x86_64.rpm> \
              <./cloudhsm-client-jce-latest.el7.x86_64.rpm>
```

------
#### [ CentOS 7 ]

```
$ sudo yum install ./cloudhsm-client-latest.el7.x86_64.rpm \
              <./cloudhsm-client-pkcs11-latest.el7.x86_64.rpm> \
              <./cloudhsm-client-dyn-latest.el7.x86_64.rpm> \
              <./cloudhsm-client-jce-latest.el7.x86_64.rpm>
```

------
#### [ CentOS 8 ]

```
$ sudo yum install ./cloudhsm-client-latest.el8.x86_64.rpm \
              <./cloudhsm-client-pkcs11-latest.el8.x86_64.rpm> \              
              <./cloudhsm-client-jce-latest.el8.x86_64.rpm>
```

------
#### [ RHEL 7 ]

```
$ sudo yum install ./cloudhsm-client-latest.el7.x86_64.rpm \
              <./cloudhsm-client-pkcs11-latest.el7.x86_64.rpm> \
              <./cloudhsm-client-dyn-latest.el7.x86_64.rpm> \
              <./cloudhsm-client-jce-latest.el7.x86_64.rpm>
```

------
#### [ RHEL 8 ]

```
$ sudo yum install ./cloudhsm-client-latest.el8.x86_64.rpm \
              <./cloudhsm-client-pkcs11-latest.el8.x86_64.rpm> \
              <./cloudhsm-client-jce-latest.el8.x86_64.rpm>
```

------
#### [ Ubuntu 16\.04 LTS ]

```
$ sudo apt install ./cloudhsm-client_latest_amd64.deb \
              <cloudhsm-client-pkcs11_latest_amd64.deb> \
              <cloudhsm-client-dyn_latest_amd64.deb> \
              <cloudhsm-client-jce_latest_amd64.deb>
```

------
#### [ Ubuntu 18\.04 LTS ]

```
$ sudo apt install ./cloudhsm-client_latest_u18.04_amd64.deb \
              <cloudhsm-client-pkcs11_latest_amd64.deb> \
              <cloudhsm-client-jce_latest_amd64.deb>
```

------

## Step 3: Start the client daemon<a name="client-start"></a>

Use the following command to start the client daemon\.

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