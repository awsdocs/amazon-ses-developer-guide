# Creating Reputation Monitoring Alarms Using CloudWatch<a name="reputationdashboard-cloudwatch-alarm"></a>

Amazon SES automatically publishes two reputation\-related metrics to Amazon CloudWatch: `Reputation.BounceRate` and `Reputation.ComplaintRate`\. You can use these metrics to create alarms that notify you when your bounce or complaint rates reach levels that could impact your account's ability to send email\.

**Note**  
The procedures in this section omit some information about optional settings for CloudWatch alarms\. For detailed instructions, see [Creating Amazon CloudWatch Alarms](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html) in the *Amazon CloudWatch User Guide*\.

**To create a CloudWatch alarm**

1. Create a new Amazon SNS topic, and then subscribe to it using your preferred endpoint \(such as email or SMS\)\. For more information, see [Creating a Topic](https://docs.aws.amazon.com/sns/latest/dg/sns-tutorial-create-topic.html) and [Subscribing an Endpoint to a Topic](https://docs.aws.amazon.com/sns/latest/dg/sns-tutorial-create-subscribe-endpoint-to-topic.html) in the *Amazon Simple Notification Service Developer Guide*\.

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Alarms**\.

1. On the **Specify metric and conditions** page, do the following:

   1. Under **Metric**, choose **Select metric**\.

   1. In the list of metrics, choose **SES**\.
**Note**  
If you've never sent an email in the current AWS Region, **SES** might not appear in the list of available metrics\. You can make the **SES** metrics appear by sending a test email to the [Amazon SES mailbox simulator](send-email-simulator.md)\. The metrics appear in CloudWatch within a few minutes\.

   1. Choose **Account Metrics**\.

   1. Choose the metric that you want to create the alarm for\.

      For example, if you want to create an alarm when your bounce rate reaches a certain level, choose **Reputation\.BounceRate**\. If you want to create an alarm when your complaint rate reaches a certain level, choose **Reputation\.ComplaintRate**\.
**Note**  
The **Reputation\.BounceRate** and **Reputation\.ComplaintRate** metrics won't appear on this page if your account has never had a bounce or a complaint, respectively\.

      After you select a metric, choose **Select metric**\.

   1. In the **Conditions** section, under **Threshold type**, choose **Static**\.

   1. Under **Whenever Reputation\.*MetricName* is**, choose **Greater/Equal**\.

   1. Under **than**, specify the value that should cause CloudWatch to raise an alarm\.

      If you're creating an alarm to monitor your bounce rate, note that Amazon SES recommends that you maintain a bounce rate under 5%\. If the bounce rate for your account is greater than 10%, we might pause your account's ability to send email\. For this reason, you should configure CloudWatch to send you a notification when the bounce rate for your account is greater than or equal to 0\.05 \(5%\), as shown in the following image\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/create_cloudwatch_alarm_bounce.png)

      If you're creating an alarm to monitor your complaint rate, note that Amazon SES recommends that you maintain a complaint rate under 0\.1%\. If the complaint rate for your account is greater than 0\.5%, we might pause your account's ability to send email\. For this reason, you should configure CloudWatch to send you a notification when the complaint rate for your account is greater than or equal to 0\.001 \(0\.1%\), as shown in the following image\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/create_cloudwatch_alarm_complaint.png)

   1. Under **Additional configuration**, for **Missing data treatment**, choose **Treat missing data as ignore \(maintain the alarm state\)**\.

   1. Choose **Next**\.

1. On the **Configure actions** page, do the following:

   1. Under **Whenever this alarm state is**, choose **in Alarm**\.

   1. Under **Select an SNS topic**, choose **Select an existing SNS topic**\. For **Send notification to**, choose the topic that you created and subscribed to in step 1\.

   1. Choose **Next**\.

1. On the **Add a description** page, do the following:

   1. For **Alarm name**, enter a unique name for the alarm\.

   1. \(Optional\) For **Alarm description**, enter some text that describes the alarm\.

   1. Choose **Next**\.

1. On the **Preview and create** page, confirm the settings that you specified on the preceding pages\. When you're ready to create the alarm, choose **Create alarm**\.