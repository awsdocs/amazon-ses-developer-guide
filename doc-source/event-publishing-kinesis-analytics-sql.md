# Step 5: Run a SQL Query<a name="event-publishing-kinesis-analytics-sql"></a>

Now that you have created an Amazon Kinesis Data Analytics application and configured it to use your Amazon Kinesis Firehose delivery stream as its source, you can query the email sending event data that the Kinesis Firehose delivery stream receives\.

This topic shows how to run a SQL query on the email sending event data\.

**Important**  
This procedure requires that you continue to send a steady stream of emails configured for event publishing through Amazon SES, as described in [Step 3: Send Emails](event-publishing-kinesis-analytics-send-email.md)\.

**To run a SQL query in Amazon Kinesis Data Analytics**

1. Assuming that you have moved on to this procedure after completing the last step, go to the Amazon Kinesis Data Analytics console top menu and choose your application\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/event_publishing_tutorial_kinesis_analytics_select_app.png)

1. Choose **Go to SQL editor**\. 

   Amazon Kinesis Data Analytics attempts to read event data from the Kinesis Firehose stream\. If you encounter the **No rows in source stream** error, ensure that you are still sending the email stream you started at the beginning of this procedure, and then choose **Retrieve rows**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/event_publishing_tutorial_kinesis_analytics_go_to_editor.png)

1. In the code editor box, paste the following\.

   ```
   1. CREATE OR REPLACE STREAM "DESTINATION_SQL_STREAM" ("eventType" VARCHAR(16), "ses_timestamp" timestamp, "messageId" VARCHAR(64), "ses_to" VARCHAR(64), "ses_configuration_set" VARCHAR(32));
   2. 
   3. CREATE OR REPLACE PUMP "STREAM_PUMP" AS INSERT INTO "DESTINATION_SQL_STREAM"
   4. 
   5. SELECT STREAM "eventType", "ses_timestamp", "messageId", "ses_to", "ses_configuration_set"
   6. FROM "SOURCE_SQL_STREAM_001"
   7. WHERE "eventType" = 'Send'
   ```

1. Choose **Save and run SQL**\.

   After Amazon Kinesis Data Analytics retrieves and processes incoming records, you see a list of event records of type "Send"\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/event_publishing_tutorial_kinesis_analytics_sql.png)

## Next Step<a name="event-publishing-kinesis-analytics-sql-next-step"></a>

[\(Optional\) Step 6: Save SQL Query Results](event-publishing-kinesis-analytics-destination.md)