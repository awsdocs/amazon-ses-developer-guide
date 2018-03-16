# Monitoring Using Amazon SES Event Publishing<a name="monitor-using-event-publishing"></a>

To enable you to track your email sending at a granular level, you can set up Amazon SES to publish *email sending events* to Amazon CloudWatch or Amazon Kinesis Firehose based on fine\-grained email characteristics that you define\. For example, you can categorize your emails by purpose \(transactional versus marketing\), product details, the recipient's "From" domain, and so on\.

You can track several types of email sending events, including sends, deliveries, opens, clicks, bounces, complaints, and rejections\. This information can be useful for operational and analytical purposes\. For example, you can publish your email sending data to CloudWatch and create dashboards that track the performance of your email campaigns\.

## How Event Publishing Works<a name="event-publishing-how-works"></a>

To use event publishing, you first set up one or more *configuration sets*\. A configuration set specifies where to publish your events and which events to publish\. Then, each time you send an email, you provide the name of the configuration set and one or more *message tags*, in the form of name/value pairs, to categorize the email\. For example, if you advertise books, you could name a message tag *genre*, and assign a value of *sci\-fi* or *western*, when you send an email for the associated campaign\. Depending on which email sending interface you use, you either provide the message tag as a parameter to the API call or as an Amazon SES\-specific email header\. For more information about configuration sets, see [Using Amazon SES Configuration Sets](using-configuration-sets.md)\.

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

## How to Use Event Publishing<a name="event-publishing-how-to-use"></a>

The following sections contain the information you need to set up and use Amazon SES event publishing\.

+ [Setting Up Event Publishing](event-publishing-setting-up.md)

+ [Working with Event Data](working-with-event-data.md)

+ [Tutorials](event-publishing-tutorials.md)

## Event Publishing Terminology<a name="event-publishing-terminology"></a>

The following list defines terms related to Amazon SES event publishing\.

**Email sending event**  
Information associated with the outcome of an email you submit to Amazon SES\. Sending events include the following:  

+ **Sends** – The call to Amazon SES was successful and Amazon SES will attempt to deliver the email\.

+ **Rejects** – Amazon SES accepted the email, determined that it contained a virus, and rejected it\. Amazon SES didn't attempt to deliver the email to the recipient's mail server\.

+ **Bounces** – The recipient's mail server permanently rejected the email\. This event corresponds to hard bounces\. Soft bounces are only included when Amazon SES fails to deliver the email after retrying for a period of time\.

+ **Complaints** – The email was successfully delivered to the recipient\. The recipient marked the email as spam\.

+ **Deliveries** – Amazon SES successfully delivered the email to the recipient's mail server\.

+ **Opens** – The recipient received the message and opened it in his or her email client\.

+ **Clicks** – The recipient clicked one or more links contained in the email\.

+ **Rendering Failures** – The email was not sent because of a template rendering issue\. This event type only occurs when you send email using the [SendTemplatedEmail](http://docs.aws.amazon.com/ses/latest/APIReference/API_SendTemplatedEmail.html) or [SendBulkTemplatedEmail](http://docs.aws.amazon.com/ses/latest/APIReference/API_SendBulkTemplatedEmail.html) API operations\. This event type can occur when template data is missing, or when there is a mismatch between template parameters and data\.

**Configuration set**  
An Amazon SES construct that encapsulates where you want to publish email sending events, and what email sending events you want to publish\. When you send an email that you want to use with event publishing, you specify the configuration set to associate with the email\.

**Event destination**  
An Amazon SES construct that represents an AWS service to which you publish Amazon SES email sending events\. Each event destination that you set up belongs to one, and only one, configuration set\.

**Message tag**  
A name/value pair that you use to categorize an email for the purpose of event publishing\. Examples are *campaign/book* and *campaign/clothing*\. When you send an email, you either specify the message tag as a parameter to the API call or as an Amazon SES\-specific email header\.

**Auto\-tag**  
Message tags that are automatically included in event publishing reports\. There is an auto\-tag for the configuration set name, the domain of the "From" address, the caller's outgoing IP address, the Amazon SES outgoing IP address, and the IAM identity of the caller\.


****  

|  | 
| --- |
| For technical discussions about various Amazon SES topics, visit the [Amazon SES Blog](https://aws.amazon.com//blogs/ses/)\. To browse and post questions, go to the [Amazon SES Forum](https://forums.aws.amazon.com/forum.jspa?forumID=90)\. | 