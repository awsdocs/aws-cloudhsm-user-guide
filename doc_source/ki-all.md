# Known issues for all HSM instances<a name="ki-all"></a>

The following issues impact all AWS CloudHSM users regardless of whether they use the key\_mgmt\_util command line tool, the PKCS \#11 SDK, the JCE SDK, or the OpenSSL SDK\. 

**Topics**
+ [Issue: AES key wrapping uses PKCS \#5 padding instead of providing a standards\-compliant implementation of key wrap with zero padding](#ki-all-1)
+ [Issue: The client daemon requires at least one valid IP address in its configuration file to successfully connect to the cluster](#ki-all-2)
+ [Issue: There was an upper limit of 16 KB on data that can be hashed and signed by AWS CloudHSM](#ki-all-3)
+ [Issue: Imported keys could not be specified as nonexportable](#ki-all-4)
+ [Issue: The default mechanism for the wrapKey and unWrapKey commands in the key\_mgmt\_util has been removed](#ki-all-5)
+ [Issue: If you have a single HSM in your cluster, HSM failover does not work correctly](#ki-all-6)
+ [Issue: If you exceed the key capacity of the HSMs in your cluster within a short period of time, the client enters an unhandled error state](#ki-all-7)
+ [Issue: Digest operations with HMAC keys of size greater than 800 bytes are not supported](#ki-all-8)
+ [Issue: The client\_info tool, distributed with Client SDK 3, deletes the contents of the path specified by the optional output argument](#ki-all-9)
+ [Issue: You receive an error when running the SDK 5 configure tool using the `--cluster-id` argument in containerized environments](#ki-all-10)

## Issue: AES key wrapping uses PKCS \#5 padding instead of providing a standards\-compliant implementation of key wrap with zero padding<a name="ki-all-1"></a>

Additionally, key wrap with no padding and zero padding is not supported\.
+ **Impact: **There is no impact if you wrap and unwrap using this algorithm within AWS CloudHSM\. However, keys wrapped with AWS CloudHSM cannot be unwrapped within other HSMs or software that expects compliance to the no\-padding specification\. This is because eight bytes of padding data might be added to the end of your key data during a standards\-compliant unwrap\. Externally wrapped keys cannot be properly unwrapped into an AWS CloudHSM instance\. 
+ **Workaround: **To externally unwrap a key that was wrapped with AES Key Wrap with PKCS \#5 Padding on an AWS CloudHSM instance, strip the extra padding before you attempt to use the key\. You can do this by trimming the extra bytes in a file editor or copying only the key bytes into a new buffer in your code\. 
+ **Resolution status: **With the 3\.1\.0 client and software release, AWS CloudHSM provides standards\-compliant options for AES key wrapping\. For more information, see [AES Key Wrapping](manage-aes-key-wrapping.md)\. 

## Issue: The client daemon requires at least one valid IP address in its configuration file to successfully connect to the cluster<a name="ki-all-2"></a>
+ **Impact: **If you delete every HSM in your cluster and then add another HSM, which gets a new IP address, the client daemon continues to search for your HSMs at their original IP addresses\. 
+ **Workaround: **If you run an intermittent workload, we recommend that you use the `IpAddress` argument in the [CreateHsm](https://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_CreateHsm.html) function to set the elastic network interface \(ENI\) to its original value\. Note than an ENI is specific to an Availability Zone \(AZ\)\. The alternative is to delete the `/opt/cloudhsm/daemon/1/cluster.info` file and then reset the client configuration to the IP address of your new HSM\. You can use the `client -a <IP address>` command\. For more information, see [Install and Configure the AWS CloudHSM Client \(Linux\)](cmu-install-and-configure-client-linux.md) or [Install and Configure the AWS CloudHSM Client \(Windows\)](cmu-install-and-configure-client-win.md)\.

## Issue: There was an upper limit of 16 KB on data that can be hashed and signed by AWS CloudHSM<a name="ki-all-3"></a>
+ **Resolution status: **Data less than 16KB in size continues to be sent to the HSM for hashing\. We have added capability to hash locally, in software, data between 16KB and 64KB in size\. The client and the SDKs will explicitly fail if the data buffer is larger than 64KB\. You must update your client and SDK\(s\) to version 1\.1\.1 or higher to benefit from the fix\. 

## Issue: Imported keys could not be specified as nonexportable<a name="ki-all-4"></a>
+ **Resolution Status: **This issue is fixed\. No action is required on your part to benefit from the fix\.

## Issue: The default mechanism for the wrapKey and unWrapKey commands in the key\_mgmt\_util has been removed<a name="ki-all-5"></a>
+ **Resolution: **When using the wrapKey or unWrapKey commands, you must use the `-m` option to specify the mechanism\. See the examples in the [wrapKey](key_mgmt_util-wrapKey.md) or [unWrapKey](key_mgmt_util-unwrapKey.md) articles for more information\. 

## Issue: If you have a single HSM in your cluster, HSM failover does not work correctly<a name="ki-all-6"></a>
+ **Impact: **If the single HSM instance in your cluster loses connectivity, the client will not reconnect with it even if the HSM instance is later restored\.
+ **Workaround: **We recommend at least two HSM instances in any production cluster\. If you use this configuration, you will not be impacted by this issue\. For single\-HSM clusters, bounce the client daemon to restore connectivity\.
+ **Resolution status: ** This issue has been resolved in the AWS CloudHSM client 1\.1\.2 release\. You must upgrade to this client to benefit from the fix\.

## Issue: If you exceed the key capacity of the HSMs in your cluster within a short period of time, the client enters an unhandled error state<a name="ki-all-7"></a>
+ **Impact: ** When the client encounters the unhandled error state, it freezes and must be restarted\.
+ **Workaround: **Test your throughput to ensure you are not creating session keys at a rate that the client is unable to handle\. You can lower your rate by adding an HSM to the cluster or slowing down the session key creation\.
+ **Resolution status: ** This issue has been resolved in the AWS CloudHSM client 1\.1\.2 release\. You must upgrade to this client to benefit from the fix\.

## Issue: Digest operations with HMAC keys of size greater than 800 bytes are not supported<a name="ki-all-8"></a>
+ **Impact: **HMAC keys larger than 800 bytes can be generated on or imported into the HSM\. However, if you use this larger key in a digest operation via the JCE or key\_mgmt\_util, the operation will fail\. Note that if you are using PKCS11, HMAC keys are limited to a size of 64 bytes\.
+ **Workaround: **If you will be using HMAC keys for digest operations on the HSM, ensure the size is smaller than 800 bytes\.
+ **Resolution status: **None at this time\.

## Issue: The client\_info tool, distributed with Client SDK 3, deletes the contents of the path specified by the optional output argument<a name="ki-all-9"></a>
+ **Impact: **All existing files and sub\-directories under the specified output path may be permanently lost\.
+ **Workaround: **Do not use the optional argument `-output path` when using the `client_info` tool\.
+ **Resolution status: **This issue has been resolved in the [Client SDK 3\.3\.2 release](https://docs.aws.amazon.com/cloudhsm/latest/userguide/client-history.html#client-version-3-3-2)\. You must upgrade to this client to benefit from the fix\.

## Issue: You receive an error when running the SDK 5 configure tool using the `--cluster-id` argument in containerized environments<a name="ki-all-10"></a>

You receive the following error when using the \-\-cluster\-id argument with the Configure Tool:

`No credentials in the property bag`

This error is caused by an update to Instance Metadata Service Version 2 \(IMDSv2\)\. For more information, see the [IMDSv2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/configuring-instance-metadata-service.html) documentation\.
+ **Impact:** This issue will impact users running the configure tool on SDK versions 5\.5\.0 and later in containerized environments and utilizing EC2 instance metadata to provide credentials\.
+ **Workaround:** Set the PUT response hop limit to at least two\. for guidance on how to do this, see [ Configure the instance metadata options](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/configuring-instance-metadata-options.html)\.