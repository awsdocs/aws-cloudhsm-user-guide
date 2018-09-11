# Monitoring AWS CloudHSM Audit Logs in Amazon CloudWatch Logs<a name="get-hsm-audit-logs-using-cloudwatch"></a>

When an HSM in your account receives a command from the AWS CloudHSM [command line tools](command-line-tools.md) or [software libraries](use-hsm.md), it records its execution of the command in audit log form\. The HSM audit logs include all client\-initiated [management commands](cloudhsm-audit-log-reference.md), including those that create and delete the HSM, log into and out of the HSM, and manage users and keys\. These logs provide a reliable record of actions that have changed the state of the HSM\.

AWS CloudHSM collects your HSM audit logs and sends them to [Amazon CloudWatch Logs](http://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html) on your behalf\. You can use the features of CloudWatch Logs to manage your AWS CloudHSM audit logs, including searching and filtering the logs and exporting log data to Amazon S3\. You can work with your HSM audit logs in the [Amazon CloudWatch console](https://console.aws.amazon.com/cloudwatch/) or use the CloudWatch Logs commands in the [AWS CLI](http://docs.aws.amazon.com/cli/latest/reference/logs/index.html) and [CloudWatch Logs SDKs](http://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/)\.

**Topics**
+ [How Audit Logging Works](get-audit-logs-from-cloudwatch.md)
+ [Viewing Audit Logs in CloudWatch Logs](understand-audit-logs.md)
+ [Interpreting HSM Audit Logs](interpreting-audit-logs.md)
+ [Audit Log Reference](cloudhsm-audit-log-reference.md)