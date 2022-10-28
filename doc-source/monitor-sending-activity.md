# Monitoring your Amazon SES sending activity<a name="monitor-sending-activity"></a>

Amazon SES provides methods to monitor your sending activity using events, metrics, and statistics\. An event is something that happens related to your sending activity that you’ve specified to be tracked as a metric\. A metric represents a time\-ordered set of data points representing the values of a monitored event type producing statistics\. Statistics are metric data aggregations for a specified period of time including up to the present\. 

These monitoring methods assist you in keeping track of important measures, such as your account's bounce, complaint and reject rates\. Excessively high bounce and complaint rates may jeopardize your ability to send emails using SES\. These methods can also be used to measure the rates at which your customers engage with the emails you send by helping you to identify your overall open and click through rates utilizing event publishing and custom domains associated with configuration sets \- see [Configuring custom domains to handle open and click tracking](configure-custom-open-click-domains.md)\.

The first step in setting up monitoring is to identify the types of email events related to your sending activity that you want to measure and monitor using SES\. You can choose the following event types to monitor in SES:
+ **Send** – The send request was successful and Amazon SES will attempt to deliver the message to the recipient’s mail server\. \(If account\-level or global suppression is being used, SES will still count it as a send, but delivery is suppressed\.\)
+ **Rendering Failure** – The email wasn't sent because of a template rendering issue\. This event type can occur when template data is missing, or when there is a mismatch between template parameters and data\. \(This event type only occurs when you send email using the [https://docs.aws.amazon.com/ses/latest/APIReference/API_SendTemplatedEmail.html](https://docs.aws.amazon.com/ses/latest/APIReference/API_SendTemplatedEmail.html) or [https://docs.aws.amazon.com/ses/latest/APIReference/API_SendBulkTemplatedEmail.html](https://docs.aws.amazon.com/ses/latest/APIReference/API_SendBulkTemplatedEmail.html) API operations\.\)
+ **Reject** – Amazon SES accepted the email, but determined that it contained a virus and didn’t attempt to deliver it to the recipient’s mail server\.
+ **Delivery** – Amazon SES successfully delivered the email to the recipient's mail server\.
+ **Hard bounce** – The recipient's mail server permanently rejected the email\. \(*Soft bounces* are only included when Amazon SES fails to deliver the email after retrying for a period of time\.\)
+ **Complaint** – The email was successfully delivered to the recipient’s mail server, but the recipient marked it as spam\.
+ **Delivery Delay** – The email couldn't be delivered to the recipient’s mail server because a temporary issue occurred\. Delivery delays can occur, for example, when the recipient's inbox is full, or when the receiving email server experiences a transient issue\.
+ **Subscription** – The email was successfully delivered, but the recipient updated the subscription preferences by clicking `List-Unsubscribe` in the email header or the `Unsubscribe` link in the footer\.
+ **Open** – The recipient received the message and opened it in their email client\.
+ **Click** – The recipient clicked one or more links in the email\.

You can monitor email sending events in several ways\. The method you choose depends on the type of event you want to monitor, the granularity and level of detail you want to monitor it with, and the location where you want Amazon SES to publish the data\. You're required to use either feedback notifications or event publishing to track bounce and complaint events\. You can also choose to use multiple monitoring methods\. The characteristics of each method are listed in the following table\.


| Monitoring Method | Events You Can Monitor | How to Access the Data | Level of Detail | Granularity | 
| --- | --- | --- | --- | --- | 
|  Amazon SES console  |  Account health, emails sent, quota used, successful send requests, rejects, bounces & complaints *\(recent history to current reputation\)*  |  [Account dashboard page](monitor-sending-activity-console.md) in Amazon SES console  |  Count and percentage  |  Across entire AWS account  | 
|  Amazon SES console  |  Account health, emails sent, bounces & complaints *\(current reputation\)*  |  [Reputation metrics page](reputation-dashboard-dg.md) in Amazon SES console  |  Calculated rates only  |  Across entire AWS account  | 
|  Amazon SES API  |  Deliveries, bounces, complaints, and rejects  |  [https://docs.aws.amazon.com/ses/latest/APIReference/API_GetSendStatistics.html](https://docs.aws.amazon.com/ses/latest/APIReference/API_GetSendStatistics.html) API operation  |  Count only  |  Across entire AWS account  | 
|  Amazon CloudWatch console  |  Sends, deliveries, opens, clicks, bounces, bounce rate, complaints, complaint rate, rejects, redering failures, and blacklisted IPs\.  |  CloudWatch console  Some metrics don't appear in CloudWatch until the associated event occurs\. For example, bounce metrics don't appear in CloudWatch until at least one email that you send bounces, or until you generate a simulated bounce event by using the [mailbox simulator](send-an-email-from-console.md)\.   |  Count only  |  Across entire AWS account  | 
|  Feedback notifications  |  Deliveries, bounces, and complaints  |  Amazon SNS notification \(deliveries, bounces, and complaints\) or email \(bounces and complaints only\)  |  Details on each event  |  Across entire AWS account  | 
|  Event publishing  |  Sends, deliveries, opens, clicks, bounces, complaints, rejects, and rendering failures\.  |  Amazon CloudWatch or Amazon Kinesis Data Firehose, or by Amazon SNS notification  |  Details on each event  |  Fine\-grained \(based on user\-definable email characteristics\)  | 
|  Event publishing utilizing custom domains associated with configuration sets \- [more info](configure-custom-open-click-domains.md)  |  Open and click tracking\.  |  Amazon CloudWatch or Amazon Kinesis Data Firehose, or by Amazon SNS notification  |  Details on each event\.  |  Fine\-grained \(based on user\-definable email characteristics\)  | 

**Note**  
The metrics measured by email sending events may not align perfectly with your sending quotas\. This discrepancy can be caused by email bounces and rejections, or by using the Amazon SES inbox simulator\. To find out how close you are to your sending quotas, see [Monitoring your sending quotas](manage-sending-quotas-monitor.md)\.

**Topics**
+ [Monitoring your sending statistics using the Amazon SES console](monitor-sending-activity-console.md)
+ [Monitoring your usage statistics using the Amazon SES API](monitor-sending-activity-api.md)
+ [Monitor email sending using Amazon SES event publishing](monitor-using-event-publishing.md)