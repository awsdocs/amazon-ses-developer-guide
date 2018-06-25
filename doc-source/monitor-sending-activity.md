# Monitoring Your Amazon SES Sending Activity<a name="monitor-sending-activity"></a>

Amazon SES provides methods to monitor your sending activity\. We recommend that you implement these methods so that you can keep track of important measures, such as your account's bounce, complaint and reject rates\. Excessively high bounce and complaint rates may jeopardize your ability to send emails using Amazon SES\. 

You can also use these methods to measure the rates at which your customers engage with the emails you send\. For example, these sending metrics can help you identify your overall open and clickthrough rates\.

The metrics that you can measure using Amazon SES are referred to as *email sending events*\. The email sending events that you can monitor are:
+ **Sends** – The call to Amazon SES was successful and Amazon SES will attempt to deliver the email\.
+ **Rejects** – Amazon SES accepted the email, determined that it contained a virus, and rejected it\. Amazon SES didn't attempt to deliver the email to the recipient's mail server\.
+ **Bounces** – The recipient's mail server permanently rejected the email\. This event corresponds to hard bounces\. Soft bounces are only included when Amazon SES fails to deliver the email after retrying for a period of time\.
+ **Complaints** – The email was successfully delivered to the recipient\. The recipient marked the email as spam\.
+ **Deliveries** – Amazon SES successfully delivered the email to the recipient's mail server\.
+ **Opens** – The recipient received the message and opened it in his or her email client\.
+ **Clicks** – The recipient clicked one or more links contained in the email\.
+ **Rendering Failures** – The email was not sent because of a template rendering issue\. This event type only occurs when you send email using the [SendTemplatedEmail](http://docs.aws.amazon.com/ses/latest/APIReference/API_SendTemplatedEmail.html) or [SendBulkTemplatedEmail](http://docs.aws.amazon.com/ses/latest/APIReference/API_SendBulkTemplatedEmail.html) API operations\. This event type can occur when template data is missing, or when there is a mismatch between template parameters and data\.

You can monitor email sending events in several ways\. The method you choose depends on the type of event you want to monitor, the granularity and level of detail you want to monitor it with, and the location where you want Amazon SES to publish the data\. You're required to use either feedback notifications or event publishing to track bounce and complaint events\. You can also choose to use multiple monitoring methods\. The characteristics of each method are listed in the following table\.


| Monitoring Method | Events You Can Monitor | How to Access the Data | Level of Detail | Granularity | 
| --- | --- | --- | --- | --- | 
|  Amazon SES console  |  Deliveries and rejects  |  [Sending Statistics page](monitor-using-console.md) in Amazon SES console  |  Count only  |  Across entire AWS account  | 
|  Amazon SES console  |  Bounce and complaint rates  |  [Reputation Dashboard page](reputation-dashboard-dg.md) in Amazon SES console  |  Calculated rates only  |  Across entire AWS account  | 
|  Amazon SES API  |  Deliveries, bounces, complaints, and rejects  |  `GetSendStatistics` API operation  |  Count only  |  Across entire AWS account  | 
|  Amazon CloudWatch console  |  Sends, deliveries, opens, clicks, bounces, complaints, and rejects  |  CloudWatch console  Some metrics don't appear in CloudWatch until the associated event occurs\. For example, bounce metrics don't appear in CloudWatch until at least one email that you send bounces\.   |  Count only  |  Across entire AWS account  | 
|  Feedback notifications  |  Deliveries, bounces, and complaints  |  Amazon SNS notification \(deliveries, bounces, and complaints\) or email \(bounces and complaints only\)  |  Details on each event  |  Across entire AWS account  | 
|  Event publishing  |  Sends, deliveries, opens, clicks, bounces, complaints, rejects, and rendering failures\.  |  Amazon CloudWatch or Amazon Kinesis Firehose, or by Amazon SNS notification  |  Details on each event  |  Fine\-grained \(based on user\-definable email characteristics\)  | 

**Note**  
The metrics measured by email sending events may not align perfectly with your sending limits\. This discrepancy can be caused by email bounces and rejections, or by using the Amazon SES inbox simulator\. To find out how close you are to your sending limits, see [Monitoring Your Sending Limits](monitor-sending-limits.md)\.

For information on how to use each monitoring method, see the following topics:
+ [Monitoring Your Sending Statistics Using the Amazon SES Console](monitor-using-console.md)
+ [Monitoring Your Amazon SES Sender Reputation](monitor-sender-reputation.md)
+ [Monitoring Your Usage Statistics Using the Amazon SES API](monitor-usage-statistics-api.md)
+ [Monitoring Using Notifications](monitor-sending-using-notifications.md)
+ [Monitoring Using Event Publishing](monitor-using-event-publishing.md)


****  

|  | 
| --- |
| For information and discussions about a variety of topics related to Amazon SES, visit the [AWS Messaging and Targeting Blog](https://aws.amazon.com//blogs/messaging-and-targeting/)\. To browse and post questions, go to the [Amazon SES Forum](https://forums.aws.amazon.com/forum.jspa?forumID=90)\. | 