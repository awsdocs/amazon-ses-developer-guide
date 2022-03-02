# Managing Amazon SES event destinations<a name="event-destinations-manage"></a>

Event destinations allow you to publish the following outgoing email tracking actions to other AWS services for monitoring:
+ Sends
+ Rendering failures
+ Rejects
+ Deliveries
+ Hard bounces
+ Complaints
+ Delivery delays
+ Subscriptions
+ Opens
+ Clicks

To learn more about setting up event publishing, see [Monitor email sending using Amazon SES event publishing](monitor-using-event-publishing.md)\.<a name="proc-access-event-destinations"></a>

**Access event destinations for an existing configuration set \(console\)**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane, under **Configuration**, choose **Configuration sets**\.

1. To see details for a configuration set, choose the **Name** from the configuration set list\. This takes you to the details page\.

1. To work with event destinations, choose the **Event destinations** tab\. This takes you to a list of event destinations you have defined for the configuration set\.

## Add an event destination \(console\)<a name="event-destination-add"></a>

Follow these steps to add an event destination to a configuration set:

1. [Access event destinations for an existing configuration set \(console\)](#proc-access-event-destinations)\.

1. Choose **Add destination**\.

1. 

**Select event types**

   Email sending events are metrics relating to your sending activity that you can measure using Amazon SES\. In this step, you select which types of email sending events you would like Amazon SES to publish to your event destination\.

   To learn more about event types, see [Monitoring your Amazon SES sending activity](monitor-sending-activity.md)\.

   1. Choose **Event types** to publish
      + **Sending and delivery** – to choose event types to publish, select their respective check boxes, or choose **Select all** to publish all of the event types\.

**Event types**
        + **Sends** – the send request was successful and Amazon SES will attempt to deliver the message to the recipient’s mail server\.
        + **Rendering failures** – the email wasn't sent because of a template rendering issue\. This event type can occur when template data is missing, or when there is a mismatch between template parameters and data\. \(This event type only occurs when you send email using the [https://docs.aws.amazon.com/ses/latest/APIReference/API_SendTemplatedEmail.html](https://docs.aws.amazon.com/ses/latest/APIReference/API_SendTemplatedEmail.html) or [https://docs.aws.amazon.com/ses/latest/APIReference/API_SendBulkTemplatedEmail.html](https://docs.aws.amazon.com/ses/latest/APIReference/API_SendBulkTemplatedEmail.html) API operations\.\)
        + **Rejects** – Amazon SES accepted the email, but determined that it contained a virus and didn’t attempt to deliver it to the recipient’s mail server\.
        + **Deliveries** – Amazon SES successfully delivered the email to the recipient's mail server\.
        + **Hard bounces** – the recipient's mail server permanently rejected the email\. \(*Soft bounces* are only included when Amazon SES fails to deliver the email after retrying for a period of time\.\)
        + **Complaints** – the email was successfully delivered to the recipient’s mail server, but the recipient marked it as spam\.
        + **Delivery delays** – the email couldn't be delivered to the recipient’s mail server because a temporary issue occurred\. Delivery delays can occur, for example, when the recipient's inbox is full, or when the receiving email server experiences a transient issue\.
        + **Subscriptions** – the email was successfully delivered, but the recipient updated the subscription preferences by clicking `List-Unsubscribe` in the email header or the `Unsubscribe` link in the footer\.
      + **Open and click tracking** – to measure subscriber engagement, choose one or both of the check boxes to track **Opens** and **Clicks**\.
        + **Opens** – the recipient received the message and opened it in their email client\.
        + **Clicks** – the recipient clicked one or more links in the email\.
        + **Configuration set redirect domain** – this field will appear and be prepopulated with the name of the custom redirect domain if you assigned one when creating the configuration set\.
**Note**  
You can update the **Custom redirect domain** in the configuration set for open and click tracking under that domain\. For more information about configuring custom open and click domains see [Configuring custom domains to handle open and click tracking](configure-custom-open-click-domains.md)\.

   1. Choose **Next** to continue\.

1. <a name="add-event-destination-step-4"></a>

**Specify destination**

   An event destination is an AWS service to which email sending events can be published\. Choosing the appropriate destination depends on the level of detail you want to capture and how you want to receive the data\.

   1. 

**Destination options**
      + **Destination type** – when you select the radio button next to the AWS service to publish your events to, a details panel will appear with fields respective to the service\. Selecting the links below will give instructions about the service's detail panel:
        + [Amazon CloudWatch](event-publishing-add-event-destination-cloudwatch.md)
        + [Amazon Kinesis Data Firehose](event-publishing-add-event-destination-firehose.md)
        + Amazon Pinpoint
        + [Amazon SNS](event-publishing-add-event-destination-sns.md)

        To learn more about using the event publishing model to monitor your email operation, see [Monitor email sending using Amazon SES event publishing](monitor-using-event-publishing.md)\.
      + **Name** – enter the name of the destination for this configuration set\. The name can include letters, numbers, dashes, and hyphens\.
      + **Event publishing** – to turn on event publishing for this destination, select the **Enabled** check box\.

   1. Choose **Next** to continue\.

1. 

**Review**

   When you are satisfied that your entries are correct, choose **Add destination** to add your event destination\.