# Key extraction using JCE<a name="java-lib-configs-getencoded"></a>

The Java Cryptography Extension \(JCE\) uses an architecture that allows different cryptography implementations to be plugged in\. AWS CloudHSM ships one such JCE provider that offloads cryptographic operations to the HSM\. For most other JCE providers to work with keys stored in AWS CloudHSM, they must extract the key bytes from your HSMs in clear text into your machine’s memory for their use\. HSMs typically only allow keys to be extracted as [wrapped objects](cloudhsm_using_trusted_keys_control_key_wrap.md), not clear text\. However, to support inter\-provider integration use cases, AWS CloudHSM allows an opt\-in configuration option to enable extraction of the key bytes in the clear\.

**Important**  
JCE offloads operations to AWS CloudHSM whenever the AWS CloudHSM provider is specified or an AWS CloudHSM key object is used\. You do not need to extract keys in clear if you expect your operation to happen inside the HSM\. Key extraction in clear text is only needed when your application cannot use secure mechanisms such as wrapping and unwrapping a key due to restrictions from a third party library or JCE provider\. 

The AWS CloudHSM JCE Provider allows extraction of **public keys** to work with external JCE providers by default\. The following methods are always allowed:


| Class | Method | Format \(getEncoded\) | 
| --- | --- | --- | 
| EcPublicKey | getEncoded\(\) | X\.509 | 
|  | getW\(\) | N/A | 
| RSAPublicKey | getEncoded\(\) | X\.509 | 
|  | getPublicExponent\(\) | N/A | 
| CloudHsmRsaPrivateCrtKey | getPublicExponent\(\) | N/A | 

## <a name="get-encoded-pre-reqs"></a>

The AWS CloudHSM JCE Provider doesn’t allow extraction of key bytes in clear for the **private** or **secret** keys by default\. If your use case requires it, you can enable extraction of key bytes in clear for **private** or **secret** keys under the following conditions:

1. The `EXTRACTABLE` attribute for private and secret keys is set to **true**\.
   + By default, the `EXTRACTABLE` attribute for private and secret keys is set to **true**\. `EXTRACTABLE` keys are keys that are permitted to be exported out of the HSM\. For more information see Supported Java attributes for [Client SDK 5]()\.

1. The `WRAP_WITH_TRUSTED` attribute for the private and secret keys is set to **false**\.
   + `getEncoded`, `getPrivateExponent`, and `getS` cannot be used with private keys that cannot be exported in clear\. `WRAP_WITH_TRUSTED` doesn't allow your private keys to exported out of the HSM in clear\. For more information see [Using trusted keys to control key unwraps]()\.

## Allowing the AWS CloudHSM JCE provider to extract private key secrets out of AWS CloudHSM<a name="get-encoded-take-out-private-keys"></a>

**Important**  
This configuration change allows extraction of all `EXTRACTABLE` key bytes in clear from your HSM cluster\. For better security, you should consider using [key wrapping methods]() to extract the key out of the HSM securely\. This prevents unintentional extraction of your key bytes from the HSM\. 

1. Use the following commands to enable your **private** or **secret** keys to be extracted in JCE:

------
#### [ Linux ]

   ```
   $ /opt/cloudhsm/bin/configure-jce --enable-clear-key-extraction-in-software
   ```

------
#### [ Windows ]

   ```
   C:\Program Files\Amazon\CloudHSM\> .\configure-jce.exe --enable-clear-key-extraction-in-software
   ```

------

1. Once you enable your clear key extraction, the following methods are enabled for extracting private keys into memory\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/java-lib-configs-getencoded.html)

If you want restore the default behavior and not allow JCE to export keys in clear, run the following command:

------
#### [ Linux ]

```
$ /opt/cloudhsm/bin/configure-jce --disable-clear-key-extraction-in-software
```

------
#### [ Windows ]

```
C:\Program Files\Amazon\CloudHSM\> .\configure-jce.exe --disable-clear-key-extraction-in-software
```

------