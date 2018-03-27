# Stop Action<a name="receiving-email-action-stop"></a>

The **Stop** action terminates the evaluation of the receipt rule set and, optionally, notifies you through Amazon SNS\. This action has the following options\.
+ **SNS Topic—**The name or ARN of the Amazon SNS topic to notify when the Stop action is performed\. An example of an Amazon SNS topic ARN is *arn:aws:sns:us\-west\-2:123456789012:MyTopic*\. You can also create an Amazon SNS topic when you set up your action by choosing **Create SNS Topic**\. For more information about Amazon SNS topics, see the [Amazon Simple Notification Service Developer Guide](http://docs.aws.amazon.com/sns/latest/dg/CreateTopic.html)\.
**Note**  
The Amazon SNS topic you choose must be in the same AWS region as the Amazon SES endpoint you use to receive email\. 