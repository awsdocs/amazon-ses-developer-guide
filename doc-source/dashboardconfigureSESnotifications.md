# Part 5: Configure Bounce and Complaint Notifications in Amazon SES<a name="dashboardconfigureSESnotifications"></a>

To receive bounce and complaint notifications from Amazon SES, you must configure notifications in the Amazon SES console\. This procedure describes how to set up these notifications\.

1. Open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation bar, under **Identity Management**, choose **Email Addresses**\.

1. Select the email address that you want to use to receive bounce and complaint notifications, and then choose **View Details**\.

1. Under **Notifications**, choose **Edit Configuration**\.

1. For both **Bounces** and **Complaints**, choose the Amazon SNS topic you created in [Part 1: Create a Topic in Amazon Simple Notification Service](dashboardcreateSNStopic.md)\. Choose **Save Config**\.

1. Repeat steps 3–5 for each email address that should receive bounce or complaint notifications\.

1. Open the Amazon SQS console at [https://console\.aws\.amazon\.com/sqs/](https://console.aws.amazon.com/sqs/)\.

1. Right\-click the queue you created in [Part 2: Create a Queue in Amazon Simple Queue Service](dashboardcreateSQSqueue.md), and then choose **Purge Queue**\.
**Note**  
This step is required\. When you configure notifications, Amazon SES sends a confirmation notification that is picked up by Amazon SNS\. These confirmations cannot be parsed by the Lambda function, and may cause the Lambda function to fail\.

1. Proceed to [Part 6: Create an AWS Lambda Function](dashboardcreatelambdafunction.md)\.