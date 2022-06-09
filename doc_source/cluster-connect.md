# Connect the client SDK to the AWS CloudHSM cluster<a name="cluster-connect"></a>

 To connect to the cluster with either Client SDK 5 or Client SDK 3, you must first do two things: 
+ Have an issuing certificate in place on the EC2 instance
+ Bootstrap the Client SDK to the cluster

## Place the issuing certificate<a name="place-hsm-cert"></a>

 You create the issuing certificate when you initialize the cluster\. Copy the issuing certificate to the default location for the platform on each EC2 instance that connects to the cluster\. 

Linux

```
/opt/cloudhsm/etc/customerCA.crt
```

Windows

```
C:\ProgramData\Amazon\CloudHSM\customerCA.crt
```

With Client SDK 5 you can use the configure tool to specify the location of the issuing certificate\. 

------
#### [ PKCS \#11 library ]

**To place the issuing certificate on Linux for Client SDK 5**
+  Use the configure tool to specify a location for the issuing certificate\. 

  ```
  sudo /opt/cloudhsm/bin/configure-pkcs11 --hsm-ca-cert <customerCA certificate file>
  ```

**To place the issuing certificate on Windows for Client SDK 5**
+  Use the configure tool to specify a location for the issuing certificate\. 

  ```
  C:\Program Files\Amazon\CloudHSM\configure-pkcs11.exe --hsm-ca-cert <customerCA certificate file>
  ```

------
#### [ OpenSSL Dynamic Engine ]

**To place the issuing certificate on Linux for Client SDK 5**
+  Use the configure tool to specify a location for the issuing certificate\. 

  ```
  sudo /opt/cloudhsm/bin/configure-dyn --hsm-ca-cert <customerCA certificate file>
  ```

------
#### [ JCE provider ]

**To place the issuing certificate on Linux for Client SDK 5**
+  Use the configure tool to specify a location for the issuing certificate\. 

  ```
  sudo /opt/cloudhsm/bin/configure-jce --hsm-ca-cert <customerCA certificate file>
  ```

**To place the issuing certificate on Windows for Client SDK 5**
+  Use the configure tool to specify a location for the issuing certificate\. 

  ```
  C:\Program Files\Amazon\CloudHSM\configure-jce.exe --hsm-ca-cert <customerCA certificate file>
  ```

------

For more information, see [Configure Tool](configure-sdk-5.md)\.

For more information about initializing the cluster or creating and signing the certificate, see [Initilize the Cluster](initialize-cluster.md#initialize)\. 

## Bootstrap the Client SDK<a name="connect-how-to"></a>

The bootstrap process is different depending on the version of the Client SDK you're using, but you must have the IP address of one of the hardware security modules \(HSM\) in the cluster\. You can use the IP address of any HSM attached to your cluster\. After the Client SDK connects, it retrieves the IP addresses of any additional HSMs and performs load balancing and client\-side key synchronization operations\. 

### To get an IP address for the cluster<a name="connect-get-ip"></a>

**To get an IP address for a HSM \(console\)**

1. Open the AWS CloudHSM console at [https://console\.aws\.amazon\.com/cloudhsm/](https://console.aws.amazon.com/cloudhsm/)\.

1. To change the AWS Region, use the Region selector in the upper\-right corner of the page\.

1. In the navigation pane, choose **Clusters**\.

1. To open the cluster detail page, in the cluster table, choose the cluster ID\.

1. To get the IP address, on the HSMs tab, choose one of the IP addresses listed under **ENI IP address**\. 

**To get an IP address for a HSM \(AWS CLI\)**
+ Get the IP address of an HSM by using the [describe\-clusters](https://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/describe-clusters.html) command from the AWS CLI\. In the output from the command, the IP address of the HSMs are the values of `EniIp`\. 

  ```
  $  aws cloudhsmv2 describe-clusters
  	
  
  {
      "Clusters": [
          { ... }
              "Hsms": [
                  {
  ...
                      "EniIp": "10.0.0.9",
  ...
                  },
                  {
  ...
                      "EniIp": "10.0.1.6",
  ...
  ```

For more information about bootstrapping, see [Configure Tool](configure-tool.md)\.

### To bootstrap Client SDK 5<a name="sdk8-connect"></a>

------
#### [ PKCS \#11 library ]

**To bootstrap a Linux EC2 instance for Client SDK 5**
+  Use the configure tool to specify the IP address of a HSM in your cluster\. 

  ```
  sudo /opt/cloudhsm/bin/configure-pkcs11 -a <HSM IP address>
  ```

**To bootstrap a Windows EC2 instance for Client SDK 5**
+  Use the configure tool to specify the IP address of a HSM in your cluster\. 

  ```
  C:\Program Files\Amazon\CloudHSM\bin\configure-pkcs11.exe -a <HSM IP address>
  ```

------
#### [ OpenSSL Dynamic Engine ]

**To bootstrap a Linux EC2 instance for Client SDK 5**
+  Use the configure tool to specify the IP address of a HSM in your cluster\. 

  ```
  sudo /opt/cloudhsm/bin/configure-dyn -a <HSM IP address>
  ```

------
#### [ JCE provider ]

**To bootstrap a Linux EC2 instance for Client SDK 5**
+  Use the configure tool to specify the IP address of a HSM in your cluster\. 

  ```
  sudo /opt/cloudhsm/bin/configure-jce -a <HSM IP address>
  ```

**To bootstrap a Windows EC2 instance for Client SDK 5**
+  Use the configure tool to specify the IP address of a HSM in your cluster\. 

  ```
  C:\Program Files\Amazon\CloudHSM\bin\configure-jce.exe -a <HSM IP address>
  ```

------

### To bootstrap Client SDK 3<a name="sdk3-connect"></a>

**To bootstrap a Linux EC2 instance for Client SDK 3**
+ Use configure to specify the IP address of a HSM in your cluster\. 

  ```
  sudo /opt/cloudhsm/bin/configure -a <IP address>
  ```

**To bootstrap a Windows EC2 instance for Client SDK 3**
+ Use configure to specify the IP address of a HSM in your cluster\. 

  ```
  C:\Program Files\Amazon\CloudHSM\bin\configure-jce.exe -a <HSM IP address>
  ```

For more information about configure, see [Configure tool](configure-tool.md)\.