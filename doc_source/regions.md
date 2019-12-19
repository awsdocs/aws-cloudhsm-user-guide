# Regions<a name="regions"></a>

For information about the supported Regions for AWS CloudHSM, see [AWS CloudHSM Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/cloudhsm.html) in the *AWS General Reference*, or the [Region Table](https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/)\.

Like most AWS resources, clusters and HSMs are regional resources\. To create HSMs in multiple Regions, you must first create a cluster in each Region\. You cannot reuse or extend a cluster across Regions\. You must perform all the required steps listed in [Getting Started with AWS CloudHSM](getting-started.md) to create a cluster in a new Region\.

AWS CloudHSM might not be available in all Availability Zones in a given Region\. However, this should not affect performance, as AWS CloudHSM automatically load balances across all HSMs in a cluster\.