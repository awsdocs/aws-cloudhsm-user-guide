# Tagging AWS CloudHSM resources<a name="tag-resources"></a>

A tag is a label that you assign to an AWS resource\. You can assign tags to your AWS CloudHSM clusters\. Each tag consists of a tag key and a tag value, both of which you define\. For example, the tag key might be **Cost Center** and the tag value might be **12345**\. Tag keys must be unique for each cluster\.

You can use tags for a variety of purposes\. One common use is to categorize and track your AWS costs\. You can apply tags that represent business categories \(such as cost centers, application names, or owners\) to organize your costs across multiple services\. When you add tags to your AWS resources, AWS generates a cost allocation report with usage and costs aggregated by tags\. You can use this report to view your AWS CloudHSM costs in terms of projects or applications, instead of viewing all AWS CloudHSM costs as a single line item\.

For more information about using tags for cost allocation, see [Using Cost Allocation Tags](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-alloc-tags.html) in the *AWS Billing User Guide*\.

You can use the [AWS CloudHSM console](https://console.aws.amazon.com/cloudhsm/) or one of the [AWS SDKs or command line tools](https://aws.amazon.com/tools/) to add, update, list, and remove tags\.

**Topics**
+ [Adding or updating tags](#add-update-tags)
+ [Listing tags](#list-tags)
+ [Removing tags](#remove-tags)

## Adding or updating tags<a name="add-update-tags"></a>

You can add or update tags from the [AWS CloudHSM console](https://console.aws.amazon.com/cloudhsm/), the [AWS Command Line Interface \(AWS CLI\)](https://aws.amazon.com/cli/), or the AWS CloudHSM API\.

**To add or update tags \(console\)**

1. Open the AWS CloudHSM console at [https://console\.aws\.amazon\.com/cloudhsm/home](https://console.aws.amazon.com/cloudhsm/home)\.

1. Choose the cluster that you are tagging\.

1. Choose **Tags**\.

1. To add a tag, do the following:

   1. Choose **Edit Tag** and then choose **Add Tag**\.

   1. For **Key**, type a key for the tag\.

   1. \(Optional\) For **Value**, type a value for the tag\.

   1. Choose **Save**\.

1. To update a tag, do the following:

   1. Choose **Edit Tag**\.
**Note**  
If you update the tag key for an existing tag, the console deletes the existing tag and creates a new one\.

   1. Type the new tag value\.

   1. Choose **Save**\.

**To add or update tags \(AWS CLI\)**

1. At a command prompt, issue the [https://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/tag-resource.html](https://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/tag-resource.html) command, specifying the tags and the ID of the cluster that you are tagging\. If you don't know the cluster ID, issue the [describe\-clusters](https://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/describe-clusters.html) command\.

   ```
   $ aws cloudhsmv2 tag-resource --resource-id <cluster ID> \
                                 --tag-list Key="<tag key>",Value="<tag value>"
   ```

1. To update tags, use the same command but specify an existing tag key\. When you specify a new tag value for an existing tag, the tag is overwritten with the new value\.

**To add or update tags \(AWS CloudHSM API\)**
+ Send a [https://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_TagResource.html](https://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_TagResource.html) request\. Specify the tags and the ID of the cluster that you are tagging\.

## Listing tags<a name="list-tags"></a>

You can list tags for a cluster from the [AWS CloudHSM console](https://console.aws.amazon.com/cloudhsm/), the [AWS CLI](https://aws.amazon.com/cli/), or the AWS CloudHSM API\.

**To list tags \(console\)**

1. Open the AWS CloudHSM console at [https://console\.aws\.amazon\.com/cloudhsm/home](https://console.aws.amazon.com/cloudhsm/home)\.

1. Choose the cluster whose tags you are listing\.

1. Choose **Tags**\.

**To list tags \(AWS CLI\)**
+ At a command prompt, issue the [https://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/list-tags.html](https://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/list-tags.html) command, specifying the ID of the cluster whose tags you are listing\. If you don't know the cluster ID, issue the [describe\-clusters](https://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/describe-clusters.html) command\.

  ```
  $ aws cloudhsmv2 list-tags --resource-id <cluster ID>
  {
      "TagList": [
          {
              "Key": "Cost Center",
              "Value": "12345"
          }
      ]
  }
  ```

**To list tags \(AWS CloudHSM API\)**
+ Send a [https://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_ListTags.html](https://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_ListTags.html) request, specifying the ID of the cluster whose tags you are listing\.

## Removing tags<a name="remove-tags"></a>

You can remove tags from a cluster by using the [AWS CloudHSM console](https://console.aws.amazon.com/cloudhsm/), the [AWS CLI](https://aws.amazon.com/cli/), or the AWS CloudHSM API\.

**To remove tags \(console\)**

1. Open the AWS CloudHSM console at [https://console\.aws\.amazon\.com/cloudhsm/home](https://console.aws.amazon.com/cloudhsm/home)\.

1. Choose the cluster whose tags you are removing\.

1. Choose **Tags**\.

1. Choose **Edit Tag** and then choose **Remove tag** for the tag you want to remove\.

1. Choose **Save**\.

**To remove tags \(AWS CLI\)**
+ At a command prompt, issue the [https://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/untag-resource.html](https://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/untag-resource.html) command, specifying the tag keys of the tags that you are removing and the ID of the cluster whose tags you are removing\. When you use the AWS CLI to remove tags, specify only the tag keys, not the tag values\.

  ```
  $ aws cloudhsmv2 untag-resource --resource-id <cluster ID> \
                                  --tag-key-list "<tag key>"
  ```

**To remove tags \(AWS CloudHSM API\)**
+ Send an [https://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_UntagResource.html](https://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_UntagResource.html) request in the AWS CloudHSM API, specifying the ID of the cluster and the tags that you are removing\.