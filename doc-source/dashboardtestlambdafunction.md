# Part 7: Test the AWS Lambda Function<a name="dashboardtestlambdafunction"></a>

Before you schedule the Lambda function to run on a regular basis, you should test it to be sure that all of the components are configured properly\. In this procedure, you send two messages through the Amazon SES mailbox simulator, and then run the Lambda function to ensure that messages are being processed properly\.

**To test the Lambda function**

1. Open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the column on the left, choose either **Email Addresses** or **Domains**\.

1. Select a verified email address or domain, and then choose **Send a Test Email**\.

1. For **To:**, type **bounce@simulator\.amazonses\.com**\. For **Subject** and **Body**, type some sample text\. Choose **Send Test Email**\.

1. Repeat steps 3 and 4 to create another test message, but this time, for **To:**, type **complaint@simulator\.amazonses\.com**\.

1. Open the Amazon SQS console at [https://console\.aws\.amazon\.com/sqs/](https://console.aws.amazon.com/sqs/)\. The **Messages Available** column should indicate that 2 messages are available\.

1. Open the Lambda console at [https://console\.aws\.amazon\.com/lambda/](https://console.aws.amazon.com/lambda/)\.

1. In the navigation bar, choose **Functions**\.

1. In the list of functions, choose the function you created in [Part 6: Create an AWS Lambda Function](dashboardcreatelambdafunction.md)\.

1. Choose **Test**\. When the function finishes running, expand the **Details** section\. If the Lambda function was configured properly, you will receive one of the following messages: 
   + `null`: Indicates that the function ran without errors\.
   + `Queue empty`: Indicates that there were no new bounce or complaint notifications in the queue\.

1. Proceed to [Part 8: Configure Triggers in Amazon CloudWatch](dashboardtriggercloudwatch.md)\.