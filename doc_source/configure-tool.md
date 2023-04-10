# Configure tool<a name="configure-tool"></a>

AWS CloudHSM automatically synchronizes data among all hardware security modules \(HSM\) in a cluster\. The configure tool updates the HSM data in the configuration files that the synchronization mechanisms use\. Use configure to refresh the HSM data before you use the command line tools, especially when the HSMs in the cluster have changed\.

 AWS CloudHSM includes two major Client SDK versions: 
+ Client SDK 5: This is our latest and default Client SDK\. For information on the benefits and advantages it provides, see [Benefits of Client SDK 5](client-sdk-5-benefits.md)\.
+ Client SDK 3: This is our older Client SDK\. It includes a full set of components for platform and language\-based applications compatibility and management tools\.

**Topics**
+ [Latest configure tool](configure-sdk-5.md)
+ [Previous configure tool](configure-sdk-3.md)