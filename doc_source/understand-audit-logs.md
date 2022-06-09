# Viewing HSM audit logs in CloudWatch Logs<a name="understand-audit-logs"></a>

Amazon CloudWatch Logs organizes the audit logs into *log groups* and, within a log group, into *log streams*\. Each log entry is an *event*\. AWS CloudHSM creates one *log group* for each cluster and one *log stream* for each HSM in the cluster\. You do not have to create any CloudWatch Logs components or change any settings\.
+ The *log group* name is `/aws/cloudhsm/<cluster ID>`; for example `/aws/cloudhsm/cluster-likphkxygsn`\. When you use the log group name in a AWS CLI or PowerShell command, be sure to enclose it in double quotation marks\.
+ The *log stream* name is the HSM ID; for example, `hsm-nwbbiqbj4jk`\. 

  In general, there is one log stream for each HSM\. However, any action that changes the HSM ID, such as when an HSM fails and is replaced, creates a new log stream\.

For more information about CloudWatch Logs concepts, see [Concepts](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/CloudWatchLogsConcepts.html) in the *Amazon CloudWatch Logs User Guide*\.

You can view the audit logs for an HSM from the CloudWatch Logs page in the AWS Management Console, the [CloudWatch Logs commands](https://docs.aws.amazon.com/cli/latest/reference/logs/index.html#cli-aws-logs) in the AWS CLI, the [CloudWatch Logs PowerShell cmdlets](https://docs.aws.amazon.com/powershell/latest/reference/items/Amazon_CloudWatch_Logs_cmdlets.html), or the [CloudWatch Logs SDKs](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/)\. For instructions, see [View Log Data](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/Working-with-log-groups-and-streams.html#ViewingLogData) in the *Amazon CloudWatch Logs User Guide*\. 

For example, the following image shows the log group for the `cluster-likphkxygsn` cluster in the AWS Management Console\.

![\[The log group for a AWS CloudHSM cluster in CloudWatch Logs.\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/cloudwatch-logs-cluster.png)

When you choose the cluster log group name, you can view the log stream for each of the HSMs in the cluster\. The following image shows the log streams for the HSMs in the `cluster-likphkxygsn` cluster\.

![\[The log stream for an HSM in CloudWatch Logs.\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/cloudwatch-logs-hsm.png)

When you choose an HSM log stream name, you can view the events in the audit log\. For example, this event, which has a sequence number of 0x0 and an `Opcode` of `CN_INIT_TOKEN`, is typically the first event for the first HSM in each cluster\. It records the initialization of the HSM in the cluster\. 

![\[An event in an AWS CloudHSM audit log in CloudWatch Logs.\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/cloudwatch-logs-event.png)

You can use all the many features in CloudWatch Logs to manage your audit logs\. For example, you can use the **Filter events** feature to find particular text in an event, such as the `CN_CREATE_USER` `Opcode`\.

To find all events that do not include the specified text, add a minus sign \(\-\) before the text\. For example, to find events that do not include `CN_CREATE_USER`, enter \-CN\_CREATE\_USER\.

![\[Filtering an event in an AWS CloudHSM audit log in CloudWatch Logs by its Opcode value.\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/cloudwatch-logs-event-filter.png)