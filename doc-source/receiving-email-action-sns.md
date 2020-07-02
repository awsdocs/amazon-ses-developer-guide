# SNS action<a name="receiving-email-action-sns"></a>

The **SNS** action publishes the mail using an Amazon SNS notification\. The notification includes the complete email content\. This action has the following options\.
+ **SNS Topic—**The name or ARN of the Amazon SNS topic to which to publish the emails\. The Amazon SNS notifications will contain a raw, unmodified copy of the email, which is typically in Multipurpose Internet Mail Extensions \(MIME\) format\. For more information about MIME format, see [RFC 2045](https://tools.ietf.org/html/rfc2045)\.
**Important**  
If you choose to receive your emails through Amazon SNS notifications, the maximum email size \(including headers\) is 150 KB\. Larger emails will bounce\. If you anticipate emails larger than this size, save the emails to an Amazon S3 bucket instead\.

  An example of an Amazon SNS topic ARN is *arn:aws:sns:us\-west\-2:123456789012:MyTopic*\. You can also create an Amazon SNS topic when you set up your action by choosing **Create SNS Topic**\. For more information about Amazon SNS topics, see the [Amazon Simple Notification Service Developer Guide](https://docs.aws.amazon.com/sns/latest/dg/CreateTopic.html)\.
**Note**  
The Amazon SNS topic you choose must be in the same AWS region as the Amazon SES endpoint you use to receive email\. 
+ **Encoding—**The encoding to use for the email within the Amazon SNS notification\. UTF\-8 is easier to use, but may not preserve all special characters when a message was encoded with a different encoding format\. Base64 preserves all special characters\. For information about UTF\-8 and Base64, see [RFC 3629](https://tools.ietf.org/html/rfc3629) and [RFC 4648](https://tools.ietf.org/html/rfc4648), respectively\.