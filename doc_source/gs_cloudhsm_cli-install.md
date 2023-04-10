# Install and configure CloudHSM CLI<a name="gs_cloudhsm_cli-install"></a>

To interact with the HSM in your AWS CloudHSM cluster, you need the CloudHSM CLI\. 

**Topics**
+ [Install the AWS CloudHSM client and command line tools](#gs_cloudhsm_cli-install-command)

## Install the AWS CloudHSM client and command line tools<a name="gs_cloudhsm_cli-install-command"></a>

Connect to your client instance and run the following commands to download and install the AWS CloudHSM client and command line tools\. For more information, see [Launch an Amazon EC2 client instance](launch-client-instance.md)\.

Use the following commands to download and install the CloudHSM CLI\. 

------
#### [ Amazon Linux ]

Amazon Linux on x86\_64 architecture:

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL6/cloudhsm-cli-latest.el6.x86_64.rpm
```

```
$ sudo yum install ./cloudhsm-cli-latest.el6.x86_64.rpm
```

------
#### [ Amazon Linux 2 ]

Amazon Linux 2 on x86\_64 architecture:

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL7/cloudhsm-cli-latest.el7.x86_64.rpm
```

```
$ sudo yum install ./cloudhsm-cli-latest.el7.x86_64.rpm
```

Amazon Linux 2 on ARM64 architecture:

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL7/cloudhsm-cli-latest.el7.aarch64.rpm
```

```
$ sudo yum install ./cloudhsm-cli-latest.el7.aarch64.rpm
```

------
#### [ CentOS 7 ]

CentOS 7 on x86\_64 architecture:

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL7/cloudhsm-cli-latest.el7.x86_64.rpm
```

```
$ sudo yum install ./cloudhsm-cli-latest.el7.x86_64.rpm
```

------
#### [ CentOS 8 ]

CentOS 8 on x86\_64 architecture:

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL8/cloudhsm-cli-latest.el8.x86_64.rpm
```

```
$ sudo yum install ./cloudhsm-cli-latest.el8.x86_64.rpm
```

------
#### [ RHEL 7 ]

RHEL 7 on x86\_64 architecture:

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL7/cloudhsm-cli-latest.el7.x86_64.rpm
```

```
$ sudo yum install ./cloudhsm-cli-latest.el7.x86_64.rpm
```

------
#### [ RHEL 8 ]

RHEL 8 on x86\_64 architecture:

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL8/cloudhsm-cli-latest.el8.x86_64.rpm
```

```
$ sudo yum install ./cloudhsm-cli-latest.el8.x86_64.rpm
```

------
#### [ Ubuntu 18\.04 LTS ]

Ubuntu 18\.04 LTS on x86\_64 architecture:

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/Bionic/cloudhsm-cli_latest_u18.04_amd64.deb
```

```
$ sudo apt install ./cloudhsm-cli_latest_u18.04_amd64.deb
```

------
#### [ Ubuntu 20\.04 LTS ]

Ubuntu 20\.04 LTS on x86\_64 architecture:

```
$ wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/Focal/cloudhsm-cli_latest_u20.04_amd64.deb
```

```
$ sudo apt install ./cloudhsm-cli_latest_u20.04_amd64.deb
```

------
#### [ Windows Server 2016 ]

For Windows Server 2016 on x86\_64 architecture, open PowerShell as an administrator and run the following command:

```
PS C:\> wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/Windows/AWSCloudHSMCLI-latest.msi -Outfile C:\AWSCloudHSMCLI-latest.msi
```

```
PS C:\> Start-Process msiexec.exe -ArgumentList '/i C:\AWSCloudHSMCLI-latest.msi /quiet /norestart /log C:\client-install.txt' -Wait
```

------
#### [ Windows Server 2019 ]

For Windows Server 2019 on x86\_64 architecture, open PowerShell as an administrator and run the following command:

```
PS C:\> wget https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/Windows/AWSCloudHSMCLI-latest.msi -Outfile C:\AWSCloudHSMCLI-latest.msi
```

```
PS C:\> Start-Process msiexec.exe -ArgumentList '/i C:\AWSCloudHSMCLI-latest.msi /quiet /norestart /log C:\client-install.txt' -Wait
```

------

Use the following commands to configure CloudHSM CLI\.

**To bootstrap a Linux EC2 instance for Client SDK 5**
+  Use the configure tool to specify the IP address of a HSM in your cluster\. 

  ```
  $ sudo /opt/cloudhsm/bin/configure-cli -a <HSM IP address>
  ```

**To bootstrap a Windows EC2 instance for Client SDK 5**
+  Use the configure tool to specify the IP address of a HSM in your cluster\. 

  ```
  C:\Program Files\Amazon\CloudHSM\bin\ .\configure-cli.exe -a <HSM IP address>
  ```