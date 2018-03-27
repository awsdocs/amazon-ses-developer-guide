# Analyze Email Sending Events With Amazon Kinesis Data Analytics<a name="event-publishing-kinesis-analytics"></a>

Amazon Kinesis Data Analytics enables you to process and analyze streaming data using SQL\. You can use Amazon Kinesis Data Analytics to analyze your Amazon SES email sending events\.

In this tutorial, you first set up an Amazon SES configuration set to publish your email sending events to an Amazon Kinesis Firehose delivery stream, and then you send emails through Amazon SES using that configuration set\. You then set up Amazon Kinesis Data Analytics to capture the email sending events from the Kinesis Firehose stream and use SQL to extract key information from the emails you sent\.

**Note**  
This tutorial requires that you have an application that can send a steady stream of emails through Amazon SES\. This requirement is explained in [Prerequisites](event-publishing-kinesis-analytics-prerequisites.md)\.

The following sections walk you through the tutorial\.
+ [Prerequisites](event-publishing-kinesis-analytics-prerequisites.md)
+ [Step 1: Create a Kinesis Firehose Delivery Stream](event-publishing-kinesis-analytics-firehose-stream.md)
+ [Step 2: Set up a Configuration Set](event-publishing-kinesis-analytics-configuration-set.md)
+ [Step 3: Send Emails](event-publishing-kinesis-analytics-send-email.md)
+ [Step 4: Create an Amazon Kinesis Data Analytics Application](event-publishing-kinesis-analytics-application.md)
+ [Step 5: Run a SQL Query](event-publishing-kinesis-analytics-sql.md)
+ [\(Optional\) Step 6: Save SQL Query Results](event-publishing-kinesis-analytics-destination.md)