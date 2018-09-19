# Step 1: Create a Kinesis Data Firehose Delivery Stream<a name="event-publishing-kinesis-analytics-firehose-stream"></a>

To analyze Amazon SES email sending events with Amazon Kinesis Data Analytics, you must configure Amazon SES to publish the events to an Amazon Kinesis Data Firehose delivery stream, and then configure Amazon Kinesis Data Analytics to get the event data from Kinesis Data Firehose\.

When you set up a Kinesis Data Firehose delivery stream, you choose the final destination of the data\. Your destination options are Amazon Simple Storage Service \(Amazon S3\), Amazon Elasticsearch Service, and Amazon Redshift\. If you simply want to analyze email sending events with Amazon Kinesis Data Analytics, it does not matter which destination you choose\. For this tutorial, we configure Kinesis Data Firehose to publish the data to Amazon S3, but you can use the other destination options if they are in the same region as your Amazon SES sending and Kinesis Data Firehose delivery stream\.

 This section shows how to create a Kinesis Data Firehose delivery stream using the Kinesis Data Firehose console\. For this tutorial, we choose basic options\. For information about all available options, see [Creating an Amazon Kinesis Firehose Delivery Stream](https://docs.aws.amazon.com/firehose/latest/dev/basic-create.html) in the *Amazon Kinesis Data Firehose Developer Guide*\.

**To create a delivery stream from Kinesis Data Firehose to Amazon S3**

1. Sign in to the AWS Management Console and open the Kinesis Data Firehose console at [https://console\.aws\.amazon\.com/firehose/](https://console.aws.amazon.com/firehose/)\.

1. Choose **Create Delivery Stream**\.

1. On the **Destination** page, choose the following options\.
   + **Destination** – Choose **Amazon S3**\.
   + **Delivery stream name** – Type a name for the delivery stream\.
   + **S3 bucket** – Choose an existing bucket, or choose **New S3 Bucket**\. If you create a new bucket, type a name for the bucket and choose the region your console is currently using\.
   + **S3 prefix** – Leave this field empty\.

1. Choose **Next**\.

1. On the **Configuration** page, leave the fields at the default settings\. The only required step is to select an IAM role that enables Kinesis Data Firehose to access your resources, as follows:

   1. For **IAM Role**, choose **Select an IAM role**\.

   1. In the drop\-down menu, under **Create/Update existing IAM role**, choose **Firehose delivery IAM role**\.

      You are taken to the IAM console\.

   1. In the IAM console, leave the fields at their default settings, and then choose **Allow**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/white_space_horizontal.png)  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/event_publishing_tutorial_firehose_iam.png)

      You return to the Kinesis Data Firehose delivery stream set\-up steps in the Kinesis Data Firehose console\.

1. Choose **Next**\.

1. On the **Review** page, review your settings, and then choose **Create Delivery Stream**\.

## Next Step<a name="event-publishing-kinesis-analytics-firehose-stream-next-step"></a>

[Step 2: Set up a Configuration Set](event-publishing-kinesis-analytics-configuration-set.md)