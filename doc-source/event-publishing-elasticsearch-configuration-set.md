# Step 3: Set up a Configuration Set<a name="event-publishing-elasticsearch-configuration-set"></a>

To set up Amazon SES to publish your email sending events to Amazon Kinesis Firehose, you first create a configuration set, and then you add a Kinesis Firehose event destination to the configuration set\. This section shows how to accomplish those tasks\.

If you already have a configuration set, you can add a Kinesis Firehose destination to your existing configuration set\. In this case, skip to [Adding a Kinesis Firehose Event Destination](#event-publishing-elasticsearch-configuration-set-add-destination)\.

## Creating a Configuration Set<a name="event-publishing-elasticsearch-configuration-set-create"></a>

The following procedure shows how to create a configuration set\.

**To create a configuration set**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the left navigation pane, choose **Configuration Sets**\.

1. In the content pane, choose **Create Configuration Set**\.

1. Type a name for the configuration set, and then choose **Create Configuration Set**\.

1. Choose **Close**\.

## Adding a Kinesis Firehose Event Destination<a name="event-publishing-elasticsearch-configuration-set-add-destination"></a>

The following procedure shows how to add a Kinesis Firehose event destination to the configuration set you created\.

**To add a Kinesis Firehose event destination to the configuration set**

1. Choose the configuration set from the configuration set list\.

1. For **Add Destination**, choose **Select a destination type**, and then choose **Kinesis Firehose**\.

1. For **Name**, type a name for the event destination\.

1. Select all **Event types**\.

1. Select **Enabled**\.

1. For **Stream**, choose the delivery stream that you created in [Step 2: Create a Kinesis Firehose Delivery Stream](event-publishing-elasticsearch-firehose-stream.md)\.

1. For **IAM role**, choose **Let SES make a new role**, and then type a name for the role\.

1. Choose **Save**\.

1. To exit the **Edit Configuration Set** page, use the back button of your browser\.

## Next Step<a name="event-publishing-elasticsearch-configuration-set-next-step"></a>

[Step 4: Send Emails](event-publishing-elasticsearch-send-email.md)