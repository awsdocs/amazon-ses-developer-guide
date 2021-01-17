# Set up an Amazon SNS event destination for event publishing<a name="event-publishing-add-event-destination-sns"></a>

A event destination notifies you about specific email sending events using Amazon SNS\. Because an Amazon SNS event destination only exists within a configuration set, you have to [create a configuration set](event-publishing-create-configuration-set.md) before you add the event destination to the configuration set\.

This section includes a procedure for creating an event destination by using the Amazon SES console\. You can also use the [UpdateConfigurationSetEventDestination](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_UpdateConfigurationSetEventDestination.html) operation in the Amazon SES API V2 destination to create and update event destinations\. 

**Note**  
It's also possible to receive notifications through Amazon SNS at the account level\. This means that you can receive Amazon SNS notifications every time a sending event occurs across your entire Amazon SES account\. By using event publishing rather than account\-level notifications, you can configure Amazon SES to only send notifications about specific event types, or only for emails sent using a particular configuration set\. For more information about setting up account\-level Amazon SNS notifications, see [Monitoring Amazon SES email sending using notifications](monitor-sending-activity-using-notifications.md)\.

There are additional charges for sending messages to the endpoints that are subscribed to your Amazon SNS topics\. For more information, see [Amazon SNS Pricing](https://aws.amazon.com/sns/pricing/)\.

**To add an Amazon SNS event destination to a configuration set**

1. If you have not already done so, create an Amazon SNS topic and subscribe to it\. For more information, see [Getting Started](https://docs.aws.amazon.com/sns/latest/dg/GettingStarted.html) in the *Amazon Simple Notification Service Developer Guide*\.

1. Open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane, choose **Configuration Sets**\.

1. Choose a configuration set from the configuration set list\. If the list is empty, you must first [create a configuration set](event-publishing-create-configuration-set.md)\.

1. For **Add Destination**, choose **Select a destination type**, and then choose **SNS**\.

1. For **Name**, type a name for the event destination\.

1. For **Event types**, select at least one event type to publish to the event destination:
   + **Sends** – The call to Amazon SES was successful and Amazon SES will attempt to deliver the email\.
   + **Rejects** – Amazon SES accepted the email, determined that it contained a virus, and rejected it\. Amazon SES didn't attempt to deliver the email to the recipient's mail server\.
   + **Bounces** – The recipient's mail server permanently rejected the email\. This event corresponds to hard bounces\. Soft bounces are only included when Amazon SES fails to deliver the email after retrying for a period of time\.
   + **Complaints** – The email was successfully delivered to the recipient\. The recipient marked the email as spam\.
   + **Deliveries** – Amazon SES successfully delivered the email to the recipient's mail server\.
   + **Opens** – The recipient received the message and opened it in their email client\.
   + **Clicks** – The recipient clicked one or more links in the email\.
   + **Rendering Failures** – The email wasn't sent because of a template rendering issue\. This event type only occurs when you send email using the [SendTemplatedEmail](https://docs.aws.amazon.com/ses/latest/APIReference/API_SendTemplatedEmail.html) or [SendBulkTemplatedEmail](https://docs.aws.amazon.com/ses/latest/APIReference/API_SendBulkTemplatedEmail.html) API operations\. This event type can occur when template data is missing, or when there is a mismatch between template parameters and data\.
   + **Delivery Delays** – The email couldn't be delivered to the recipient because a temporary issue occurred\. Delivery delays can occur, for example, when the recipient's inbox is full, or when the receiving email server experiences a transient issue\.
**Note**  
To add the `DELIVERY_DELAY` event type to an event destination, you have to use the [ UpdateConfigurationSetEventDestination](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_UpdateConfigurationSetEventDestination.html) operation in the Amazon SES API V2\. Currently, you can't add this event type to a configuration set by using the Amazon SES console\.

1. Select **Enabled**\.

1. For **Topic**, choose an existing Amazon SNS topic, or choose **Create new topic** to create a new one\.

   For information about creating a topic, see [Create a Topic](https://docs.aws.amazon.com/sns/latest/dg/CreateTopic.html) in the *Amazon Simple Notification Service Developer Guide*\.

1. Choose **Save**\.

1. To use a configuration set when sending an email, see [Specifying a Configuration Set When You Send Email](using-configuration-sets-in-email.md)\.