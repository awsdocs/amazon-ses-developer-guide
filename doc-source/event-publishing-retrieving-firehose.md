# Retrieving Amazon SES event data from Kinesis Data Firehose<a name="event-publishing-retrieving-firehose"></a>

Amazon SES publishes email sending events to Kinesis Data Firehose as JSON records\. Kinesis Data Firehose then publishes the records to the AWS service destination that you chose when you set up the delivery stream in Kinesis Data Firehose\. For information about setting up Kinesis Data Firehose delivery streams, see [Creating an Kinesis Data Firehose Delivery Stream](https://docs.aws.amazon.com/firehose/latest/dev/basic-create.html) in the *Amazon Kinesis Data Firehose Developer Guide*\. 

For examples of how you can use Kinesis Data Firehose to publish your email sending events to Amazon Redshift and Amazon OpenSearch Service, see [Tutorials](event-publishing-tutorials.md)\.

**Topics**
+ [Contents of event data that Amazon SES publishes to Kinesis Data Firehose](event-publishing-retrieving-firehose-contents.md)
+ [Examples of event data that Amazon SES publishes to Kinesis Data Firehose](event-publishing-retrieving-firehose-examples.md)