# Step 5: Set up a Configuration Set<a name="event-publishing-redshift-configuration-set"></a>

To set up Amazon SES to publish your email sending events to Amazon Kinesis Data Firehose, you first create a configuration set, and then you add a Kinesis Data Firehose event destination to the configuration set\. This section shows how to accomplish those tasks\.

If you already have a configuration set, you can add a Kinesis Data Firehose destination to your existing configuration set\. In this case, skip to [Adding a Kinesis Data Firehose Event Destination](#event-publishing-redshift-configuration-set-add-destination)\.

## Creating a Configuration Set<a name="event-publishing-redshift-configuration-set-create"></a>

If you haven't already created a configuration set, or you want to create a new one to use for publishing your email sending events to Amazon Kinesis Data Firehose, follow the [Create a configuration set](creating-configuration-sets.md#config-sets-create-console) procedures\.

## Adding a Kinesis Data Firehose Event Destination<a name="event-publishing-redshift-configuration-set-add-destination"></a>

To add a Kinesis Data Firehose event destination to your configuration set, refer to the [Set Up a Kinesis Data Firehose destination](event-publishing-add-event-destination-firehose.md) procedures\.

## Next Step<a name="event-publishing-redshift-configuration-set-next-step"></a>

 [Step 6: Send Emails](event-publishing-redshift-send-email.md) 