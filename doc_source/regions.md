# Regions<a name="regions"></a>

Visit [AWS Regions and Endpoints](http://docs.aws.amazon.com/general/latest/gr/rande.html#cloudhsm_region) in the *AWS General Reference* or the [AWS Region Table](https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/) to see the regional and Availability Zone support for AWS CloudHSM\.

Like most AWS resources, clusters and HSMs are used regionally\. To create an HSM in more than one region, you must first create a cluster in that region\. You cannot reuse or extend a cluster across regions\. You must perform all the required steps listed in [Getting Started with AWS CloudHSM](getting-started.md) to create a new cluster in a new region\.

**Note**  
AWS CloudHSM may not be available across all Availability Zones in a given region\. However, this should not affect performance, as AWS CloudHSM automatically load balances across all HSMs in a cluster\.