# Automatically pausing email sending for your entire Amazon SES account<a name="monitoring-sender-reputation-pausing-account"></a>

The procedures in this section explain the steps to set up Amazon SES, Amazon SNS, Amazon CloudWatch, and AWS Lambda to automatically pause email sending for your Amazon SES account in a single AWS Region\. If you send email from multiple regions, repeat the procedures in this section for each region in which you want to implement this solution\.

**Topics**
+ [Part 1: Create an IAM Role](#monitoring-sender-reputation-pausing-account-part-1)
+ [Part 2: Create the Lambda Function](#monitoring-sender-reputation-pausing-account-part-2)
+ [Part 3: Re\-Enable Email Sending for Your Account](#monitoring-sender-reputation-pausing-account-part-3)
+ [Part 4: Create an Amazon SNS Topic](#monitoring-sender-reputation-pausing-account-part-4)
+ [Part 5: Create a CloudWatch Alarm](#monitoring-sender-reputation-pausing-account-part-5)
+ [Part 6: Test the solution](#monitoring-sender-reputation-pausing-account-part-6)

## Part 1: Create an IAM Role<a name="monitoring-sender-reputation-pausing-account-part-1"></a>

The first step in configuring automatic pausing of email sending is to create an IAM role that can execute the `UpdateAccountSendingEnabled` API operation\.

**To create the IAM role**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**\.

1. Choose **Create role**\.

1. On the **Select trusted entity** page, choose **AWS service** for the **Trusted entity type**\.

1. Under **Use case**, choose **Lambda**, then choose **Next**\.

1. On the **Add permissions** page, choose the following policies:
   + **AWSLambdaBasicExecutionRole**
   + **AmazonSESFullAccess**
**Tip**  
Use the search box under **Permission policies** to quickly locate these policies, but note that after searching for and selecting the first policy, you must choose **Clear filters** before searching and selecting the second policy\.

   Then choose **Next**\.

1. On the **Name, review, and create** page, under **Role details**, enter a meaningful name for the policy in the **Role name** field\.

1. Verify that the two policies you selected are listed in the **Permissions policy summary** table, then choose **Create role**\.

## Part 2: Create the Lambda Function<a name="monitoring-sender-reputation-pausing-account-part-2"></a>

After you create an IAM role, you can create the Lambda function that pauses email sending for your account\.

**To create the Lambda function**

1. Open the AWS Lambda console at [https://console\.aws\.amazon\.com/lambda/](https://console.aws.amazon.com/lambda/)\.

1. Use the region selector to choose the region in which you want to deploy this Lambda function\.
**Note**  
This function only pauses email sending in the AWS Region you select in this step\. If you send email from more than one region, repeat the procedures in this section for each region in which you want to automatically pause email sending\.

1. Choose **Create function**\.

1. Under **Create function**, choose **Author from scratch**\.

1. Under **Basic information**, complete the following steps:
   + For **Function name**, type a name for the Lambda function\.
   + For **Runtime**, choose **Node\.js 14x** \(or the version currently offered in the select list\)\.
   + For **Architecture**, keep the preselected default, **x86\_64**\.
   + Under Permissions, expand **Change default execution role** and choose **Use an existing role**\.
   + Click inside the **Existing role** list box, and choose the IAM role you created in [Part 1: Create an IAM Role](#monitoring-sender-reputation-pausing-account-part-1)\.

   Then choose **Create function**\.

1. Under **Code source**, in the code editor, paste the following code:

   ```
   'use strict';
   
   var aws = require('aws-sdk');
   
   // Create a new SES object. 
   var ses = new aws.SES();
   
   // Specify the parameters for this operation. In this case, there is only one
   // parameter to pass: the Enabled parameter, with a value of false
   // (Enabled = false disables email sending, Enabled = true enables it).
   var params = {
       Enabled: false
   };
   
   exports.handler = (event, context, callback) => {
       // Pause sending for your entire SES account
       ses.updateAccountSendingEnabled(params, function(err, data) {
           if(err) {
               console.log(err.message);
           } else {
               console.log(data);
           }
       });
   };
   ```

   Then choose **Deploy**\.

1. Choose **Test**\. If the **Configure test event** window appears, type a name in the **Event name** field, and then choose **Save**\.

1. Expand the **Test** drop box and select the name of the event you just created, and then choose **Test**\.

1. The **Execution results** tab will appear \- just below it and to the right, ensure that Status: Succeeded is displayed\. If the function failed to execute, do the following:
   + Verify that the IAM role you created in [Part 1: Create an IAM Role](#monitoring-sender-reputation-pausing-account-part-1) contains the correct policies\.
   + Verify that the code in the Lambda function does not contain any errors\. The Lambda code editor automatically highlights syntax errors and other potential issues\.

## Part 3: Re\-Enable Email Sending for Your Account<a name="monitoring-sender-reputation-pausing-account-part-3"></a>

A side effect of testing the Lambda function in [Part 2: Create the Lambda Function](#monitoring-sender-reputation-pausing-account-part-2) is that email sending for your Amazon SES account is paused\. In most cases, you do not want to pause sending for your account until the CloudWatch alarm is triggered\.

The procedures in this section re\-enable email sending for your Amazon SES account\. To complete these procedures, you must install and configure the AWS Command Line Interface\. For more information, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\.

**To re\-enable email sending**

1. At the command line, type the following command to re\-enable email sending for your account\. Replace *sending\_region* with the name of the Region in which you want to re\-enable email sending\.

   ```
   aws ses update-account-sending-enabled --enabled --region sending_region
   ```

1. At the command line, type the following command to check the email sending status for your account:

   ```
   aws ses get-account-sending-enabled --region sending_region
   ```

   If you see the following output, then you have successfully re\-enabled email sending for your account:

   ```
   {
       "Enabled": true 
   }
   ```

## Part 4: Create an Amazon SNS Topic<a name="monitoring-sender-reputation-pausing-account-part-4"></a>

For CloudWatch to execute your Lambda function when an alarm is triggered, you must first create an Amazon SNS topic and subscribe the Lambda function to it\.

**To create the Amazon SNS topic**

1. Open the Amazon SNS console at [https://console\.aws\.amazon\.com/sns/v3/home](https://console.aws.amazon.com/sns/v3/home)\.

1. Use the region selector to choose the region in which you want to automatically pause email sending\.

1. In the navigation pane, choose **Topics**\.

1. Choose **Create new topic**\.

1. On the **Create new topic** window, for **Topic name**, type a name for the topic\. Optionally, you can type a more descriptive name in the **Display name** field\.

   Choose **Create topic**\.

1. In the list of topics, check the box next to the topic you created in the previous step\. On the **Actions** menu, choose **Subscribe to topic**\.

1. On the **Create subscription** window, make the following selections:
   + For **Protocol**, choose **AWS Lambda**\.
   + For **Endpoint**, choose the Lambda function you created in [Part 2: Create the Lambda Function](#monitoring-sender-reputation-pausing-account-part-2)\.
   + For **Version or alias**, choose **default**\.

1. Choose **Create subscription**\.

## Part 5: Create a CloudWatch Alarm<a name="monitoring-sender-reputation-pausing-account-part-5"></a>

This section contains procedures for creating an alarm in CloudWatch that is triggered when a metric reaches a certain threshold\. When the alarm is triggered, it delivers a notification to the Amazon SNS topic you created in [Part 4: Create an Amazon SNS Topic](#monitoring-sender-reputation-pausing-account-part-4), which then executes the Lambda function you created in [Part 2: Create the Lambda Function](#monitoring-sender-reputation-pausing-account-part-2)\.

**To create a CloudWatch alarm**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. Use the region selector to choose the region in which you want to automatically pause email sending\.

1. In the navigation pane, choose **Alarms**\.

1. Choose **Create Alarm**\.

1. On the **Create Alarm** window, under **SES Metrics**, choose **Account Metrics**\.

1. Under **Metric Name**, choose one of the following options:
   + **Reputation\.BounceRate** – Choose this metric if you want to pause email sending for your account when the overall hard bounce rate for your account crosses a threshold that you define\.
   + **Reputation\.ComplaintRate** – Choose this metric if you want to pause email sending for your account when the overall complaint rate for your account crosses a threshold that you define\.

   Choose **Next**\.

1. Complete the following steps:
   + Under **Alarm Threshold**, for **Name**, type a name for the alarm\.
   + Under **Whenever: Reputation\.BounceRate** or **Whenever: Reputation\.ComplaintRate**, specify the threshold that causes the alarm to trigger\.
**Note**  
Your account is automatically placed under review if your bounce rate exceeds 10%, or if your complaint rate exceeds \.5%\. When you specify the bounce or complaint rate that causes the CloudWatch alarm to trigger, we recommend that you use values that are below these rates to prevent your account from being placed under review\.
   + Under **Actions**, for **Whenever this alarm**, choose **State is ALARM**\. For **Send notification to**, choose the Amazon SNS topic you created in [Part 4: Create an Amazon SNS Topic](#monitoring-sender-reputation-pausing-account-part-4)\.

   Choose **Create Alarm**\.

## Part 6: Test the solution<a name="monitoring-sender-reputation-pausing-account-part-6"></a>

You can now test the alarm to ensure that it executes the Lambda function when it enters the `ALARM` state\. You can use the `SetAlarmState` API operation to temporarily change the state of the alarm\.

The procedures in this section are optional, but we recommend that you complete them to ensure that the entire solution is configured correctly\.

1. At the command line, type the following command to check the email sending status for your account\. Replace *region* with the name of the Region\.

   ```
   aws ses get-account-sending-enabled --region region
   ```

   If sending is enabled for your account, you see the following output:

   ```
   {
       "Enabled": true 
   }
   ```

1. At the command line, type the following command to temporarily change the alarm state to `ALARM`: aws cloudwatch set\-alarm\-state \-\-alarm\-name *MyAlarm* \-\-state\-value ALARM \-\-state\-reason "Testing execution of Lambda function" \-\-region *region*

   Replace *MyAlarm* in the preceding command with the name of the alarm you created in [Part 5: Create a CloudWatch Alarm](#monitoring-sender-reputation-pausing-account-part-5), and replace *region* with the Region in which you want to automatically pause email sending\.
**Note**  
When you execute this command, the status of the alarm switches from `OK` to `ALARM` and back to `OK` within a few seconds\. You can view these status changes on the alarm's **History** tab in the CloudWatch console, or by using the [DescribeAlarmHistory](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_DescribeAlarmHistory.html) operation\.

1. At the command line, type the following command to check the email sending status for your account\.

   ```
   aws ses get-account-sending-enabled --region region
   ```

   If the Lambda function executed successfully, you see the following output:

   ```
   {
       "Enabled": false
   }
   ```

1. Complete the steps in [Part 3: Re\-Enable Email Sending for Your Account](#monitoring-sender-reputation-pausing-account-part-3) to re\-enable email sending for your account\.