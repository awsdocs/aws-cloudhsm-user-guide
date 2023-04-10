# Benefits of Client SDK 5<a name="client-sdk-5-benefits"></a>

Compared to Client SDK 3, Client SDK 5 is easier to manage, offers superior configurability, and increased reliability\. Client SDK 5 also provides some additional key advantages to Client SDK 3\. 

**Designed for serverless architecture**  
Client SDK 5 does not require a client daemon, so you no longer need to manage a background service\. This helps users in a few important ways:   
+ Simplifies the application startup process\. All you need to do to get started with CloudHSM is configure the SDK before running your application\.
+ You don't need a constantly running process, which makes integration with serverless components like Lambda and Elastic Container Service \(ECS\) easier\.

**Improved user experience and configurability**  
Client SDK 5 improves log message readability, which makes self\-service triaging much easier for users\. SDK 5 also offers a variety of configurations, which are listed in the [Configure Tool page](https://docs.aws.amazon.com/cloudhsm/latest/userguide/configure-sdk-5.html)\. 

**Broader platform support**  
Client SDK 5 offers more support for modern operating platforms\. This includes support for ARM technologies and greater support for [JCE](https://docs.aws.amazon.com/cloudhsm/latest/userguide/java-library_5.html), [PKCS\#11](https://docs.aws.amazon.com/cloudhsm/latest/userguide/pkcs11-library.html), and [OpenSSL](https://docs.aws.amazon.com/cloudhsm/latest/userguide/openssl-library.html)\. For more information, refer to the [Supported Platforms page](https://docs.aws.amazon.com/cloudhsm/latest/userguide/sdk8-support.html)\. 

**Topics**
+ [Migrating from Client SDK 3 to Client SDK 5](migrate-sdk.md)