# Step 2: Add an Event Destination Using Amazon SES<a name="event-publishing-add-event-destination"></a>

Event destinations represent AWS services to which you publish email sending events such as bounces, complaints, deliveries, sent emails, and rejected emails\. Each event destination that you set up belongs to one, and only one, configuration set\. When you set up an event destination with Amazon SES, you choose the AWS service destination, and you specify parameters associated with that destination\. 

There are three event destinations: Amazon CloudWatch, Amazon Kinesis Firehose, and Amazon Simple Notification Service \(Amazon SNS\)\. The event destination that you choose depends on the level of detail you want about the events, and the way in which you want to receive the event information\. If you simply want a running total of each type of event \(for example, so that you can set an alarm when the total gets too high\), use CloudWatch\. If you want detailed event records that you can output to another service such as Amazon Elasticsearch Service or Amazon Redshift for analysis, choose Kinesis Firehose\. If you want to receive notifications when certain events occur, choose Amazon SNS\.

**Topics**
+ [Set Up a CloudWatch Event Destination for Amazon SES Event Publishing](event-publishing-add-event-destination-cloudwatch.md)
+ [Set Up a Kinesis Firehose Event Destination for Amazon SES Event Publishing](event-publishing-add-event-destination-firehose.md)
+ [Set Up an Amazon SNS Event Destination for Amazon SES Event Publishing](event-publishing-add-event-destination-sns.md)