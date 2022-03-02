# Step 2: Set up a Configuration Set<a name="event-publishing-kinesis-analytics-configuration-set"></a>

To set up Amazon SES to publish your email sending events to Amazon Kinesis Data Firehose, you create a configuration set, and then you add a Kinesis Data Firehose event destination to the configuration set\. This section describes how to accomplish those tasks\.

If you already have a configuration set, you can add a Kinesis Data Firehose event destination to your existing configuration set\.

**To add a Kinesis Data Firehose event destination to the configuration set**

1. Choose the configuration set\.

1. Choose **Event destinations**, **Add destination**\.

1. For **Event types**, choose **Select all**\. Choose **Next**\.

1. For **Destination type**, choose **Amazon Kinesis Data Firehose**\.

1. For **Name**, type a name for the event destination\.

1. For **Delivery stream**, choose the delivery stream that you created in [Step 1: Create a Kinesis Data Firehose Delivery Stream](event-publishing-kinesis-analytics-firehose-stream.md)\.

1. For **IAM role**, choose an existing role that grants Amazon SES permission to publish to Kinesis Data Firehose on your behalf, or choose **Create a new role in IAM**\. For more information, see [Giving Amazon SES Permission to Publish to Your Kinesis Data Firehose Delivery Stream](event-publishing-add-event-destination-firehose.md#event-publishing-add-event-destination-firehose-role)\. Choose **Next**\.

1. Choose **Add destination**\.

## Next step<a name="event-publishing-kinesis-analytics-configuration-set-next-step"></a>

[Step 3: Send Emails](event-publishing-kinesis-analytics-send-email.md)