# Amazon SES notifications sent through Amazon SNS<a name="monitor-sending-activity-using-notifications-sns"></a>

You can configure Amazon SES to notify an Amazon SNS topic when you receive bounces or complaints, or when emails are delivered\. Amazon SNS notifications are in [JavaScript Object Notation \(JSON\)](http://www.json.org) format, which enables you to process them programmatically\.

In order to send email using Amazon SES, you must configure it to send bounce and complaint notifications by using one of the following methods:
+ By sending notifications to an Amazon SNS topic\. The procedure for setting up this type of notification is included in this section\.
+ By enabling email feedback forwarding\. For more information, see [Amazon SES notifications sent by email](monitor-sending-activity-using-notifications-email.md)\.
+ By publishing event notifications\. For more information, see [Monitor email sending using Amazon SES event publishing](monitor-using-event-publishing.md)\.

**Important**  
See [Monitoring Amazon SES email sending using notifications](monitor-sending-activity-using-notifications.md) for important information about notifications\.

**Topics**
+ [Configuring Amazon SNS notifications for Amazon SES](configure-sns-notifications.md)
+ [Amazon SNS notification contents for Amazon SES](notification-contents.md)
+ [Amazon SNS notification examples for Amazon SES](notification-examples.md)
