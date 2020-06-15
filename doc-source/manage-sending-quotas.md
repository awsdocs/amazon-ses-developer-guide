# Managing Your Amazon SES Sending Quotas<a name="manage-sending-quotas"></a>

Your Amazon SES account has a set of sending quotas that regulate the number of email messages that you can send and the rate at which you can send them\. Sending quotas benefit all Amazon SES customers because they help to maintain the trusted relationship between Amazon SES and email providers\. Sending quotas help you to gradually ramp up your sending activity and decrease the likelihood that email providers block your emails because of sudden, unexpected spikes in your email sending volume or rate\.

The following quotas apply to sending email through Amazon SES:
+ **Maximum daily sends**—The maximum number of emails that you can send in a 24\-hour period\. This quota is calculated on a rolling time period\. Every time you try to send an email, Amazon SES determines the number of emails that you sent in the previous 24 hours\. As long as the total number of emails that you have sent in the past 24 hours is less than this daily maximum, your send request is accepted and your email is sent\.

  If sending a message would exceed the daily maximum for your account, your call to Amazon SES is rejected\.
+ **Maximum sending rate**—The maximum number of emails that Amazon SES can accept from your account each second\. You can exceed this quota for short bursts, but not for sustained periods of time\.
**Note**  
The rate at which Amazon SES accepts your messages can be less than the maximum send rate for your account\.

Your Amazon SES sending quotas are separate for each AWS Region\. For information about using Amazon SES in multiple AWS Regions, see [Regions and Amazon SES](regions.md)\.

When your account is in the Amazon SES sandbox, you can only send 200 messages per 24\-hour period, and your maximum sending rate is one message per second\. When you submit a request to have your account removed from the sandbox, you can also request that your quotas are increased at the same time\. For more information about having your account removed from the sandbox, see [Moving out of the Amazon SES sandbox](request-production-access.md)\.

When your account has been removed from the sandbox, you can request additional quota increases at any time by creating a new case in the AWS Support Center\. For more information, see [Increasing Your Amazon SES Sending Quotas](manage-sending-quotas-request-increase.md)\.

**Note**  
Sending quotas are based on recipients rather than on messages\. For example, an email that has 10 recipients counts as 10 against your quota\. However, we don't recommend that you send an email to multiple recipients in a single call to the `SendEmail` API operation, because if the call fails, the entire email is rejected\. We recommend that you call `SendEmail` once for every recipient\.
+ To increase your sending quotas, see [Increasing Your Amazon SES Sending Quotas](manage-sending-quotas-request-increase.md)\. 
+ For information about the errors your application receives when you reach your sending quotas, see [Errors Related to the Sending Quotas for Your Amazon SES Account](manage-sending-quotas-errors.md)\.
+ To monitor your sending quotas by using the Amazon SES console or the Amazon SES API, see [Monitoring Your Amazon SES Sending Quotas](manage-sending-quotas-monitor.md)\.


****  

|  | 
| --- |
| For information and discussions about a variety of topics related to Amazon SES, see the [AWS Messaging and Targeting Blog](https://aws.amazon.com//blogs/messaging-and-targeting/)\. To browse and post questions, go to the [Amazon SES Forum](https://forums.aws.amazon.com/forum.jspa?forumID=90)\. | 