# Set up an Amazon Pinpoint event destination for event publishing<a name="event-publishing-add-event-destination-pinpoint"></a>

A event destination notifies you about specific email sending events using Amazon Pinpoint\. Because an Amazon Pinpoint event destination only exists within a configuration set, you have to [create a configuration set](event-publishing-create-configuration-set.md) before you add the event destination to the configuration set\.

The procedure in this section shows how to add Amazon Pinpoint event destination details to a configuration set and assumes you have completed steps 1 through 6 in [Creating an event destination](event-destinations-manage.md#event-destination-add)\.

You can also use the [UpdateConfigurationSetEventDestination](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_UpdateConfigurationSetEventDestination.html) operation in the Amazon SES API V2 to create and modify event destinations\.

There are additional charges for the types of channels you have configured in your Amazon Pinpoint projects\. For more information, see [Amazon Pinpoint Pricing](https://aws.amazon.com/pinpoint/pricing/)\.

**To add Amazon Pinpoint event destination details to a configuration set using the console**

1. These are the detailed instructions for selecting Amazon Pinpoint as your event destination type in [Step 7](event-destinations-manage.md#specify-event-dest-step) and assumes you have completed all the previous steps in [Creating an event destination](event-destinations-manage.md#event-destination-add)\.
**Note**  
Amazon Pinpoint does not support event types **Delivery delays** or **Subscriptions**\.

   After selecting the Amazon Pinpoint **Destination type** and enabling **Event publishing**, the **Amazon Pinpoint project details** panel will appear \- its fields are addressed in the following steps\.

1. For **Project**, choose an existing Amazon Pinpoint project, or choose **Create a new project in Amazon Pinpoint** to create a new one\.

   For information about creating a project, see [Create a project](https://docs.aws.amazon.com/pinpoint/latest/userguide/gettingstarted-create-project.html) in the *Amazon Pinpoint User Guide*\.

1. Choose **Next**\.

1. On the review screen, if you're satisfied with how you defined your event destination, choose **Add destination**\. This will open the event destination's summary page where a success banner will confirm if your event destination was created or modified successfully\.