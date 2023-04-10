# Connecting to multiple clusters with the JCE provider<a name="java-lib-configs-multi"></a>

This configuration allows a single client instance to communicate to multiple clusters\. Compared to having a single instance only communicate with a single cluster, this can be a cost\-savings feature for some use cases\. The `CloudHsmProvider` class is AWS CloudHSM's implementation of [Java Security's Provider class](https://docs.oracle.com/javase/8/docs/api/java/security/Provider.html)\. Each instance of this class represents a connection to your entire AWS CloudHSM cluster\. You instantiate this class and add it to Java Security provider's list so that you can interact with it using standard JCE classes\.

The following example instantiates this class and adds it to Java Security provider’s list:

```
if (Security.getProvider(CloudHsmProvider.PROVIDER_NAME) == null) {
    Security.addProvider(new CloudHsmProvider());
}
```

## `CloudHsmProvider` configuration<a name="java-lib-configs-chsm"></a>

`CloudHsmProvider` can be configured in two ways:

1. Configure with file \(default configuration\)

1. Configure using code

## Configure with file \(Default configuration\)<a name="java-lib-configs-default"></a>

When you instantiate `CloudHsmProvider` using default constructor, by default it will look for configuration file in `/opt/cloudhsm/etc/cloudhsm-jce.cfg` path in Linux\.  This configuration file can be configured using the `configure-jce`\. 

An object created using the default constructor will use the default CloudHSM provider name `CloudHSM`\. The provider name is useful to interact with JCE to let it know which provider to use for various operation\. An example to use CloudHSM provider name for Cipher operation is as below:

```
Cipher cipher = Cipher.getInstance("AES/GCM/NoPadding", "CloudHSM");
```

## Configure using code<a name="java-lib-configs-using-code"></a>

As of Client SDK version 5\.8\.0, you can also configure the `CloudHsmProvider` using Java code\. The way to do this is using an object of `CloudHsmProviderConfig` class\. You can build this object using `CloudHsmProviderConfigBuilder`\. 

`CloudHsmProvider` has another constructor which takes the `CloudHsmProviderConfig` object, as the following sample shows\.

**Example**  

```
CloudHsmProviderConfig config = CloudHsmProviderConfig.builder()  
                                    .withCluster(  
                                        CloudHsmCluster.builder()  
                                            .withHsmCAFilePath(hsmCAFilePath)
                                            .withClusterUniqueIdentifier("CloudHsmCluster1")
        .withServer(CloudHsmServer.builder().withHostIP(hostName).build())  
                        .build())  
        .build();
CloudHsmProvider provider = new CloudHsmProvider(config);
```

In this example, the name of the JCE provider is `CloudHsmCluster1`\. this is the name that application can then use to interact with JCE:

**Example**  

```
Cipher cipher = Cipher.getInstance("AES/GCM/NoPadding", "CloudHsmCluster1");
```

Alternatively, applications can also use the provider object created above to let JCE know to use that provider for the operation:

```
Cipher cipher = Cipher.getInstance("AES/GCM/NoPadding", provider);
```

If a unique identifier is not specified with the `withClusterUniqueIdentifier` method, a randomly generated provider name is created for you\. To get this randomly generated identifier, applications can call `provider.getName()` to get the identifier\.

## Connecting to multiple clusters<a name="java-lib-connecting-to-multiclusters"></a>

As stated above, each `CloudHsmProvider` represents a connection to your CloudHSM Cluster\. If you want to talk to another cluster from the same application, you can create another object of `CloudHsmProvider` with configurations for your other cluster and you can interact with this other cluster either using the provider object or using the provider name, as shown in the following example\.

**Example**  

```
CloudHsmProviderConfig config = CloudHsmProviderConfig.builder()  
                                    .withCluster(  
                                        CloudHsmCluster.builder()  
                                            .withHsmCAFilePath(hsmCAFilePath)
                                            .withClusterUniqueIdentifier("CloudHsmCluster1")
        .withServer(CloudHsmServer.builder().withHostIP(hostName).build())  
                        .build())  
        .build();
CloudHsmProvider provider1 = new CloudHsmProvider(config);

if (Security.getProvider(provider1.getName()) == null) {
    Security.addProvider(provider1);
}

CloudHsmProviderConfig config2 = CloudHsmProviderConfig.builder()  
                                    .withCluster(  
                                        CloudHsmCluster.builder()  
                                            .withHsmCAFilePath(hsmCAFilePath2)
                                            .withClusterUniqueIdentifier("CloudHsmCluster2")
        .withServer(CloudHsmServer.builder().withHostIP(hostName2).build())  
                        .build())  
        .build();
CloudHsmProvider provider2 = new CloudHsmProvider(config2);

if (Security.getProvider(provider2.getName()) == null) {
    Security.addProvider(provider2);
}
```

Once you have configured both the providers \(both the clusters\) above, you can interact with them either using the provider object or using the provider name\. 

Expanding upon this example that shows how to talk to `cluster1`, you could use the following sample for a AES/GCM/NoPadding operation:

```
Cipher cipher = Cipher.getInstance("AES/GCM/NoPadding", provider1);
```

And in the same application to do "AES" Key generation on the second cluster using the provider name, you could also use the following sample:

```
Cipher cipher = Cipher.getInstance("AES/GCM/NoPadding", provider2.getName());
```