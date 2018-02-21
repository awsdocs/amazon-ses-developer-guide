# Retrieving Amazon SES Event Data from Kinesis Firehose<a name="event-publishing-retrieving-firehose"></a>

Amazon SES publishes email sending events to Kinesis Firehose as JSON records\. Kinesis Firehose then publishes the records to the AWS service destination that you chose when you set up the delivery stream in Kinesis Firehose\. For information about setting up Kinesis Firehose delivery streams, see [Creating an Amazon Kinesis Firehose Delivery Stream](http://docs.aws.amazon.com/firehose/latest/dev/basic-create.html) in the *Amazon Kinesis Data Firehose Developer Guide*\. 

For examples of how you can use Kinesis Firehose to publish your email sending events to Amazon Redshift and Amazon Elasticsearch Service, see [Tutorials](event-publishing-tutorials.md)\.

For a description of the record contents and for example records, see the following sections\.

+ [Event Record Contents](event-publishing-retrieving-firehose-contents.md)

+ [Event Record Examples](event-publishing-retrieving-firehose-examples.md)