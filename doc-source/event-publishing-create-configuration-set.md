# Step 1: Create a Configuration Set Using Amazon SES<a name="event-publishing-create-configuration-set"></a>

Configuration sets enable you to publish email sending events \(bounces, complaints, deliveries, sent emails, and rejected emails\) to Amazon CloudWatch or Amazon Kinesis Firehose\.

You can use the Amazon SES console or the `CreateConfigurationSet` API to create a configuration set\. 

**To create a configuration set \(console\)**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the left navigation pane, choose **Configuration Sets**\.

1. In the content pane, choose **Create Configuration Set**\.

1. Type a name for the configuration set, and then choose **Create Configuration Set**\.

1. Choose **Close**\.

For information about how to use the `CreateConfigurationSet` API to create a configuration set, see the [Amazon Simple Email Service API Reference](http://docs.aws.amazon.com/ses/latest/APIReference/API_CreateConfigurationSet.html)\.