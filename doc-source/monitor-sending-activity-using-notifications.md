# Monitoring Using Amazon SES Notifications<a name="monitor-sending-activity-using-notifications"></a>

In order to send email using Amazon SES, you must have a system in place for managing bounces and complaints\. Amazon SES can notify you of bounce or complaint events in three ways: by sending a notification email, by notifying an Amazon SNS topic, or by publishing sending events\. This section contains information about setting up Amazon SES to send certain kinds of notifications by email or by notifying an Amazon SNS topic\. For more information about publishing sending events, see [Monitoring Using Amazon SES Event Publishing](monitor-using-event-publishing.md)\.

You can set up notifications using the Amazon SES console or the Amazon SES API\.

**Topics**
+ [Important Considerations](#monitor-sending-activity-using-notifications-considerations)
+ [Amazon SES Notifications Through Email](monitor-sending-activity-using-notifications-email.md)
+ [Amazon SES Notifications Through Amazon SNS](monitor-sending-activity-using-notifications-sns.md)

## Important Considerations<a name="monitor-sending-activity-using-notifications-considerations"></a>

There are several important points to consider when you set up Amazon SES to send notifications:
+ Email and Amazon SNS notifications apply to individual identities \(the verified email addresses or domains you use to send email\)\. When you enable notifications for an identity, Amazon SES only sends notifications for emails sent from that identity, and only in the AWS Region you configured notifications in\.
+ You have to enable one method of receiving bounce or complaint notifications\. You can send notifications to the domain or email address that generated the bounce or complaint, or to an Amazon SNS topic\. You can also use [event publishing](monitor-using-event-publishing.md) to send notifications about several different types of events \(including bounces, complaints, deliveries, and more\) to an Amazon SNS topic or an Kinesis Data Firehose stream\.

  If you don't set up one of these methods of receiving bounce or complaint notifications, Amazon SES automatically forwards bounce and complaint notifications to the Return\-Path address \(or the Source address, if you didn't specify a Return\-Path address\) in the email that resulted in the bounce or complaint event, even if you disabled email feedback forwarding\.

  If you disable email feedback forwarding and enable event publishing, you must apply the configuration set that contains the event publishing rule to all emails you send\. In this situation, if you don't use the configuration set, Amazon SES automatically forwards bounce and complaint notifications to the Return\-Path or Source address in the email that resulted in the bounce or complaint event\.
+ If you set up Amazon SES to send bounce and complaint events using more than one method \(such as by sending email notifications and by using sending events\), you may receive more than one notification for the same event\.