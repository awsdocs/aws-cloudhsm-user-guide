# Verify the performance of the HSM<a name="troubleshooting-verify-hsm-performance"></a>

This topic describes how to verify HSM performance with Client SDK 3\.

To verify the performance of the HSMs in your AWS CloudHSM cluster, you can use the pkpspeed \(Linux\) or pkpspeed\_blocking \(Windows\) tool that is included with Client SDK 3\. For more information about installing the client on a Linux EC2 instance, see [Install and configure the AWS CloudHSM client \(Linux\)](cmu-install-and-configure-client-linux.md)\. For more information about installing the client on a Windows instance, see [Install and configure the AWS CloudHSM client \(Windows\)](cmu-install-and-configure-client-win.md)\. 

After you install and configure the AWS CloudHSM client, run the following command to start it\.

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
#### [ Windows ]
+ For Windows client 1\.1\.2\+:

  ```
  C:\Program Files\Amazon\CloudHSM>net.exe start AWSCloudHSMClient
  ```
+ For Windows clients 1\.1\.1 and older:

  ```
  C:\Program Files\Amazon\CloudHSM>start "cloudhsm_client" cloudhsm_client.exe C:\ProgramData\Amazon\CloudHSM\data\cloudhsm_client.cfg
  ```

------

If you have already installed the client software, you might need to download and install the latest version to get pkpspeed\. You can find the pkpspeed tool at `/opt/cloudhsm/bin/pkpspeed` in Linux or `C:\Program Files\Amazon\CloudHSM\` in Windows\. 

To use pkpspeed, run the pkpspeed command or pkpspeed\_blocking\.exe, specifying the user name and password of a crypto user \(CU\) on the HSM\. Then set the options to use while considering the following recommendations\. 

**Recommendations**  
To test the performance of RSA sign and verify operations, choose the `RSA_CRT` cipher in Linux or option B in Windows\. Don't choose `RSA` \(option A in Windows\)\. The ciphers are equivalent, but `RSA_CRT` is optimized for performance\. 
Start with a small number of threads\. For testing AES performance, one thread is typically enough to show maximum performance\. For testing RSA performance\(`RSA_CRT`\), three or four threads is typically enough\. 

The following examples show the options that you can choose with pkpspeed \(Linux\) or pkpspeed\_blocking \(Windows\) to test the HSM's performance for RSA and AES operations\. 

**Example – Using pkpspeed to test RSA performance**  
You can run this example on Windows, Linux, and compatible operating systems\.  
Use these instructions for Linux and compatible operating systems\.  

```
/opt/cloudhsm/bin/pkpspeed -s CU user name -p password

SDK Version: 2.03

        Available Ciphers:
                AES_128
                AES_256
                3DES
                RSA  (non-CRT. modulus size can be 2048/3072)
                RSA_CRT (same as RSA)
For RSA, Exponent will be 65537

Current FIPS mode is: 00002
Enter the number of thread [1-10]: 3
Enter the cipher: RSA_CRT
Enter modulus length: 2048
Enter time duration in Secs: 60
Starting non-blocking speed test using data length of 245 bytes...
[Test duration is 60 seconds]

Do you want to use static key[y/n] (Make sure that KEK is available)?n
```

```
c:\Program Files\Amazon\CloudHSM>pkpspeed_blocking.exe -s CU user name -p password

Please select the test you want to run

RSA non-CRT------------------->A
RSA CRT----------------------->B
Basic 3DES CBC---------------->C
Basic AES--------------------->D
FIPS Random------------------->H
Random------------------------>I
AES GCM ---------------------->K

eXit------------------------>X
B

Running 4 threads for 25 sec

Enter mod size(2048/3072):2048
Do you want to use Token key[y/n]n
Do you want to use static key[y/n] (Make sure that KEK is available)?  n
OPERATIONS/second                821/1
OPERATIONS/second                833/1
OPERATIONS/second                845/1
OPERATIONS/second                835/1
OPERATIONS/second                837/1
OPERATIONS/second                836/1
OPERATIONS/second                837/1
OPERATIONS/second                849/1
OPERATIONS/second                841/1
OPERATIONS/second                856/1
OPERATIONS/second                841/1
OPERATIONS/second                847/1
OPERATIONS/second                838/1
OPERATIONS/second                843/1
OPERATIONS/second                852/1
OPERATIONS/second                837/
```

**Example – Using pkpspeed to test AES performance**  
Use these instructions for Linux and compatible operating systems\.  

```
/opt/cloudhsm/bin/pkpspeed -s <CU user name> -p <password>

SDK Version: 2.03

        Available Ciphers:
                AES_128
                AES_256
                3DES
                RSA  (non-CRT. modulus size can be 2048/3072)
                RSA_CRT (same as RSA)
For RSA, Exponent will be 65537

Current FIPS mode is: 00000002
Enter the number of thread [1-10]: 1
Enter the cipher: AES_256
Enter the data size [1-16200]: 8192
Enter time duration in Secs: 60
Starting non-blocking speed test using data length of 8192 bytes...
```

```
c:\Program Files\Amazon\CloudHSM>pkpspeed_blocking.exe -s CU user name -p password
login as USER
Initializing Cfm2 library
        SDK Version: 2.03

 Current FIPS mode is: 00000002
Please enter the number of threads [MAX=400] : 1
Please enter the time in seconds to run the test [MAX=600]: 20


Please select the test you want to run

RSA non-CRT------------------->A
RSA CRT----------------------->B
Basic 3DES CBC---------------->C
Basic AES--------------------->D
FIPS Random------------------->H
Random------------------------>I
AES GCM ---------------------->K

eXit------------------------>X
D

Running 1 threads for 20 sec

Enter the key size(128/192/256):256
Enter the size of the packet in bytes[1-16200]:8192
OPERATIONS/second                9/1
OPERATIONS/second                10/1
OPERATIONS/second                11/1
OPERATIONS/second                10/1
OPERATIONS/second                10/1
OPERATIONS/second                10/...
```