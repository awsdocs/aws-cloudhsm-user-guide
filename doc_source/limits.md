# AWS CloudHSM quotas<a name="limits"></a>

Quotas, formerly known as limits, are the assigned values for AWS resources\. The following quotas apply to your AWS CloudHSM resources per AWS Region and AWS account\. The default quota is the initial value applied by AWS, and these values are listed in the table below\. An adjustable quota can be increased above the default quota\.


**Service quotas**  

| Resource | Default Quota | Adjustable? | 
| --- | --- | --- | 
| Clusters | 4 | Yes | 
| HSMs | 6 | Yes | 
| HSMs per cluster | 28 | No | 

The quotas in the following System Quotas table are not adjustable\.


**System quotas**  

| Resource | Quota | 
| --- | --- | 
| Keys per cluster | 3,300 | 
| Number of users per cluster | 1,024 | 
| Maximum length of a user name | 31 characters | 
| Required password length | 7 to 32 characters | 
| Maximum number of concurrent clients | 900 | 

The recommended way of requesting a quota increase is to open the [Service Quotas console](https://console.aws.amazon.com/servicequotas/home?region=us-east-1#!/dashboard)\. In the console, choose your service and quota, and submit your request\. For more information, see the [Service Quotas documentation](https://docs.aws.amazon.com/servicequotas/latest/userguide/request-quota-increase.html)\.