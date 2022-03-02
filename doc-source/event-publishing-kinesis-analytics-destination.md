# \(Optional\) Step 6: Save SQL Query Results<a name="event-publishing-kinesis-analytics-destination"></a>

You can set up your Amazon Kinesis Data Analytics application to write the output of your SQL queries to an Amazon Kinesis Data Firehose delivery stream\. To do so, you must create another Kinesis Data Firehose delivery stream because you cannot use the same delivery stream as both the source and destination of an Amazon Kinesis Data Analytics application\. As with any Kinesis Data Firehose delivery stream, you can choose Amazon Simple Storage Service \(Amazon S3\), Amazon OpenSearch Service, or Amazon Redshift as the destination\.

The following procedure shows how to configure Amazon Kinesis Data Analytics to save SQL query results in JSON format to a Kinesis Data Firehose delivery stream that writes the data to Amazon S3\. Then you run a SQL query and access the saved data\.

**To save the results of SQL queries to Amazon S3**

1. Set up a new Kinesis Data Firehose stream that uses Amazon S3 as the destination\. It is the same procedure as [Step 1: Create a Kinesis Data Firehose Delivery Stream](event-publishing-kinesis-analytics-firehose-stream.md)\.

1. Go to the [Amazon Kinesis Data Analytics console](https://console.aws.amazon.com/kinesisanalytics), choose the arrow next to your application, and then choose **Application details**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/dg/images/event_publishing_tutorial_kinesis_analytics_expand_app.png)

1. Choose **Connect to a destination**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/dg/images/event_publishing_tutorial_kinesis_analytics_connect.png)

1. Choose the Kinesis Data Firehose stream you created in step 1, leave the rest of the options at their default settings, and then choose **Save and continue**\.

   In several seconds, you return to the main page of the application\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/dg/images/event_publishing_tutorial_kinesis_analytics_destination.png)

1. Choose **Go to SQL results**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/dg/images/event_publishing_tutorial_kinesis_analytics_go_to_sql.png)

1. Choose **Save and run SQL** to re\-run the query you ran in [Step 5: Run a SQL Query](event-publishing-kinesis-analytics-sql.md)\.

   Amazon Kinesis Data Analytics attempts to process event data it receives from the Kinesis Data Firehose delivery stream\. If you encounter the **No rows have arrived yet** error, ensure that you are still sending emails so that Amazon Kinesis Data Analytics has email sending events to process\.

   As Amazon Kinesis Data Analytics processes records, results appear in the **Real\-time analytics** tab\. Amazon Kinesis Data Analytics automatically saves the results to the Amazon S3 bucket that you specified when you set up the Kinesis Data Firehose delivery stream in step 1\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/dg/images/event_publishing_tutorial_kinesis_analytics_sql.png)

1. To retrieve the results, go to the [Amazon S3 console](https://console.aws.amazon.com/s3/)\.

1. Choose the Amazon S3 bucket that is associated with the Kinesis Data Firehose delivery stream that the Amazon Kinesis Data Analytics application uses as its destination\.

1. Navigate to the data, which, by default, is organized in a folder hierarchy based on the date the results are saved to the bucket\.

   If the bucket is empty, wait a few minutes and try again\. It can take several minutes for data to get from Amazon Kinesis Data Analytics to your Amazon S3 bucket\.

1. Choose a file, and then from the **Actions** menu, choose **Download**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/dg/images/event_publishing_tutorial_kinesis_analytics_s3.png)

1. Follow the on\-screen instructions to download the file to your computer\.

1. On your computer, open the file with a text editor\. The records are in JSON format, and each record is contained in curly braces\. The following is an example of a file that contains two records\.

   ```
   1. {"eventType":"Send","ses_timestamp":"2016-12-08 18:51:12.092","messageId":"EXAMPLE8dfc6695c-5f048b74-ca83-4052-8348-4e7da9669fc3-000000","ses_to":"[\"success@simulator.amazonses.com\" ]","ses_configuration_set":"[\"MyConfigSet\" ]"}{"eventType":"Send","ses_timestamp":"2016-12-08 18:50:42.181","messageId":"EXAMPLEdfc5f485-d40a2543-2cac-4b84-8a8f-30bebdf3820c-000000","ses_to":"[\"success@simulator.amazonses.com\" ]","ses_configuration_set":"[\"MyConfigSet\" ]"}
   ```