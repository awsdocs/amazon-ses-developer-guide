# Setting up Amazon SES event publishing<a name="monitor-sending-using-event-publishing-setup"></a>

This section describes what you need to do to configure Amazon SES to publish your email sending events to Amazon CloudWatch, Amazon Kinesis Data Firehose, or Amazon Simple Notification Service \(Amazon SNS\)\.

First, you create a *configuration set* using the Amazon SES console or API\. After you create a configuration set, you add one or more *event destinations* \(CloudWatch, Kinesis Data Firehose, or Amazon SNS\) to the configuration set, and configure parameters unique to the event destination\. Then, each time you send an email, you specify which configuration set to use\.

**Topics**
+ [Step 1: Create a configuration set](event-publishing-create-configuration-set.md)
+ [Step 2: Add an event destination](event-publishing-add-event-destination.md)
+ [Step 3: Specify your configuration set when you send email](event-publishing-send-email.md)