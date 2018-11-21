# Authenticating to PKCS \#11<a name="pkcs11-pin"></a>

When you use PKCS \#11 with AWS CloudHSM, your application runs as a particular [crypto user \(CU\)](hsm-users.md) in your HSMs\. Your application can view and manage only the keys that the CU owns and shares\. You can use an existing CU in your HSMs or [create a new CU](manage-hsm-users.md#create-user) for your application\.

To specify the CU to PKCS \#11, use the pin parameter of the PKCS \#11 [C\_Login function](http://docs.oasis-open.org/pkcs11/pkcs11-base/v2.40/os/pkcs11-base-v2.40-os.html#_Toc385057915)\. For AWS CloudHSM, the pin parameter has the following format:

```
<CU_user_name>:<password>
```

For example, the following command sets the PKCS \#11 pin to the CU with user name `CryptoUser` and password `CUPassword123!`\.

```
CryptoUser:CUPassword123!
```