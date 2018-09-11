# Monitoring AWS CloudHSM Logs<a name="get-logs"></a>

AWS CloudHSM is integrated with the following AWS services to provide different kinds of logs\.

**AWS CloudTrail for API logs**  
AWS CloudHSM is integrated with AWS CloudTrail, a service that records all AWS CloudHSM API calls in your AWS account\. CloudTrail records these calls in log files that are delivered to an Amazon Simple Storage Service \(Amazon S3\) bucket of your choice\. For example, when you create and delete AWS CloudHSM clusters, create and delete HSMs in a cluster, tag AWS CloudHSM resources, and more, the corresponding API calls are recorded in CloudTrail log files\.

**Amazon CloudWatch Logs for HSM Audit Logs**  
AWS CloudHSM sends the audit logs recorded by your HSM instances to Amazon CloudWatch Logs, a service that stores, organizes, and displays log data from multiple sources\. For example, when you create and delete HSM users, change user passwords, create and delete keys, and more, these events are collected and stored in CloudWatch Logs\.

For more information, see the following topics\.

**Topics**
+ [Getting AWS CloudHSM Client Logs](hsm-client-logs.md)
+ [Logging AWS CloudHSM API Calls with AWS CloudTrail](get-api-logs-using-cloudtrail.md)
+ [Monitoring AWS CloudHSM Audit Logs in Amazon CloudWatch Logs](get-hsm-audit-logs-using-cloudwatch.md)