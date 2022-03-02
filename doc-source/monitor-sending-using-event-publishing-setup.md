# Setting up Amazon SES event publishing<a name="monitor-sending-using-event-publishing-setup"></a>

This section describes what you need to do to configure Amazon SES to publish your email sending events to the following AWS services:
+ Amazon CloudWatch
+ Amazon Kinesis Data Firehose
+ Amazon Pinpoint
+ Amazon Simple Notification Service \(Amazon SNS\)

The following steps required for setting up event publishing are covered in the topics below:

1. You must create a *configuration set* using the Amazon SES console or API\.

1. Add one or more *event destinations* \(CloudWatch, Kinesis Data Firehose, Pinpoint, or SNS\) to the configuration set, and configure parameters unique to the event destination\.

1. When you send an email, you specify which configuration set to use that contains your event destination\.

**Topics**
+ [Step 1: Create a configuration set](event-publishing-create-configuration-set.md)
+ [Step 2: Add an event destination](event-publishing-add-event-destination.md)
+ [Step 3: Specify your configuration set when you send email](event-publishing-send-email.md)