# WorkMail Action<a name="receiving-email-action-workmail"></a>

The **WorkMail** action integrates with Amazon WorkMail\. If Amazon WorkMail performs all of your email processing, you will typically not use this action directly because Amazon WorkMail takes care of the setup\. This action has the following options\.

+ **Organization ARN—**The ARN of the Amazon WorkMail organization\. Amazon WorkMail organization ARNs are in the form `arn:aws:workmail:region:account_ID:organization/organization_ID`, where:

  + `region` is the region in which you are using Amazon SES and Amazon WorkMail\. \(You must use them from the same region\.\) An example is us\-west\-2\.

  + `account_ID` is the AWS account ID\. You can find your AWS account ID on the [Account](https://console.aws.amazon.com/billing/home?#/account) page of the AWS Management Console\.

  + `organization_ID` is a unique identifier that Amazon WorkMail generates when you create an organization\. You can find the organization ID in the Amazon WorkMail console on the Organization Settings page of your organization\. 

  An example of a complete Amazon WorkMail organization ARN is *arn:aws:workmail:us\-west\-2:123456789012:organization/m\-68755160c4cb4e29a2b2f8fb58f359d7*\. For information about Amazon WorkMail organizations, see the [Amazon WorkMail Administrator Guide](http://docs.aws.amazon.com/workmail/latest/adminguide/organizations_overview.html)\.

+ **SNS Topic—**The name or ARN of the Amazon SNS topic to notify when the Amazon WorkMail action is taken\. An example of an Amazon SNS topic ARN is *arn:aws:sns:us\-west\-2:123456789012:MyTopic*\. You can also create an Amazon SNS topic when you set up your action by choosing **Create SNS Topic**\. For more information about Amazon SNS topics, see the [Amazon Simple Notification Service Developer Guide](http://docs.aws.amazon.com/sns/latest/dg/CreateTopic.html)\.
**Note**  
The Amazon SNS topic you choose must be in the same AWS region as the Amazon SES endpoint you use to receive email\. 