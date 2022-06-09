# Managing cloned clusters<a name="cloned-clusters"></a>

 Use CloudHSM Management Utility \(CMU\) to synchronize a cluster in a remote region, *if the cluster in that region was originally created from the backup of a cluster in another region*\. Let's say you copied a cluster to another region \(destination\) and then later you want to synchronize changes from the original cluster \(source\)\. In scenarios like this, you use CMU to synchronize the clusters\. You do this by creating a new CMU configuration file, specifying hardware security modules \(HSM\) from both clusters in the new file, and then using CMU to connect to the cluster with that file\. 

**To use CMU across cloned clusters**

1. Create a copy of your current configuration file and change the name of the copy to something else\. 

   For example, use the following file locations to locate and create a copy of your current configuration file, then change the name of the copy from `cloudhsm_mgmt_config.cfg` to `syncConfig.cfg`\.
   + Linux: `/opt/cloudhsm/etc/cloudhsm_mgmt_config.cfg`
   + Windows: `C:\ProgramData\Amazon\CloudHSM\data\cloudhsm_mgmt_config.cfg`

1. In the renamed copy, add the Elastic Network Interface \(ENI\) IP of the destination HSM \(the HSM in the foreign region that needs to be synced\)\. We recommend that you add the destination HSM *below* the source HSM\. 

   ```
   {
       ...
       "servers": [
           {
               ...
               "hostname": "<ENI Source IP>",
               ...
           },
           {
               ...
               "hostname": "<ENI Destination IP>",
               ...
           }
       ]
   }
   ```

   For more information about how to get the IP address, see [Get an IP address for an HSM](#get-ip-clone-cluster)\.

1. Initialize CMU with the new configuration file:

------
#### [ Linux ]

   ```
   $ /opt/cloudhsm/bin/cloudhsm_mgmt_util /opt/cloudhsm/etc/userSync.cfg
   ```

------
#### [ Windows ]

   ```
   C:\Program Files\Amazon\CloudHSM>cloudhsm_mgmt_util.exe C:\ProgramData\Amazon\CloudHSM\data\userSync.cfg
   ```

------

1. Check the status messages returned to ensure that the CMU is connected to all desired HSMs and determine which of the returned ENI IPs corresponds to each cluster\. Use syncUser and syncKey to manually synchronize users and keys\. For more information, see [syncUser](cloudhsm_mgmt_util-syncUser.md) and [syncKey](cloudhsm_mgmt_util-syncKey.md)\.

## Get an IP address for an HSM<a name="get-ip-clone-cluster"></a>

Use this section to obtain an IP address for an HSM\.

**To get an IP address for a HSM \(console\)**

1. Open the AWS CloudHSM console at [https://console\.aws\.amazon\.com/cloudhsm/](https://console.aws.amazon.com/cloudhsm/)\.

1. To change the AWS Region, use the Region selector in the upper\-right corner of the page\.

1. In the navigation pane, choose **Clusters**\.

1. To open the cluster detail page, in the cluster table, choose the cluster ID\.

1. To get the IP address, on the HSMs tab, choose one of the IP addresses listed under **ENI IP address**\. 

**To get an IP address for a HSM \(AWS CLI\)**
+ Get the IP address of an HSM by using the [describe\-clusters](https://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/describe-clusters.html) command from the AWS CLI\. In the output from the command, the IP address of the HSMs are the values of `EniIp`\. 

  ```
  $  aws cloudhsmv2 describe-clusters
  	
  
  {
      "Clusters": [
          { ... }
              "Hsms": [
                  {
  ...
                      "EniIp": "10.0.0.9",
  ...
                  },
                  {
  ...
                      "EniIp": "10.0.1.6",
  ...
  ```

## Related topics<a name="clone-cluster-related"></a>
+ [syncUser](cloudhsm_mgmt_util-syncUser.md)
+ [syncKey](cloudhsm_mgmt_util-syncKey.md)
+ [Copying Backups Across Regions](copy-backup-to-region.md)