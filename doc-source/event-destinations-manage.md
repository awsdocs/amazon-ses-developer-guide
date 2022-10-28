# Creating Amazon SES event destinations<a name="event-destinations-manage"></a>

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

To learn more about setting up event publishing, see [Monitor email sending using Amazon SES event publishing](monitor-using-event-publishing.md)\.

## Creating an event destination<a name="event-destination-add"></a>

After you’ve created a configuration set, you have the option to create event destinations for your configuration set which enables event publishing that is triggered on the event types you specify for the event destination\. A configuration set can have multiple event destinations with multiple event types defined\.

If you haven't created a configuration set, see [Creating configuration sets in Amazon SES](creating-configuration-sets.md)\.

The following steps show how to create or add an event destination to a configuration set\.

**To create or add and event destination using the SES console:**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane, under **Configuration**, choose **Configuration sets**\.

1. Choose a configuration set's name from the **Name** column to access its details\.

1. Select the **Event destinations** tab\.

1. Choose **Add destination**\.

1. <a name="select-event-types-step"></a>

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
        + **Delivery delays** – the email couldn't be delivered to the recipient’s mail server because a temporary issue occurred\. Delivery delays can occur, for example, when the recipient's inbox is full, or when the receiving email server experiences a transient issue\. *\(This event type not supported by Amazon Pinpoint\.\)*
        + **Subscriptions** – the email was successfully delivered, but the recipient updated the subscription preferences by clicking `List-Unsubscribe` in the email header or the `Unsubscribe` link in the footer\. *\(This event type not supported by Amazon Pinpoint\.\)*
      + **Open and click tracking** – to measure subscriber engagement, choose one or both of the check boxes to track **Opens** and **Clicks**\.
        + **Opens** – the recipient received the message and opened it in their email client\.
        + **Clicks** – the recipient clicked one or more links in the email\.
        + **Configuration set redirect domain** – this field will appear and be prepopulated with the name of the custom redirect domain if you assigned one when creating the configuration set\.
**Note**  
You can update the **Custom redirect domain** in the configuration set for open and click tracking under that domain\. For more information about configuring custom open and click domains see [Configuring custom domains to handle open and click tracking](configure-custom-open-click-domains.md)\.

   1. Choose **Next** to continue\.

1. <a name="specify-event-dest-step"></a>

**Specify destination**

   An event destination is an AWS service to which email sending events can be published\. Choosing the appropriate destination depends on the level of detail you want to capture and how you want to receive the data\.

   1. 

**Destination options**
      + **Destination type** – when you select the radio button next to the AWS service to publish your events to, a details panel will appear with fields respective to the service\. Selecting the links below will give instructions about the service's detail panel:
        + [Amazon CloudWatch](event-publishing-add-event-destination-cloudwatch.md)
        + [Amazon Kinesis Data Firehose](event-publishing-add-event-destination-firehose.md)
        + [Amazon Pinpoint](event-publishing-add-event-destination-pinpoint.md) *\(Does not support **Delivery delays** or **Subscriptions** event types\.\)*
        + [Amazon SNS](event-publishing-add-event-destination-sns.md)

        To learn more about using the event publishing model to monitor your email operation, see [Monitor email sending using Amazon SES event publishing](monitor-using-event-publishing.md)\.
      + **Name** – enter the name of the destination for this configuration set\. The name can include letters, numbers, dashes, and hyphens\.
      + **Event publishing** – to turn on event publishing for this destination, select the **Enabled** check box\.

   1. Choose **Next** to continue\.

1. 

**Review**

   When you are satisfied that your entries are correct, choose **Add destination** to add your event destination\.

You can also create an event destination using the Amazon SES console, the Amazon SES API v2, or the Amazon SES CLI v2\.

**To create an event destination using the SES API:**
+ For creating an event destination using the SES API, see [https://docs.aws.amazon.com/ses/latest/APIReference/API_CreateConfigurationSetEventDestination.html](https://docs.aws.amazon.com/ses/latest/APIReference/API_CreateConfigurationSetEventDestination.html)\.

## Editing, disabling/enabling, or deleting an event destination<a name="event-destination-edit"></a>

Follow these steps to edit, disable/enable, or delete an event destination using the SES console:

**To edit, disable/enable, or delete an event destination using the SES console:**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane, under **Configuration**, choose **Configuration sets**\.

1. Choose a configuration set's name from the **Name** column to access its details\.

1. Select the configuration set's **Event destinations** tab\.

1. Select the event destination's name under the **Name** column\.

1. 
   + **To edit** – Choose the **Edit** button on the respective panel for the set of fields you want to edit and make your changes followed by **Save changes**\.
   + **To disable or enable** – Choose the button that's either labeled **Disable** or **Enable** in the upper\-right corner\.
   + **To delete** – Choose the **Delete** button in the upper\-right corner\.

You can also edit, disable/enable, or delete an event destination using the Amazon SES console, the Amazon SES API v2, or the Amazon SES CLI v2\.

**To edit, disable/enable, or delete an event destination using the SES API:**

1. For disabling/enabling an event destination using the SES API, see [https://docs.aws.amazon.com/ses/latest/APIReference/API_UpdateConfigurationSetEventDestination.html](https://docs.aws.amazon.com/ses/latest/APIReference/API_UpdateConfigurationSetEventDestination.html)\.

1. For deleting an event destination using the SES API, see [https://docs.aws.amazon.com/ses/latest/APIReference/API_DeleteConfigurationSetEventDestination.html](https://docs.aws.amazon.com/ses/latest/APIReference/API_DeleteConfigurationSetEventDestination.html)\.