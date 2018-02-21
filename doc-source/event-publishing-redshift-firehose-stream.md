# Step 4: Create a Kinesis Firehose Delivery Stream<a name="event-publishing-redshift-firehose-stream"></a>

To publish email sending events to Amazon Kinesis Firehose, you must create a Kinesis Firehose delivery stream\. When you set up a Kinesis Firehose delivery stream, you choose where Kinesis Firehose publishes the data\. For this tutorial, we will set up Kinesis Firehose to publish the data to Amazon Redshift, and choose to have Kinesis Firehose publish the records to Amazon S3 as an intermediary step\. In the process, we need to specify how Amazon Redshift should copy records from Amazon S3 into the table we created in the previous step\.

This section shows how to create a Kinesis Firehose delivery stream that sends data to Amazon Redshift, and how to edit the delivery stream to specify how Amazon Redshift should copy the Amazon SES event publishing data to Amazon S3\.

**Note**  
You must have already set up the Amazon Redshift cluster, connected to your cluster, and created a database table, as explained previous steps\.

## Creating a Kinesis Firehose Delivery Stream<a name="event-publishing-redshift-firehose-stream-create"></a>

The following procedure shows how to create a Kinesis Firehose delivery stream that publishes data to Amazon Redshift, using Amazon S3 as the intermediary data location\.

**To create a delivery stream from Kinesis Firehose to Amazon Redshift**

1. Sign in to the AWS Management Console and open the Kinesis Firehose console at [https://console\.aws\.amazon\.com/firehose/](https://console.aws.amazon.com/firehose/)\.

1. Choose **Create Delivery Stream**\.

1. On the **Destination** page, choose the following options\.

   + **Destination** – Choose Amazon Redshift\.

   + **Delivery stream name** – Type a name for the delivery stream\.

   + **S3 bucket** – Choose **New S3 bucket**, type a bucket name, choose the region, and then choose **Create Bucket**\.

   + **Redshift cluster** – Choose the Amazon Redshift cluster that you created in a previous step\.

   + **Redshift database** – Type **dev**, which is the default database name\.

   + **Redshift table** – Type **ses**, which is the table you created in [Step 3: Create a Database Table](event-publishing-redshift-table.md)\.

   + **Redshift table columns** – Leave this field empty\.

   + **Redshift username** – Type the username that you chose when you set up the Amazon Redshift cluster\.

   + **Redshift password** – Type the password that you chose when you set up the Amazon Redshift cluster\.

   + **Redshift COPY options** – Leave this field empty\.

   + **Retry duration** – Leave this at its default value\.

   + **COPY command** – Leave this at its default value\. You will update it in the next procedure\.

1. Choose **Next**\.

1. On the **Configuration** page, leave the fields at the default settings for this simple tutorial\. The only step you must do is select an IAM role that enables Kinesis Firehose to access your resources, as explained in the following procedure\.

   1. For **IAM Role**, choose **Select an IAM role**\.

   1. In the drop\-down menu, under **Create/Update existing IAM role**, choose **Firehose delivery IAM role**\.

      You will be taken to the IAM console\.

   1. In the IAM console, leave the fields at their default settings, and then choose **Allow**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/white_space_horizontal.png)  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/event_publishing_tutorial_firehose_iam.png)

      You will return to the Kinesis Firehose delivery stream set\-up steps in the Kinesis Firehose console\.

1. Choose **Next**\.

1. On the **Review** page, review your settings, and then choose **Create Delivery Stream**\.

## Setting Amazon Redshift Copy Options<a name="event-publishing-redshift-firehose-stream-copy"></a>

Next, you must specify to Amazon Redshift how to copy the Amazon SES event publishing JSON records into the database table you created in [Step 3: Create a Database Table](event-publishing-redshift-table.md)\. You do this by editing the copy options in the Kinesis Firehose delivery stream\.

For this procedure, you must create a *JSONPaths file*\. A JSONPaths file is a text file that specifies to the Amazon Redshift COPY command how to parse the JSON source data\. We provide a JSONPaths file in the procedure\. For more information about JSONPaths files, see [COPY from JSON Format](http://docs.aws.amazon.com/redshift/latest/dg/copy-usage_notes-copy-from-json.html) in the *Amazon Redshift Database Developer Guide*\.

You upload the JSONPaths file to the Amazon S3 bucket you set up when you created the Kinesis Firehose delivery stream, and then edit the COPY options of the Kinesis Firehose delivery stream to use the JSONPaths file you uploaded\. These steps are explained in the following procedure\. 

**To set Amazon Redshift COPY command options**

1. **Create a JSONPaths file** – On your computer, create a file called *jsonpaths\.json*\. Copy the following text into the file, and then save the file\.

   ```
    1. {
    2.     "jsonpaths": [
    3.         "$.mail.messageId",
    4.         "$.eventType",
    5.         "$.mail.sendingAccountId",
    6.         "$.mail.timestamp",
    7.         "$.mail.destination",
    8.         "$.mail.tags.ses:configuration-set",
    9.         "$.mail.tags.campaign"
   10.     ]
   11. }
   ```

1. **Upload the JSONPaths file to the Amazon S3 bucket** – Go to the [Amazon S3 console](https://console.aws.amazon.com/s3/) and upload the file to the bucket you created when you set up the Kinesis Firehose delivery stream in [Creating a Kinesis Firehose Delivery Stream](#event-publishing-redshift-firehose-stream-create)\.

1. **Set the COPY command in the Kinesis Firehose delivery stream settings** – Now you have the information you need to set the syntax of the COPY command that Amazon Redshift uses when it puts your data in the table you created\. The following procedure shows how to update the COPY command information in the Kinesis Firehose delivery stream settings\.

   1. Go to the [Kinesis Firehose console](https://console.aws.amazon.com/firehose/)\.

   1. Under **Redshift Delivery Streams**, choose the Kinesis Firehose delivery stream that you created for Amazon SES event publishing\.

   1. On the **Details** page, choose **Edit**\.

   1. In the **Redshift COPY options** box, type the following text, replacing the following values with your own values:

      + **S3\-BUCKET\-NAME** – The name of the Amazon S3 bucket where Kinesis Firehose places your data for Amazon Redshift to access\. You created this bucket when you set up your Kinesis Firehose delivery stream in [Step 4: Create a Kinesis Firehose Delivery Stream](#event-publishing-redshift-firehose-stream)\. An example is `my-bucket`\.

      + **REGION** – The region in which your Amazon SES, Kinesis Firehose, Amazon S3, and Amazon Redshift resources are located\. An example is `us-west-2`\.

      ```
      1. json 's3://S3-BUCKET-NAME/jsonpaths.json' region 'REGION';
      ```

   1. Choose **Save**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/white_space_horizontal.png)  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/event_publishing_tutorial_redshift_firehose.png)

## Next Step<a name="event-publishing-redshift-firehose-stream-next-step"></a>

[Step 5: Set up a Configuration Set](event-publishing-redshift-configuration-set.md)