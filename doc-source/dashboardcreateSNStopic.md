# Part 1: Create a Topic in Amazon Simple Notification Service<a name="dashboardcreateSNStopic"></a>

The first step in creating a bounce and complaint tracking dashboard is to create a new topic in Amazon Simple Notification Service\. When you create an Amazon SQS queue in the next section, you will subscribe to this topic to gather bounce and complaint notifications\.

**To create a new Amazon SNS topic**

1. Sign in to the AWS Management Console and open the Amazon SNS console at [https://console\.aws\.amazon\.com/sns/v2/home](https://console.aws.amazon.com/sns/v2/home)\.

1. In the navigation bar, choose **Topics**\.

1. Choose **Create new topic**\.

1. For **Topic name**, enter a name for the topic\.

1. For **Display name**, enter a display name for the topic\.

1. Choose **Create topic**\.

1. Proceed to [Part 2: Create a Queue in Amazon Simple Queue Service](dashboardcreateSQSqueue.md)\.