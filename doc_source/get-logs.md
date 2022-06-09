# Monitoring AWS CloudHSM<a name="get-logs"></a>

In addition to the logging features built into the Client SDK, you can also use AWS CloudTrail, Amazon CloudWatch Logs, and Amazon CloudWatch to monitor AWS CloudHSM\.

**Client SDK logs**  
Use Client SDK logging to monitor diagnostic and troubleshooting information from the applications you create\. 

**CloudTrail**  
Use CloudTrail to monitor all API calls in your AWS account, including the calls you make to create and delete clusters, hardware security modules \(HSM\), and resource tags\.

**CloudWatch Logs**  
Use CloudWatch Logs to monitor the logs from your HSM instances, which include events for create and delete HSM users, change user passwords, create and delete keys, and more\.

**CloudWatch**  
Use CloudWatch to monitor the health of your cluster in real time\. 

**Topics**
+ [Working with client SDK logs](hsm-client-logs.md)
+ [Working with AWS CloudTrail and AWS CloudHSM](get-api-logs-using-cloudtrail.md)
+ [Working with Amazon CloudWatch Logs and AWS CloudHSM](get-hsm-audit-logs-using-cloudwatch.md)
+ [Getting CloudWatch metrics for AWS CloudHSM](hsm-metrics-cw.md)