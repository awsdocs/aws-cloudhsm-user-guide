# Configuring AWS CloudHSM backup retention policy<a name="manage-backup-retention"></a>

With the [exemption of clusters created before 18 November 2020](#backup-retention-exemption), the default backup retention policy for clusters is 90 days\. You can set this period to any number between 7 and 379 days\. AWS CloudHSM does not delete a cluster's last backup\. For more information about managing backups, see [Managing backups](manage-backups.md)\. 

## Understanding backup retention policy<a name="backup-retention-conceptual"></a>

AWS CloudHSM purges backups based on the backup retention policy you set when you create a cluster\. Backup retention policy applies to clusters\. If you move a backup to a different region, that backup is no longer associated with a cluster and has no backup retention policy\. You must manually delete any backups not associated with a cluster\. AWS CloudHSM does not delete a cluster's last backup\. 

[AWS CloudTrail](get-api-logs-using-cloudtrail.md) reports backups marked for deletion\. You can restore backups the service purges just as you would restore [manually deleted backups](delete-restore-backup.md)\. To prevent a race condition, you should change the backup retention policy for the cluster before you restore a backup deleted by the service\. If you want to keep the retention policy the same and preserve select backups, you can specify that the service [exclude backups](#exclude-backups-console-proc) from the cluster backup retention policy\.

### Existing\-cluster exemption<a name="backup-retention-exemption"></a>

AWS CloudHSM launched managed backup retention on 18 November 2020\. Clusters created before 18 November 2020 have a backup retention policy of 90 days plus the age of the cluster\. For example, if you created a cluster on 18 November 2019, the service would assign your cluster a backup retention policy of one year plus 90 days \(455 days\)\.

**Note**  
You can opt out of managed backup retention altogether by contacting support \([https://aws\.amazon\.com/support](https://aws.amazon.com/support)\)\. 

## Configure backup retention \(console\)<a name="backup-retention-procedural-console"></a>

**To configure backup retention policy \(console\)**

1. Open the AWS CloudHSM console at [https://console\.aws\.amazon\.com/cloudhsm/home](https://console.aws.amazon.com/cloudhsm/home)\.

1. To change the AWS Region, use the Region selector in the upper\-right corner of the page\.

1. Click the cluster ID of a cluster in the Active state to manage the backup retention policy for that cluster\.

1. To change the backup retention policy, choose **Actions, Change backup retention period**\.

   The Change backup retention period dialog box appears\.

1. In **Backup retention period \(in days\)**, type a value between 7 and 379 days\.

1. Choose **Change backup retention period**\.<a name="exclude-backups-console-proc"></a>

**To exclude or include a backup from backup retention policy \(console\)**

1. Open the AWS CloudHSM console at [https://console\.aws\.amazon\.com/cloudhsm/home](https://console.aws.amazon.com/cloudhsm/home)\.

1. To view your backups, in the navigation pane choose **Backups**\. 

1. Click the backup ID of a backup in the Ready state to exclude or include\.

1. On the **Backup details** page, take one of the following actions\.
   + To exclude a backup with a date in **Expiration time**, choose **Actions, Disable expiration**\.
   + To include a backup that does not expire, choose **Actions, Use cluster retention policy**\.

## Configure backup retention \(AWS CLI\)<a name="backup-retention-procedural-cli"></a>

Check the status of a backup or find its ID by using the [describe\-backups](https://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/describe-backups.html) command from the AWS CLI\.

**To configure backup retention policy \(AWS CLI\)**
+  At a command prompt, issue the modify\-cluster command\. Specify the cluster ID and the backup retention policy\. 

  ```
  $ aws cloudhsmv2 modify-cluster --cluster-id <cluster ID> \
                                  --backup-retention-policy  Type=DAYS,Value=<number of days to retain backups>
  {
     "Cluster": { 
        "BackupPolicy": "DEFAULT",
        "BackupRetentionPolicy": {
           "Type": "DAYS",
           "Value": 90
        },
        "Certificates": {},
        "ClusterId": "cluster-kdmrayrc7gi",
        "CreateTimestamp": 1504903546.035,
        "Hsms": [],
        "HsmType": "hsm1.medium",
        "SecurityGroup": "sg-40399d28",
        "State": "ACTIVE",
        "SubnetMapping": { 
           "us-east-2a": "subnet-f1d6e798",
           "us-east-2c": "subnet-0e358c43",
           "us-east-2b": "subnet-40ed9d3b" 
        },
        "TagList": [ 
           { 
              "Key": "Cost Center",
              "Value": "12345"
           }
        ],
        "VpcId": "vpc-641d3c0d"
     }
  }
  ```

**To exclude a backup from backup retention policy \(AWS CLI\)**
+ At a command prompt, issue the modify\-backup\-attributes command\. Specify the backup ID and set the never\-expires flag to preserve the backup\. 

  ```
  $ aws cloudhsmv2 modify-backup-attributes --backup-id <backup ID> \
                                            --never-expires
  {
     "Backup": { 
        "BackupId": "backup-ro5c4er4aac",
        "BackupState": "READY",
        "ClusterId": "cluster-dygnwhmscg5",
        "NeverExpires": true
     }
  }
  ```

**To include a backup in backup retention policy \(AWS CLI\)**
+ At a command prompt, issue the modify\-backup\-attributes command\. Specify the backup ID and set the no\-never\-expires flag to include the backup in backup retention policy, which means the service will eventually delete the backup\.

  ```
  $ aws cloudhsmv2 modify-backup-attributes --backup-id <backup ID> \
                                            --no-never-expires
  {
     "Backup": { 
        "BackupId": "backup-ro5c4er4aac",
        "BackupState": "READY",
        "ClusterId": "cluster-dygnwhmscg5",
        "NeverExpires": false
     }
  }
  ```

## Configure backup retention \(AWS CloudHSM API\)<a name="backup-retention-procedural-api"></a>

Refer to the following topics to learn how to manage backup retention by using the API\.
+ [https://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_ModifyCluster.html](https://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_ModifyCluster.html)
+ [https://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_ModifyBackupAttributes.html](https://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_ModifyBackupAttributes.html)