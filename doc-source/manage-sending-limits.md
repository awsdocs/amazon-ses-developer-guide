# Managing Your Amazon SES Sending Limits<a name="manage-sending-limits"></a>

Your Amazon Simple Email Service \(Amazon SES\) account has a set of sending limits to regulate the number of email messages that you can send and the rate at which you can send them\. Sending limits benefit all Amazon SES customers because they help to maintain the trusted relationship between Amazon SES and internet service providers \(ISPs\)\. Sending limits help you to gradually ramp up your sending activity and decrease the likelihood that ISPs will block your emails because of sudden, unexpected spikes in your email sending volume or rate\.

The following are Amazon SES sending limits:
+ **Sending Quota**—The maximum number of emails that you can send in a 24\-hour period\. The sending quota reflects a rolling time period\. Every time you try to send an email, Amazon SES checks how many emails you sent in the previous 24 hours\. As long as the total number of emails that you have sent is less than your quota, your send request will be accepted and your email will be sent\. If you have already sent your full quota, your send request will be rejected with a throttling exception\. For example, if your sending quota is 50,000, and you sent 15,000 emails in the previous 24 hours, then you can send another 35,000 emails right away\. If you have already sent 50,000 emails in the previous 24 hours, you will not be able to send more emails until some of the previous sending rolls out of its 24\-hour window\.
+ **Maximum Send Rate**—The maximum number of emails that Amazon SES can accept from your account per second\. You can exceed this limit for short bursts, but not for a sustained period of time\.
**Note**  
The rate at which Amazon SES accepts your messages might be less than the maximum send rate\.

Your Amazon SES sending limits for each AWS region are separate\. For information about using Amazon SES in multiple AWS regions, see [Regions and Amazon SES](regions.md)\.

When you are in the Amazon SES sandbox, your sending quota is 200 messages per 24\-hour period and your maximum sending rate is one message per second\. To increase your sending limits, you need to submit an SES Sending Limits Increase case\. For more information, see [Moving Out of the Amazon SES Sandbox](request-production-access.md)\. After you have moved out of the sandbox and start sending emails, you can increase your sending limits further by submitting another SES Sending Limits Increase case, as discussed in [Increasing Your Amazon SES Sending Limits](increase-sending-limits.md)\.

**Note**  
Sending limits are based on recipients rather than on messages\. For example, an email that has 10 recipients counts as 10 against your quota\. However, we do not recommend that you send an email to multiple recipients in one call to `SendEmail` because if the call to Amazon SES fails \(for example, the request is improperly formatted\), the entire email will be rejected and none of the recipients will get the intended email\. We recommend that you call `SendEmail` once for every recipient\.
+ To increase your sending limits, see [Increasing Your Amazon SES Sending Limits](increase-sending-limits.md)\. 
+ For information about the errors your application receives when you reach your sending limits, see [What Happens When You Reach Your Amazon SES Sending Limits](reach-sending-limits.md)\.
+ To monitor your sending limits by using the Amazon SES console or the Amazon SES API, see [Monitoring Your Amazon SES Sending Limits](monitor-sending-limits.md)\.


****  

|  | 
| --- |
| For information and discussions about a variety of topics related to Amazon SES, visit the [AWS Messaging and Targeting Blog](https://aws.amazon.com//blogs/messaging-and-targeting/)\. To browse and post questions, go to the [Amazon SES Forum](https://forums.aws.amazon.com/forum.jspa?forumID=90)\. | 