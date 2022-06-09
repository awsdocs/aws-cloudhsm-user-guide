# PKCS \#11 library<a name="pkcs11-library"></a>

PKCS \#11 is a standard for performing cryptographic operations on hardware security modules \(HSM\)\. AWS CloudHSM offers an implementations of the PKCS \#11 library in Client SDK 3 and Client SDK 5, both of which are compliant with PKCS \#11 version 2\.40\.

**Client SDK 5**  
Client SDK 5 does not require a client daemon and offers cross\-platform support on Windows and Linux\. 

**Client SDK 3**  
Client SDK 3 requires a client daemon to connect to the cluster and is only supported on Linux\.

**Topics**
+ [Installing](pkcs11-library-install.md)
+ [Authenticating](pkcs11-pin.md)
+ [Key types](pkcs11-key-types.md)
+ [Mechanisms](pkcs11-mechanisms.md)
+ [API operations](pkcs11-apis.md)
+ [Attributes](pkcs11-attributes.md)
+ [Samples](pkcs11-samples.md)