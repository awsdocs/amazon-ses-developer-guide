# Visualize Email Sending Events With Amazon Elasticsearch Service and Kibana<a name="event-publishing-elasticsearch"></a>

Elasticsearch is an open\-source search and analytics engine for use cases such as log analytics and real\-time application monitoring\. Amazon Elasticsearch Service \(Amazon ES\) is an AWS service that enables you to deploy, operate, and scale Elasticsearch in the AWS cloud\. You can use Amazon ES to analyze your Amazon SES email sending events\. 

In this tutorial, you publish Amazon SES email sending events to an Amazon Kinesis Data Firehose delivery stream that publishes the event data to Amazon ES\. You then view the data with [Kibana](https://www.elastic.co/products/kibana), an open\-source visualization tool designed to work with Elasticsearch\. Amazon ES includes built\-in integration with Kibana\. 

The following sections walk you through the process\.
+ [Prerequisites](event-publishing-elasticsearch-prerequisites.md)
+ [Step 1: Create an Amazon ES Cluster](event-publishing-elasticsearch-cluster.md)
+ [Step 2: Create a Kinesis Data Firehose Delivery Stream](event-publishing-elasticsearch-firehose-stream.md)
+ [Step 3: Set up a Configuration Set](event-publishing-elasticsearch-configuration-set.md)
+ [Step 4: Send Emails](event-publishing-elasticsearch-send-email.md)
+ [Step 5: Visualize Data in Kibana](event-publishing-elasticsearch-kibana.md)