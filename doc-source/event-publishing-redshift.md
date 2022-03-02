# Analyze email sending events with Amazon Redshift<a name="event-publishing-redshift"></a>

In this tutorial, you publish Amazon SES email sending events to an Amazon Kinesis Data Firehose delivery stream that publishes data to Amazon Redshift\. You then connect to the Amazon Redshift database and use a SQL query tool to query the database for Amazon SES email sending events that meet certain criteria\.

The following sections walk you through the process\.
+ [Prerequisites](event-publishing-redshift-prerequisites.md)
+ [Step 1: Create an Amazon Redshift Cluster](event-publishing-redshift-cluster.md)
+ [Step 2: Connect to Your Amazon Redshift Cluster](event-publishing-redshift-cluster-connect.md)
+ [Step 3: Create a Database Table](event-publishing-redshift-table.md)
+ [Step 4: Create a Kinesis Data Firehose Delivery Stream](event-publishing-redshift-firehose-stream.md)
+ [Step 5: Set up a Configuration Set](event-publishing-redshift-configuration-set.md)
+ [Step 6: Send Emails](event-publishing-redshift-send-email.md)
+ [Step 7: Query Email Sending Events](event-publishing-redshift-query.md)