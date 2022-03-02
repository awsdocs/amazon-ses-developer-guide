# Retrieving Amazon SES event data from CloudWatch<a name="event-publishing-retrieving-cloudwatch"></a>

Amazon SES can publish metrics for your email sending events to Amazon CloudWatch\. When you publish event data to CloudWatch, it provides these metrics as an ordered set of time\-series data\. You can use these metrics to monitor the performance of your email sending\. For example, you can monitor the complaint metric and set a CloudWatch alarm to trigger when the metric exceeds a certain value\.

There are two levels of granularity at which Amazon SES can publish these events to CloudWatch:
+ **Across your AWS account** – These coarse metrics, which correspond to the metrics you monitor using the Amazon SES console and the `GetSendStatistics` API, are totals across your entire AWS account\. Amazon SES publishes these metrics to CloudWatch automatically\.
+ **Fine\-grained** – These metrics are categorized by email characteristics that you define using *message tags*\. To publish these metrics to CloudWatch, you have to [set up event publishing](monitor-sending-using-event-publishing-setup.md) with a CloudWatch event destination and [specify a configuration set](event-publishing-send-email.md) when you send an email\. You can also specify message tags or use [auto\-tags](monitor-using-event-publishing.md#event-publishing-how-works) that Amazon SES automatically provides\.

This section describes the available metrics and how to view the metrics in CloudWatch\.

## Available Metrics<a name="event-publishing-retrieving-cloudwatch-metrics"></a>

You can publish following Amazon SES email sending metrics to CloudWatch:
+ **Sends** – The send request was successful and Amazon SES will attempt to deliver the message to the recipient’s mail server\. \(If account\-level or global suppression is being used, SES will still count it as a send, but delivery is suppressed\.\)
+ **Rendering Failures** – The email wasn't sent because of a template rendering issue\. This event type can occur when template data is missing, or when there is a mismatch between template parameters and data\. \(This event type only occurs when you send email using the [https://docs.aws.amazon.com/ses/latest/APIReference/API_SendTemplatedEmail.html](https://docs.aws.amazon.com/ses/latest/APIReference/API_SendTemplatedEmail.html) or [https://docs.aws.amazon.com/ses/latest/APIReference/API_SendBulkTemplatedEmail.html](https://docs.aws.amazon.com/ses/latest/APIReference/API_SendBulkTemplatedEmail.html) API operations\.\)
+ **Rejects** – Amazon SES accepted the email, but determined that it contained a virus and didn’t attempt to deliver it to the recipient’s mail server\.
+ **Deliveries** – Amazon SES successfully delivered the email to the recipient's mail server\.
+ **Hard bounces** – The recipient's mail server permanently rejected the email\. \(*Soft bounces* are only included when Amazon SES fails to deliver the email after retrying for a period of time\.\)
+ **Complaints** – The email was successfully delivered to the recipient’s mail server, but the recipient marked it as spam\.
+ **Delivery Delays** – The email couldn't be delivered to the recipient’s mail server because a temporary issue occurred\. Delivery delays can occur, for example, when the recipient's inbox is full, or when the receiving email server experiences a transient issue\.
+ **Subscriptions** – The email was successfully delivered, but the recipient updated the subscription preferences by clicking `List-Unsubscribe` in the email header or the `Unsubscribe` link in the footer\.
+ **Opens** – The recipient received the message and opened it in their email client\.
+ **Clicks** – The recipient clicked one or more links in the email\.

## Available Dimensions<a name="event-publishing-retrieving-cloudwatch-dimensions"></a>

CloudWatch uses the dimension names that you specify when you add a CloudWatch event destination to a configuration set in Amazon SES\. For more information, see [Set up a CloudWatch event destination for event publishing](event-publishing-add-event-destination-cloudwatch.md)\.

## Viewing Amazon SES Metrics in the CloudWatch Console<a name="event-publishing-retrieving-cloudwatch-console"></a>

The following procedure describes how to view your Amazon SES event publishing metrics using the CloudWatch console\.

**To view metrics using the CloudWatch console**

1. Sign in to the AWS Management Console and open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. If necessary, change the region\. From the navigation bar, select the region where your AWS resources reside\. For more information, see [Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html)\.

1. In the navigation pane, choose **Metrics**\.

1. In the **All metrics** pane, expand **AWS Namespaces**, and then choose **SES**\.

1. To view metrics across your entire AWS account, which Amazon SES publishes automatically, choose **Account Sending Metrics**\. To view fine\-grained [event publishing metrics](monitor-using-event-publishing.md), choose the combination of dimensions that you specified when you [set up your CloudWatch event destination](event-publishing-add-event-destination-cloudwatch.md)\.

1. Choose the metric you want to view\.

   The graph will display the metric over time\.

**To view metrics using the AWS CLI**
+ At a command prompt, use the following command:

  ```
  1. aws cloudwatch list-metrics --namespace "AWS/SES"
  ```