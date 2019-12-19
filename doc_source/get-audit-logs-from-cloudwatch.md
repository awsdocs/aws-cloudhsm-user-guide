# How Audit Logging Works<a name="get-audit-logs-from-cloudwatch"></a>

Audit logging is automatically enabled in all AWS CloudHSM clusters\. It cannot be disabled or turned off, and no settings can prevent AWS CloudHSM from exporting the logs to CloudWatch Logs\. Each log event has a time stamp and sequence number that indicate the order of events and help you detect any log tampering\. 

Each HSM instance generates its own log\. The audit logs of various HSMs, even those in the same cluster, are likely to differ\. For example, only the first HSM in each cluster records initialization of the HSM\. Initialization events do not appear in the logs of HSMs that are cloned from backups\. Similarly, when you create a key, the HSM that generates the key records a key generation event\. The other HSMs in the cluster record an event when they receive the key via synchronization\.

AWS CloudHSM collects the logs and posts them to CloudWatch Logs in your account\. To communicate with the CloudWatch Logs service on your behalf, AWS CloudHSM uses a [service\-linked role](service-linked-roles.md)\. The IAM policy that is associated with the role allows AWS CloudHSM to perform only the tasks required to send the audit logs to CloudWatch Logs\.

**Important**  
If you created a cluster before January 20, 2018, and have not yet created an attached service\-linked role, you must manually create one\. This is necessary for CloudWatch to receive audit logs from your AWS CloudHSM cluster\. For more information about service\-linked role creation, see [Understanding Service\-Linked Roles](service-linked-roles.md), as well as [Creating a Service\-Linked Role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#create-service-linked-role) in the *IAM User Guide*\.