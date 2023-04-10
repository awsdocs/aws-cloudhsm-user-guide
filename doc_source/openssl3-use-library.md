# Use the OpenSSL Dynamic Engine for Client SDK 3<a name="openssl3-use-library"></a>

To use the AWS CloudHSM dynamic engine for OpenSSL from an OpenSSL\-integrated application, ensure that your application uses the OpenSSL dynamic engine named `cloudhsm`\. The shared library for the dynamic engine is located at `/opt/cloudhsm/lib/libcloudhsm_openssl.so`\.

To use the AWS CloudHSM dynamic engine for OpenSSL from the OpenSSL command line, use the `-engine` option to specify the OpenSSL dynamic engine named `cloudhsm`\. For example:

```
$ openssl s_server -cert server.crt -key server.key -engine cloudhsm
```