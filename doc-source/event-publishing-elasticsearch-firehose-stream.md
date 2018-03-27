# Step 2: Create a Kinesis Firehose Delivery Stream<a name="event-publishing-elasticsearch-firehose-stream"></a>

To publish email sending events to Amazon Kinesis Firehose, you must create a Kinesis Firehose delivery stream\. When you set up a Kinesis Firehose delivery stream, you choose where Kinesis Firehose publishes the data\. In this tutorial, we set up Kinesis Firehose to publish the data to Amazon Elasticsearch Service \(Amazon ES\)\.

 This section shows how to create a Kinesis Firehose delivery stream using the Kinesis Firehose console\. For the simplicity of this tutorial, we choose basic options\. For information about all available options, see [Creating an Amazon Kinesis Firehose Delivery Stream](http://docs.aws.amazon.com/firehose/latest/dev/basic-create.html) in the *Amazon Kinesis Data Firehose Developer Guide*\.

**Note**  
You must have already set up an Amazon ES cluster, as explained in [Step 1: Create an Amazon ES Cluster](event-publishing-elasticsearch-cluster.md)\.

**To create a delivery stream from Kinesis Firehose to Amazon Elasticsearch Service**

1. Sign in to the AWS Management Console and open the Kinesis Firehose console at [https://console\.aws\.amazon\.com/firehose/](https://console.aws.amazon.com/firehose/)\.

1. Choose **Create Delivery Stream**\.

1. On the **Destination** page, choose the following options\.
   + **Destination** – Choose Amazon Elasticsearch Service\.
   + **Delivery stream name** – Type a name for the delivery stream\.
   + **Elasticsearch domain** – Choose the Amazon ES domain that you created in [Step 1: Create an Amazon ES Cluster](event-publishing-elasticsearch-cluster.md)\.
   + **Index** – Type a name that you want to use to explore your email sending event data in Kibana\. You can choose any name, but let's use `holiday-sale` for this tutorial\. An *index* is analogous to a database\. For example, if you want an easy way to access events from each of your email campaigns separately, you can use a different Kinesis Firehose stream and index for each campaign\. 
   + **Index rotation** – Choose **NoRotation**\.
   + **Type** – Although this setting is not relevant to this tutorial, you must choose something, so type `events`\. A *type* is a logical category or partition of your index\. 
   + **Retry duration \(sec\)** – Type **300**\.
   + **Backup mode** – Choose **Failed Documents Only**\.
   + **S3 bucket** – Choose **New S3 Bucket**\. Type a name for the bucket and choose the region your console is currently using\.
   + **S3 prefix** – Leave this field empty\.

1. Choose **Next**\.

1. On the **Configuration** page, leave the fields at the default settings\. The only step you must do is select an IAM role that enables Kinesis Firehose to access your resources, as explained in the following procedure\.

   1. For **IAM Role**, choose **Select an IAM role**\.

   1. In the drop\-down menu, under **Create/Update existing IAM role**, choose **Firehose delivery IAM role**\.

      You will be taken to the IAM console\.

   1. In the IAM console, leave the fields at their default settings, and then choose **Allow**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/white_space_horizontal.png)  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/event_publishing_tutorial_firehose_iam.png)

      You will return to the Kinesis Firehose delivery stream set\-up steps in the Kinesis Firehose console\.

1. Choose **Next**\.

1. On the **Review** page, review your settings, and then choose **Create Delivery Stream**\.

## Next Step<a name="event-publishing-elasticsearch-firehose-stream-next-step"></a>

[Step 3: Set up a Configuration Set](event-publishing-elasticsearch-configuration-set.md)