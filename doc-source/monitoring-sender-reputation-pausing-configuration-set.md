# Automatically Pausing Email Sending for a Configuration Set<a name="monitoring-sender-reputation-pausing-configuration-set"></a>

You can configure Amazon SES to export reputation metrics that are specific to emails that are sent using a specific configuration set to Amazon CloudWatch\. You can then use these metrics to create CloudWatch alarms that are specific to these configuration sets\. When these alarms exceed certain thresholds, you can automatically pause the sending of emails that use the specified configuration sets, without impacting the overall email sending capabilities of your Amazon SES account\.

**Note**  
The solution described in this section pauses email sending for a specific configuration set in a single AWS Region\. If you send email from multiple regions, repeat the procedures in this section for each region in which you want to implement this solution\.


+ [Part 1: Enable Reputation Metric Reporting for the Configuration Set](#monitoring-sender-reputation-pausing-configuration-set-part-1)
+ [Part 2: Create an IAM Role](#monitoring-sender-reputation-pausing-configuration-set-part-2)
+ [Part 3: Create the Lambda Function](#monitoring-sender-reputation-pausing-configuration-set-part-3)
+ [Part 4: Re\-Enable Email Sending for the Configuration Set](#monitoring-sender-reputation-pausing-configuration-set-part-4)
+ [Part 5: Create an Amazon SNS Topic](#monitoring-sender-reputation-pausing-configuration-set-part-5)
+ [Part 6: Create a CloudWatch Alarm](#monitoring-sender-reputation-pausing-configuration-set-part-6)
+ [Part 7: Test the solution](#monitoring-sender-reputation-pausing-configuration-set-part-7)

## Part 1: Enable Reputation Metric Reporting for the Configuration Set<a name="monitoring-sender-reputation-pausing-configuration-set-part-1"></a>

Before you can configure Amazon SES to automatically pause email sending for a configuration set, you must first enable the export of reputation metrics for the configuration set\.

To enable the export of bounce and complaint metrics for the configuration set, complete the steps in [[ERROR] BAD/MISSING LINK TEXT](configuration-sets-export-metrics.md)\.

## Part 2: Create an IAM Role<a name="monitoring-sender-reputation-pausing-configuration-set-part-2"></a>

The first step in configuring automatic pausing of email sending is to create an IAM role that can execute the `UpdateConfigurationSetSendingEnabled` API operation\.

**To create the IAM role**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**\.

1. Choose **Create role**\.

1. Under **Select type of trusted entity**, choose **AWS service**\.

1. Under **Choose the service that will use this role**, choose **Lambda**\. Choose **Next: Permissions**\.

1. On the **Attach permissions policies** page, choose the following policies:

   + **AWS LambdaBasicExecutionRole**

   + **AmazonSESFullAccess**
**Tip**  
Use the search box at the top of the list of policies to quickly locate these policies\.

   Choose **Next: Review**\.

1. On the **Review** page, for **Name**, type a name for the role\. Choose **Create role**\.

## Part 3: Create the Lambda Function<a name="monitoring-sender-reputation-pausing-configuration-set-part-3"></a>

After you create an IAM role, you can create the Lambda function that pauses email sending for the configuration set\.

**To create the Lambda function**

1. Open the AWS Lambda console at [https://console\.aws\.amazon\.com/lambda/](https://console.aws.amazon.com/lambda/)\.

1. Use the region selector to choose the region in which you want to deploy this Lambda function\.
**Note**  
This function only pauses email sending for configuration sets in the AWS Region you select in this step\. If you send email from more than one region, repeat the procedures in this section for each region in which you want to automatically pause email sending\.

1. Choose **Create function**\.

1. Under **Create function**, choose **Author from scratch**\.

1. Under **Author from scratch**, complete the following steps:

   + For **Name**, type a name for the Lambda function\.

   + For **Runtime**, choose **Node\.js 6\.10**\.

   + For **Role**, choose **Choose an existing role**\.

   + For **Existing role**, choose the IAM role you created in [[ERROR] BAD/MISSING LINK TEXT](#monitoring-sender-reputation-pausing-configuration-set-part-2)\.

   Choose **Create function**\.

1. Under **Function code**, in the code editor, paste the following code:

   ```
   'use strict';
   
   var aws = require('aws-sdk');
   
   // Create a new SES object. 
   var ses = new aws.SES();
   
   // Specify the parameters for this operation. In this example, you pass the 
   // Enabled parameter, with a value of false (Enabled = false disables email 
   // sending, Enabled = true enables it). You also pass the ConfigurationSetName
   // parameter, with a value equal to the name of the configuration set for 
   // which you want to pause email sending.
   var params = {
       ConfigurationSetName: ConfigSet,
       Enabled: false
   };
   
   exports.handler = (event, context, callback) => {
       // Pause sending for a configuration set
       ses.updateConfigurationSetSendingEnabled(params, function(err, data) {
           if(err) {
               console.log(err.message);
           } else {
               console.log(data);
           }
       });
   };
   ```

   Replace *ConfigSet* in the preceding code with the name of the configuration set\. Choose **Save**\.

1. Choose **Test**\. If the **Configure test event** window appears, type a name in the **Event name** field, and then choose **Create**\.

1.  Ensure that the notification bar at the top of the page says Execution result: succeeded\. If the function failed to execute, do the following:

   + Verify that the IAM role you created in [[ERROR] BAD/MISSING LINK TEXT](#monitoring-sender-reputation-pausing-configuration-set-part-2) contains the correct policies\.

   + Verify that the code in the Lambda function does not contain any errors\. The Lambda code editor automatically highlights syntax errors and other potential issues\.

## Part 4: Re\-Enable Email Sending for the Configuration Set<a name="monitoring-sender-reputation-pausing-configuration-set-part-4"></a>

A side effect of testing the Lambda function in [[ERROR] BAD/MISSING LINK TEXT](#monitoring-sender-reputation-pausing-configuration-set-part-3) is that email sending for the configuration set is paused\. In most cases, you do not want to pause sending for the configuration set until the CloudWatch alarm is triggered\.

The procedures in this section re\-enable email sending for your configuration set\. To complete these procedures, you must install and configure the AWS Command Line Interface\. For more information, see the [AWS Command Line Interface User Guide](http://docs.aws.amazon.com/cli/latest/userguide/)\.

**To re\-enable email sending**

1. At the command line, type the following command to re\-enable email sending for the configuration set: aws ses update\-configuration\-set\-sending\-enabled \-\-configuration\-set\-name *ConfigSet* \-\-enabled \-\-region *us\-west\-2*

   In the preceding command, replace *ConfigSet* with the name of the configuration set for which you want to pause email sending, and replace *us\-west\-2* with the region in which you want to automatically pause email sending\.

1. At the command line, type the following command to ensure that email sending is enabled: aws ses describe\-configuration\-set \-\-configuration\-set\-name *ConfigSet* \-\-region *us\-west\-2*

   You will see output similar to the following:

   ```
   {                           
       "ConfigurationSet": {   
           "Name": "ConfigSet" 
       },
       "ReputationOptions": {
           "ReputationMetricsEnabled": true,
           "SendingEnabled": true
       }	
   }
   ```

   If the value of `SendingEnabled` is `true`, then email sending for the configuration set was successfully re\-enabled\.

## Part 5: Create an Amazon SNS Topic<a name="monitoring-sender-reputation-pausing-configuration-set-part-5"></a>

For CloudWatch to execute the Lambda function when an alarm is triggered, you must first create an Amazon SNS topic and subscribe the Lambda function to it\.

**To create the Amazon SNS topic**

1. Open the Amazon SNS console at [https://console\.aws\.amazon\.com/sns/v2/home](https://console.aws.amazon.com/sns/v2/home)\.

1. Use the region selector to choose the region in which you want to automatically pause email sending\.

1. In the navigation pane, choose **Topics**\.

1. Choose **Create new topic**\.

1. On the **Create new topic** window, for **Topic name**, type a name for the topic\. Optionally, you can type a more descriptive name in the **Display name** field\.

   Choose **Create topic**\.

1. In the list of topics, check the box next to the topic you created in the previous step\. On the **Actions** menu, choose **Subscribe to topic**\.

1. On the **Create subscription** window, make the following selections:

   + For **Protocol**, choose **AWS Lambda**\.

   + For **Endpoint**, choose the Lambda function you created in [[ERROR] BAD/MISSING LINK TEXT](#monitoring-sender-reputation-pausing-configuration-set-part-3)\.

   + For **Version or alias**, choose **default**\.

1. Choose **Create subscription**\.

## Part 6: Create a CloudWatch Alarm<a name="monitoring-sender-reputation-pausing-configuration-set-part-6"></a>

This section contains procedures for creating an alarm in CloudWatch that is triggered when a metric reaches a certain threshold\. When the alarm is triggered, it delivers a notification to the Amazon SNS topic you created in [[ERROR] BAD/MISSING LINK TEXT](#monitoring-sender-reputation-pausing-configuration-set-part-5), which then executes the Lambda function you created in [[ERROR] BAD/MISSING LINK TEXT](#monitoring-sender-reputation-pausing-configuration-set-part-3)\.

**To create a CloudWatch alarm**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. Use the region selector to choose the region in which you want to automatically pause email sending\.

1. In the navigation pane on the left, choose **Alarms**\.

1. Choose **Create Alarm**\.

1. On the **Create Alarm** window, under **SES Metrics**, choose **Configuration Set Metrics**\.

1. In the **ses:configuration\-set** column, locate the configuration set for which you want to create an alarm\. Under **Metric Name**, choose one of the following options:

   + **Reputation\.BounceRate** – Choose this metric if you want to pause email sending for the configuration set when the overall hard bounce rate for the configuration set crosses a threshold that you define\.

   + **Reputation\.ComplaintRate** – Choose this metric if you want to pause email sending for the configuration set when the overall complaint rate for the configuration set crosses a threshold that you define\.

   Choose **Next**\.

1. Complete the following steps:

   + Under **Alarm Threshold**, for **Name**, type a name for the alarm\.

   + Under **Whenever: Reputation\.BounceRate** or **Whenever: Reputation\.ComplaintRate**, specify the threshold that causes the alarm to trigger\.
**Note**  
If the overall bounce rate for your Amazon SES account exceeds 10%, or if the overall complaint rate for your Amazon SES account exceeds \.5%, your Amazon SES account is automatically placed on probation\. When you specify the bounce or complaint rate that causes the CloudWatch alarm to trigger, we recommend that you use values that are far below these rates to prevent your account from being placed on probation\.

   + Under **Actions**, for **Whenever this alarm**, choose **State is ALARM**\. For **Send notification to**, choose the Amazon SNS topic you created in [[ERROR] BAD/MISSING LINK TEXT](#monitoring-sender-reputation-pausing-configuration-set-part-5)\.

   Choose **Create Alarm**\.

## Part 7: Test the solution<a name="monitoring-sender-reputation-pausing-configuration-set-part-7"></a>

You can now test the alarm to ensure that it executes the Lambda function when it enters the `ALARM` state\. You can use the `SetAlarmState` operation in the CloudWatch API to temporarily change the state of the alarm\.

The procedures in this section are optional, but we recommend that you complete them to verify that the entire solution is configured correctly\.

**To test the solution**

1. At the command line, type the following command to check the email sending status for the configuration set: aws ses describe\-configuration\-set \-\-configuration\-set\-name *ConfigSet* \-\-region *us\-west\-2*

   If sending is enabled for the configuration set, you see the following output:

   ```
   {                           
       "ConfigurationSet": {   
           "Name": "ConfigSet" 
       },
       "ReputationOptions": {
           "ReputationMetricsEnabled": true,
           "SendingEnabled": true
       }	
   }
   ```

   If the value of `SendingEnabled` is `true`, then email sending is currently enabled for the configuration set\.

1. At the command line, type the following command to temporarily change the alarm state to `ALARM`: aws cloudwatch set\-alarm\-state \-\-alarm\-name *MyAlarm* \-\-state\-value ALARM \-\-state\-reason "Testing execution of Lambda function" \-\-region *us\-west\-2*

   Replace *MyAlarm* in the preceding command with the name of the alarm you created in [[ERROR] BAD/MISSING LINK TEXT](#monitoring-sender-reputation-pausing-configuration-set-part-6)\.
**Note**  
When you execute this command, the status of the alarm switches from `OK` to `ALARM` and back to `OK` within a few seconds\. You can view these status changes on the alarm's **History** tab in the CloudWatch console, or by using the [DescribeAlarmHistory](http://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_DescribeAlarmHistory.html) operation\.

1. At the command line, type the following command to check the email sending status for the configuration set: aws ses describe\-configuration\-set \-\-configuration\-set\-name *ConfigSet*

   If the Lambda function executed successfully, you see the following output:

   ```
   {                           
       "ConfigurationSet": {   
           "Name": "ConfigSet" 
       },
       "ReputationOptions": {
           "ReputationMetricsEnabled": true,
           "SendingEnabled": false
       }	
   }
   ```

   If the value of `SendingEnabled` is `false`, then email sending for the configuration set is disabled, indicating that the Lambda function executed successfully\.

1. Complete the steps in [[ERROR] BAD/MISSING LINK TEXT](#monitoring-sender-reputation-pausing-configuration-set-part-4) to re\-enable email sending for the configuration set\.