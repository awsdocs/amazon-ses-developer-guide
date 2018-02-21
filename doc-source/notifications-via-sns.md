# Amazon SES Notifications Through Amazon SNS<a name="notifications-via-sns"></a>

You can use [Amazon Simple Notification Service \(Amazon SNS\)](https://aws.amazon.com/sns) to receive notifications about bounces, complaints, and deliveries for emails that you send through Amazon SES\. Amazon SNS notifications are in [JavaScript Object Notation \(JSON\)](http://www.json.org) format, which enables you to process them programmatically\.

If you configure Amazon SNS notifications for both bounces and complaints, you can disable receiving bounce and complaint notifications by email entirely\. For information about receiving bounce and complaint notifications by email, see [Amazon SES Notifications Through Email](notifications-via-email.md)\. Delivery notifications are available only through Amazon SNS\. 

**Important**  
For several important points about notifications, see [Monitoring Using Amazon SES Notifications](monitor-sending-using-notifications.md)\.

For information about Amazon SES bounce, complaint, and delivery notifications through Amazon SNS, see the following sections:

+ To set up notifications using the Amazon SES console or the Amazon SES API, see [Configuring Amazon SNS Notifications for Amazon SES](configure-sns-notifications.md)\.

+ For a description of the contents of a notification, see [Amazon SNS Notification Contents for Amazon SES](notification-contents.md)\.

+ For examples of bounce, complaint, and delivery notifications, see [Amazon SNS Notification Examples for Amazon SES](notification-examples.md)\.