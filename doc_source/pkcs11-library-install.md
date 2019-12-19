# Installing the AWS CloudHSM Software Library for PKCS \#11<a name="pkcs11-library-install"></a>

With the AWS CloudHSM software libraries for PKCS \#11, you can to build PKCS \#11–compatible applications that use the HSMs in your AWS CloudHSM cluster\. You can use the standard AWS CloudHSM PKCS \#11 library or the AWS CloudHSM PKCS \#11 library that uses a [Redis cache](#install-pkcs11-redis)\. Both of the AWS CloudHSM libraries for PKCS \#11 are supported only on Linux operating systems\.

**Topics**
+ [Prerequisites](#pkcs11-library-prerequisites)
+ [Install the PKCS \#11 Library](#install-pkcs11-library)
+ [Install the PKCS \#11 Library with Redis \(Not applicable for SDK version 3\.0 or higher\)](#install-pkcs11-redis)

## Prerequisites<a name="pkcs11-library-prerequisites"></a>

The AWS CloudHSM software libraries for PKCS \#11 require the AWS CloudHSM client\.

If you haven't installed and configured the AWS CloudHSM client, do that now by following the steps at [Install the Client \(Linux\)](install-and-configure-client-linux.md)\. After you install and configure the client, use the following command to start it\. 

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
#### [ CentOS 6 ]

```
$ sudo start cloudhsm-client
```

------
#### [ CentOS 7 ]

```
$ sudo service cloudhsm-client start
```

------
#### [ RHEL 6 ]

```
$ sudo start cloudhsm-client
```

------
#### [ RHEL 7 ]

```
$ sudo service cloudhsm-client start
```

------
#### [ Ubuntu 16\.04 LTS ]

```
$ sudo service cloudhsm-client start
```

------

## Install the PKCS \#11 Library<a name="install-pkcs11-library"></a>

The following command downloads and installs \(or updates\) the AWS CloudHSM software library for PKCS \#11\. This step is required for the standard AWS CloudHSM PKCS \#11 library and the [AWS CloudHSM PKCS \#11 library with Redis](#install-pkcs11-redis)\.

------
#### [ Amazon Linux ]

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL6/cloudhsm-client-pkcs11-latest.el6.x86_64.rpm
```

```
$ sudo yum install -y ./cloudhsm-client-pkcs11-latest.el6.x86_64.rpm
```

------
#### [ Amazon Linux 2 ]

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL7/cloudhsm-client-pkcs11-latest.el7.x86_64.rpm
```

```
$ sudo yum install -y ./cloudhsm-client-pkcs11-latest.el7.x86_64.rpm
```

------
#### [ CentOS 6 ]

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL6/cloudhsm-client-pkcs11-latest.el6.x86_64.rpm
```

```
$ sudo yum install -y ./cloudhsm-client-pkcs11-latest.el6.x86_64.rpm
```

------
#### [ CentOS 7 ]

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL7/cloudhsm-client-pkcs11-latest.el7.x86_64.rpm
```

```
$ sudo yum install -y ./cloudhsm-client-pkcs11-latest.el7.x86_64.rpm
```

------
#### [ RHEL 6 ]

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL6/cloudhsm-client-pkcs11-latest.el6.x86_64.rpm
```

```
$ sudo yum install -y ./cloudhsm-client-pkcs11-latest.el6.x86_64.rpm
```

------
#### [ RHEL 7 ]

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL7/cloudhsm-client-pkcs11-latest.el7.x86_64.rpm
```

```
$ sudo yum install -y ./cloudhsm-client-pkcs11-latest.el7.x86_64.rpm
```

------
#### [ Ubuntu 16\.04 LTS ]

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/Xenial/cloudhsm-client-pkcs11_latest_amd64.deb
```

```
$ sudo dpkg -i cloudhsm-client-pkcs11_latest_amd64.deb
```

------

When the installation succeeds, the PKCS \#11 libraries are `/opt/cloudhsm/lib`\.

## Install the PKCS \#11 Library with Redis \(Not applicable for SDK version 3\.0 or higher\)<a name="install-pkcs11-redis"></a>

Prior to version 3\.0, AWS CloudHSM provided an optional software library for PKCS \#11 that uses a [Redis cache](https://redis.io/)\. The cache stores key handles and attributes locally so you can access them without calling into your HSMs\. 

**Note**  
Redis is not necessary for version 3\.0 or later\. If you are using AWS CloudHSM version 2\.0\.4 or earlier, AWS CloudHSM provides an optional software library for PKCS \#11 that uses a Redis cache\. The cache stores key handles and attributes locally so you can access them without calling into your HSMs\. 

When you build the cache, you specify the crypto user \(CU\) that your [PKCS \#11 application uses to be authenticated](pkcs11-pin.md)\. The cache is preloaded with the keys that the CU owns and shares\. It is automatically updated when your application uses functions in the PKCS \#11 library to make changes in the HSMs\. Examples include creating or deleting keys or changing keys attributes\. The cache is not aware of any other keys on the HSM\.

Caching can improve the performance of your PKCS \#11 application, but it might not be the right choice for all applications\. Consider the following:
+ Redis caches all PKCS \#11 library operations that run on the host, but it's not aware of operations that are performed outside the library\. For example, if you use the [command line tools](command-line-tools.md) or the [software library for Java](java-library.md) to manage keys in your HSMs, those operations do not update the cache\. You can rebuild the cache to update it to the new state of the HSMs, but the cache is not synchronized with the HSMs automatically\.

   
+ If you have other applications that use Redis on the same host, don't use the PKCS \#11 library with Redis\. The PKCS \#11 library configures Redis to recognize it as the only Redis consumer on the host\. 

To install the PKCS \#11 Library with Redis use the Extra Packages for Enterprise Linux \(EPEL\) repository\. Then enable and configure Redis to work with AWS CloudHSM and PKCS \#11\. 

Some steps in this process are required only on selected operating systems\. 

### Step 1: Install the AWS CloudHSM PKCS \#11 Library<a name="redis-install-pkcs11"></a>

To install the PKCS \#11 Library with Redis, you must first [install the standard AWS CloudHSM PKCS \#11 library](#install-pkcs11-library)\. This library is required\.

**Required for Redis on:** All supported operating systems\.

### Step 2: Install the EPEL Repository<a name="redis-install-epel"></a>

This step installs the Extra Packages for Enterprise Linux \(EPEL\) repository\. It is required only on operating systems that do not include EPEL\.

**Required for Redis only on:** Amazon Linux 2, CentOS 6, CentOS 7, Red Hat Enterprise Linux \(RHEL\) 6, Red Hat Enterprise Linux \(RHEL\) 7

------
#### [ Amazon Linux ]

No action required\.

------
#### [ Amazon Linux 2 ]

1. Download the EPEL repository\.

   ```
   curl -O https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
   ```

1. Install the EPEL repository\.

   ```
   sudo yum install epel-release-latest-7.noarch.rpm
   ```

------
#### [ CentOS 6 ]

1. Download the EPEL repository\.

   ```
   curl -O https://dl.fedoraproject.org/pub/epel/epel-release-latest-6.noarch.rpm
   ```

1. Install the EPEL repository\.

   ```
   sudo yum install epel-release-latest-6.noarch.rpm
   ```

------
#### [ CentOS 7 ]

1. Download the EPEL repository\.

   ```
   curl -O https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
   ```

1. Install the EPEL repository\.

   ```
   sudo yum install epel-release-latest-7.noarch.rpm
   ```

------
#### [ RHEL 6 ]

1. Download the EPEL repository\.

   ```
   curl -O https://dl.fedoraproject.org/pub/epel/epel-release-latest-6.noarch.rpm
   ```

1. Install the EPEL repository\.

   ```
   sudo yum install epel-release-latest-6.noarch.rpm
   ```

------
#### [ RHEL 7 ]

1. Download the EPEL repository\.

   ```
   curl -O https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
   ```

1. Install the EPEL repository\.

   ```
   sudo yum install epel-release-latest-7.noarch.rpm
   ```

------
#### [ Ubuntu 16\.04 LTS ]

No action required\.

------

### Step 3: Prepare for Redis<a name="redis-os-specific"></a>

This step includes system\-specific tasks that must be completed before you install and configure the PKCS \#11 library for Redis\.

**Required for Redis only on:** Amazon Linux, CentOS 7, Red Hat Enterprise Linux \(RHEL\) 6

------
#### [ Amazon Linux ]

This step enables the EPEL repository\. It is required only on Amazon Linux, but you can use this procedure to verify that EPEL is enabled on any Linux operating system\.

1. Open the `/etc/yum.repos.d/epel.repo` file in a text editor\. This step requires administrative permissions \(`sudo`\)\.

1. In the `[epel]` configuration in the file, set the value of `enabled` to `1`, as shown in the following example\. Then, save the file and close it\.

   ```
   [epel]
   name=Extra Packages for Enterprise Linux 6 - $basearch
   #baseurl=http://download.fedoraproject.org/pub/epel/6/$basearch
   mirrorlist=https://mirrors.fedoraproject.org/metalink?repo=epel-6&arch=$basearch
   failovermethod=priority
   enabled=1
   gpgcheck=1
   gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-6
   ```

------
#### [ Amazon Linux 2 ]

No action required\.

------
#### [ CentOS 6 ]

No action required\.

------
#### [ CentOS 7 ]

This command disables a [Security\-Enhanced Linux](https://selinuxproject.org/) \(SELinux\) policy that prevents Redis from using AWS CloudHSM resources\. 

```
sudo semanage module -d redis
```

------
#### [ RHEL 6 ]

This command eliminates the TTY requirement in the `sudoers` file\. The `sudoers` file contains the rules for the `sudo` command\.

1. Use [visudo](https://www.sudo.ws/man/1.8.17/visudo.man.html) editor to edit the `/etc/sudoers` file\.

1. Comment out the `Defaults requiretty` statement\. Then, save the file and quit `visudo`\.

   ```
   # Defaults requiretty
   ```

------
#### [ RHEL 7 ]

No action required\.

------
#### [ Ubuntu 16\.04 LTS ]

No action required\.

------

### Step 4: Install, Configure, and Build the Redis Cache<a name="redis-build-keystore"></a>

Use the following procedure to install and configure the Redis package for the AWS CloudHSM library for PKCS \#11 and build the cache\. 

**Required for Redis on:** All supported operating systems\.

1. Use the `setup_redis` script to install Redis and configure it to work with the AWS CloudHSM PKCS \#11 library for Redis\.

   ```
   $ sudo /opt/cloudhsm/bin/setup_redis
   ```

1. Start the Redis service\.

   ```
   $ sudo service redis start
   ```

1. Use the `build_keystore` command to build the Redis cache\. Type the name and password of the [crypto user \(CU\)](hsm-users.md#crypto-user) that your [PKCS \#11 application uses for authentication](pkcs11-pin.md)\.

   The cache is preloaded with the keys that the specified CU owns and shares\. It is updated automatically when your application makes changes in the HSMs on behalf of the CU\. Examples include creating or deleting keys, or changing key attributes\. The cache is not aware of any other keys on the HSMs\.

   ```
   $ /opt/cloudhsm/bin/build_keystore -s <CU user name> -p < CU password>
   ```