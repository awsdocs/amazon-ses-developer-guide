# Monitor email sending using Amazon SES event publishing<a name="monitor-using-event-publishing"></a>

To enable you to track your email sending at a granular level, you can set up Amazon SES to publish *email sending events* to Amazon CloudWatch, Amazon Kinesis Data Firehose, or Amazon Simple Notification Service based on characteristics that you define\.

You can track several types of email sending events, including sends, deliveries, opens, clicks, bounces, complaints, rejections, rendering failures, and delivery delays\. This information can be useful for operational and analytical purposes\. For example, you can publish your email sending data to CloudWatch and create dashboards that track the performance of your email campaigns, or you can use Amazon SNS to send you notifications when certain events occur\.

## How event publishing works<a name="event-publishing-how-works"></a>

To use event publishing, you first set up one or more *configuration sets*\. A configuration set specifies where to publish your events and which events to publish\. Then, each time you send an email, you provide the name of the configuration set and one or more *message tags*, in the form of name/value pairs, to categorize the email\. For example, if you advertise books, you could name a message tag *genre*, and assign a value of *sci\-fi* or *western*, when you send an email for the associated campaign\. Depending on which email sending interface you use, you either provide the message tag as a parameter to the API call or as an Amazon SES\-specific email header\. For more information about configuration sets, see [Using configuration sets in Amazon SES](using-configuration-sets.md)\.

In addition to the message tags that you specify, Amazon SES also adds *auto\-tags* to the messages you send\. You do not need to perform any additional steps to use auto\-tags\.

The following table lists the auto\-tags that are automatically applied to messages you send using Amazon SES\.


**Amazon SES Auto\-Tags**  

| Auto\-tag name | Description | 
| --- | --- | 
| ses:configuration\-set | The name of the Configuration Set associated with the email\. | 
| ses:caller\-identity | The IAM identity of the Amazon SES user who sent the email\. | 
| ses:from\-domain | The domain of the "From" address\. | 
| ses:source\-ip | The IP address that the caller used to send the email\. | 
| ses:outgoing\-ip | The IP address that Amazon SES used to send the email\. | 

## How to use event publishing<a name="event-publishing-how-to-use"></a>

The following sections contain the information you need to set up and use Amazon SES event publishing\.
+ [Setting up event publishing](monitor-sending-using-event-publishing-setup.md)
+ [Working with event data](working-with-event-data.md)
+ [Tutorials](event-publishing-tutorials.md)

## Event publishing terminology<a name="event-publishing-terminology"></a>

The following list defines terms related to Amazon SES event publishing\.

**Email sending event**  
Information associated with the outcome of an email you submit to Amazon SES\. Sending events include the following:  
+ **Sends** – The send request was successful and Amazon SES will attempt to deliver the message to the recipient’s mail server\. \(If account\-level or global suppression is being used, SES will still count it as a send, but delivery is suppressed\.\)
+ **Rendering Failures** – The email wasn't sent because of a template rendering issue\. This event type can occur when template data is missing, or when there is a mismatch between template parameters and data\. \(This event type only occurs when you send email using the [https://docs.aws.amazon.com/ses/latest/APIReference/API_SendTemplatedEmail.html](https://docs.aws.amazon.com/ses/latest/APIReference/API_SendTemplatedEmail.html) or [https://docs.aws.amazon.com/ses/latest/APIReference/API_SendBulkTemplatedEmail.html](https://docs.aws.amazon.com/ses/latest/APIReference/API_SendBulkTemplatedEmail.html) API operations\.\)
+ **Rejects** – Amazon SES accepted the email, but determined that it contained a virus and didn’t attempt to deliver it to the recipient’s mail server\.
+ **Deliveries** – Amazon SES successfully delivered the email to the recipient's mail server\.
+ **Hard bounces** – The recipient's mail server permanently rejected the email\. \(*Soft bounces* are only included when Amazon SES fails to deliver the email after retrying for a period of time\.\)
+ **Complaints** – The email was successfully delivered to the recipient’s mail server, but the recipient marked it as spam\.
+ **Delivery Delays** – The email couldn't be delivered to the recipient’s mail server because a temporary issue occurred\. Delivery delays can occur, for example, when the recipient's inbox is full, or when the receiving email server experiences a transient issue\.
+ **Subscriptions** – The email was successfully delivered, but the recipient updated the subscription preferences by clicking `List-Unsubscribe` in the email header or the `Unsubscribe` link in the footer\.
+ **Opens** – The recipient received the message and opened it in their email client\.
+ **Clicks** – The recipient clicked one or more links in the email\.

**Configuration set**  
A set of rules that defines the destination that Amazon SES publishes email sending events to, and the types of email sending events that you want to publish\. When you send an email that you want to use with event publishing, you specify the configuration set to associate with the email\.

**Event destination**  
An AWS service that you publish Amazon SES email sending events to\. Each event destination that you set up belongs to one, and only one, configuration set\.

**Message tag**  
A name/value pair that you use to categorize an email for the purpose of event publishing\. Examples are *campaign/book* and *campaign/clothing*\. When you send an email, you either specify the message tag as a parameter to the API call or as an Amazon SES\-specific email header\.

**Auto\-tag**  
Message tags that are automatically included in event publishing reports\. There is an auto\-tag for the configuration set name, the domain of the "From" address, the caller's outgoing IP address, the Amazon SES outgoing IP address, and the IAM identity of the caller\.