# Step 4: Create an Amazon Kinesis Data Analytics Application<a name="event-publishing-kinesis-analytics-application"></a>

Now that you have set up event publishing with Amazon SES, you can configure Amazon Kinesis Data Analytics to capture the email sending event data from your Amazon Kinesis Firehose delivery stream\. To do this, you create an Amazon Kinesis Data Analytics application\.

The following procedure shows how to use the Amazon Kinesis Data Analytics console to create an application that captures Amazon SES email sending event data from your Kinesis Firehose delivery stream, and then how to perform a simply SQL query on the data to return the events of type "Send"\.

**Note**  
The email sending events of different event types \(send, bounce, complaint, and delivery\) have [different JSON schemas](event-publishing-retrieving-firehose-contents.md)\. In a production environment, you might examine several fields of this schema, but in this tutorial, we limit our examination to a small set of fields that are present for all event types\.

**To create an Amazon Kinesis Data Analytics application**

1. Start sending a steady stream of emails configured for event publishing through Amazon SES, and continue sending the emails throughout this procedure\. This is required so that Amazon Kinesis Data Analytics can automatically detect the schema of the event records\. Sending one email every ten seconds for five minutes is sufficient for this tutorial\. For more information, see [Step 3: Send Emails](event-publishing-kinesis-analytics-send-email.md)\.

   After your email program has sent a few emails, move to the next step\.

1. Sign in to the AWS Management Console and open the Kinesis Data Analytics console at [ https://console\.aws\.amazon\.com/kinesisanalytics](https://console.aws.amazon.com/kinesisanalytics)\.

1. Choose **Create new application**\.

1. Enter an application name and description, and then choose **Save and continue**\.

1. Choose **Connect to a source**\.

1. Choose the Kinesis Firehose stream you created in [Step 2: Set up a Configuration Set](event-publishing-kinesis-analytics-configuration-set.md)\.

   Amazon Kinesis Data Analytics attempts to discover the schema of the email sending event records based on the incoming records\. If Amazon Kinesis Data Analytics displays **Error discovering input schema**, that means that Amazon Kinesis Data Analytics has not received any email sending records yet\. Choose **Rediscover schema**\. You might need to choose this button several times\. If schema discovery does not succeed after several attempts, ensure that your email sending application is steadily sending emails, and that the emails specify a configuration set\.

   When Amazon Kinesis Data Analytics detects a schema, it displays a success message and lists the records it detected\.
**Important**  
Do not choose **Save and continue**\. This will cause errors because the discovered schema does not adhere to SQL naming constraints\. You must edit the schema as described in the next step\.

1. Choose **Edit schema**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/event_publishing_tutorial_kinesis_analytics_schema_discovery.png)

1. For this tutorial, we remove most of the rows\. Choose **X** next to all rows *except* rows with the following column names:
   + eventType
   + timestamp
   + messageId
   + to
   + ses:configuration\-set
**Important**  
Do not choose **Save schema and update stream samples**\. This will cause errors because the discovered schema does not adhere to SQL naming constraints\. You must edit the schema as described in the next step\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/event_publishing_tutorial_kinesis_analytics_schema_remove.png)

1. Examine the remaining entries under **Column name** and compare them to the SQL naming requirements as follows:
   + **Format** – As described in [Identifiers](http://docs.aws.amazon.com/kinesisanalytics/latest/sqlref/sql-reference-identifiers.html) in the *Amazon Kinesis Data Analytics SQL Reference*, unquoted identifiers must start with a letter or underscore, and be followed by letters, digits, or underscores\. Amazon SES auto\-tag names do not comply with these requirements because they contain colons and dashes\. You will edit these in the next step\. 
   + **Reserved words** – Column names must not conflict with the SQL reserved words listed in [Reserved Words and Keywords](http://docs.aws.amazon.com/kinesisanalytics/latest/sqlref/sql-reference-reserved-words-keywords.html) in the *Amazon Kinesis Data Analytics SQL Reference*\. Examples of reserved keywords that conflict with Amazon SES event records are `timestamp`, `value`, `date`, `from`, and `to`\. 

1. Edit the remaining column names to conform to the SQL requirements as follows:
   + Rename `ses:configuration-set` to `ses_configuration_set`\.
   + Rename `timestamp` to `ses_timestamp`\.
   + Rename `to` to `ses_to`\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/white_space_horizontal.png)  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/event_publishing_tutorial_kinesis_analytics_schema_edit.png)

1. Choose **Save schema and update stream samples**\. If you encounter validation errors, ensure that you correctly performed step 10\. If you encounter the **No rows in source stream** error, ensure that you are still sending the email stream that you started at the beginning of this procedure, and then choose **Retrieve rows**\. You might need to choose **Retrieve rows** several times before Amazon Kinesis Data Analytics captures records\.

1. Upon successful retrieval of rows, choose **Exit \(done\)**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/event_publishing_tutorial_kinesis_analytics_exit.png)

## Next Step<a name="event-publishing-kinesis-analytics-application-next-step"></a>

[Step 5: Run a SQL Query](event-publishing-kinesis-analytics-sql.md)