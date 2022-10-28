# Set up an Amazon SNS event destination for event publishing<a name="event-publishing-add-event-destination-sns"></a>

A event destination notifies you about specific email sending events using Amazon SNS\. Because an Amazon SNS event destination only exists within a configuration set, you have to [create a configuration set](event-publishing-create-configuration-set.md) before you add the event destination to the configuration set\.

The procedure in this section shows how to add Amazon SNS event destination details to a configuration set and assumes you have completed steps 1 through 6 in [Creating an event destination](event-destinations-manage.md#event-destination-add)\.

You can also use the [UpdateConfigurationSetEventDestination](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_UpdateConfigurationSetEventDestination.html) operation in the Amazon SES API V2 to create and modify event destinations\.

**Note**  
It's also possible to receive notifications through Amazon SNS at the account level\. This means that you can receive Amazon SNS notifications every time a sending event occurs across your entire Amazon SES account\. By using event publishing rather than account\-level notifications, you can configure Amazon SES to only send notifications about specific event types, or only for emails sent using a particular configuration set\. For more information about setting up account\-level Amazon SNS notifications, see [Setting up event notification for Amazon SES](monitor-sending-activity-using-notifications.md)\.

There are additional charges for sending messages to the endpoints that are subscribed to your Amazon SNS topics\. For more information, see [Amazon SNS Pricing](https://aws.amazon.com/sns/pricing/)\.

**To add Amazon SNS event destination details to a configuration set using the console**

1. These are the detailed instructions for selecting Amazon SNS as your event destination type in [Step 7](event-destinations-manage.md#specify-event-dest-step) and assumes you have completed all the previous steps in [Creating an event destination](event-destinations-manage.md#event-destination-add)\. After selecting the Amazon SNS **Destination type** and enabling **Event publishing**, the **Amazon Simple Notification Service \(SNS\) topic** panel will appear \- its fields are addressed in the following steps\.

1. For **SNS topic**, choose an existing Amazon SNS topic, or choose **Create SNS topic** to create a new one\.

   For information about creating a topic, see [Create a Topic](https://docs.aws.amazon.com/sns/latest/dg/CreateTopic.html) in the *Amazon Simple Notification Service Developer Guide*\.
**Important**  
When you create your topic using Amazon SNS, for **Type**, only choose **Standard**\. \(SES does not support FIFO type topics\.\)

1. Choose **Next**\.

1. On the review screen, if you're satisfied with how you defined your event destination, choose **Add destination**\. This will open the event destination's summary page where a success banner will confirm if your event destination was created or modified successfully\.

1. Whether you created a new SNS topic or selected an existing one, you will now need to give access to SES to publish notifications to the topic\. On the event destination's summary page from the previous step, choose **Amazon SNS** from the **Destination type** column \- this will take you to the **Topics** list in the Amazon Simple Notification Service console \- *perform the following steps from the Amazon SNS console:*

   1. Select the name of the SNS topic you created or modified in the previous step\.

   1. On the topic's detail screen, choose **Edit**\.

   1. To give SES permission to publish notifications to the topic, on the **Edit topic** screen in the SNS console, expand **Access policy** and in the **JSON editor**, add the following permission policy:

      ```
      {
        "Version": "2012-10-17",
        "Id": "notification-policy",
        "Statement": [
          {
            "Effect": "Allow",
            "Principal": {
              "Service": "ses.amazonaws.com"
            },
            "Action": "sns:Publish",
            "Resource": "arn:aws:sns:topic_region:111122223333:topic_name",
            "Condition": {
              "StringEquals": {
                "AWS:SourceAccount": "111122223333",
                "AWS:SourceArn": "arn:aws:ses:topic_region:111122223333:configuration-set/configuration-set-name"
              }
            }
          }
        ]
      }
      ```

      Make the following changes to the preceding policy example:
      + Replace *topic\_region* with the AWS Region where you created the SNS topic\.
      + Replace *111122223333* with your AWS account ID\.
      + Replace *topic\_name* with the name of your SNS topic\.
      + Replace *configuration\-set\-name* with the name of your configuration set associated with the SNS event destination\.

   1. Choose **Save changes**\.