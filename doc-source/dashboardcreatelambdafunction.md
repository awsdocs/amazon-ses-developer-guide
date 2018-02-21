# Part 6: Create an AWS Lambda Function<a name="dashboardcreatelambdafunction"></a>

Now that all of the components are in place, you can create a Lambda function\. The function creates an HTML dashboard and notifies you by email when that dashboard is updated\.

**To create a new AWS Lambda function**

1. Download `sesreport.zip` from [https://github\.com/awslabs/aws\-support\-tools/raw/master/SES/SESReports/sesreport\.zip](https://github.com/awslabs/aws-support-tools/raw/master/SES/SESReports/sesreport.zip)\.

1. Open the AWS Lambda console at [https://console\.aws\.amazon\.com/lambda/](https://console.aws.amazon.com/lambda/)\.

1. In the navigation bar, choose **Functions**\.

1. Choose **Create function**\.

1. Choose **Author from scratch**\.

1. Under **Author from scratch** page, complete the following steps:

   + For **Name**, type a name for the function\.

   + For **Runtime**, choose **Node\.js 6\.10**\.

   + For **Role**, select **Choose an existing role**\.

   + For **Existing role**, choose the IAM role you created in [Part 4: Create AWS Identity and Access Management Policies and Roles](dashboardconfigureIAM.md)\.

   Choose **Create function**\.

1. Under **Function code**, for **Code entry type**, choose **Upload a \.ZIP file**\.

1. Choose **Upload**\. Upload `sesreport.zip`\.

1. Under **Environment variables**, add the following keys and values:  
**Keys and Values**    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/dashboardcreatelambdafunction.html)

1. Under **Basic settings**, make the following selections:

   + For **Memory \(MB\)**, choose **512 MB**\.
**Note**  
If you think you will receive more than 5,000 bounces and complaints per day, increase the value in the **Memory** field to at least 1024 MB\. If you think you will receive more than 10,000 bounces and complaints per day, increase the value in the **Memory** field to at least 1536 MB\.

   + For **Timeout**, select **5 min**\.

1. Choose **Test**\.

1. On the **Configure test event** window, for **Event name**, type a name, and then choose **Create**\.

1. In the notification bar at the top of the page, confirm that the function ran successfully\. If it did not, ensure that you entered the correct values for the environment variables in step 9\.

1. Proceed to [Part 7: Test the AWS Lambda Function](dashboardtestlambdafunction.md)\.