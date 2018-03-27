# Part 2: Create a Queue in Amazon Simple Queue Service<a name="dashboardcreateSQSqueue"></a>

In this procedure, you create a new queue in Amazon SQS and subscribe to the topic you created in the previous procedure\. This queue gathers all of the bounce and complaint notifications you receive; an AWS Lambda then collects the notifications in this queue for further processing\.

**To create a new Amazon SQS queue**

1. Open the Amazon SQS console at [https://console\.aws\.amazon\.com/sqs/](https://console.aws.amazon.com/sqs/)\.

1. If you have not created an Amazon SQS queue in the past, choose **Get Started Now**\.

   Otherwise, if your AWS account already contains one or more Amazon SQS queues, choose **Create New Queue**\.

1. Under **What do you want to name your queue?**, for **Queue Name**, type a name for the queue\.

1. Under **What type of queue do you need?**, choose **Standard Queue**\.

1. Choose **Configure Queue**\.

1. For **Default Visibility Timeout**, choose **5 minutes**\.

1. Leave the remaining attributes at their default settings\. Choose **Create Queue**\.

1. In the list of queues, select the queue you just created\. On the **Queue Actions** menu, choose **Subscribe Queue to SNS Topic**\.

1. On the **Subscribe to a Topic** window, make the following selections:
   + For **Topic Region**, choose the AWS Region that contains the Amazon SNS topic you created in [Part 1: Create a Topic in Amazon Simple Notification Service](dashboardcreateSNStopic.md)\.
   + For **Choose a Topic**, choose the topic you created in [Part 1: Create a Topic in Amazon Simple Notification Service](dashboardcreateSNStopic.md)\.

   Choose **Subscribe**\.

1. Proceed to [Part 3: Create an Amazon Simple Storage Service Bucket](dashboardcreateS3bucket.md)\.