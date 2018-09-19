# Configuring Amazon SNS Notifications for Amazon SES<a name="configure-sns-notifications"></a>

Amazon SES can notify you of your bounces, complaints, and deliveries through [Amazon Simple Notification Service \(Amazon SNS\)](https://aws.amazon.com/sns)\.

You can configure notifications in the Amazon SES console, or by using the Amazon SES API\.

**Topics**
+ [Prerequisites](#configure-feedback-notifications-prerequisites)
+ [Configuring Notifications Using the Amazon SES Console](#configure-feedback-notifications-console)
+ [Configuring Notifications Using the Amazon SES API](#configure-feedback-notifications-api)

## Prerequisites<a name="configure-feedback-notifications-prerequisites"></a>

Complete the following steps before you set up Amazon SNS notifications in Amazon SES:

1. Create a topic in Amazon SNS\. For more information, see [Create a Topic](https://docs.aws.amazon.com/sns/latest/dg/CreateTopic.html) in the *Amazon Simple Notification Service Developer Guide*\.

1. Subscribe at least one endpoint to the topic\. For example, if you want to receive notifications by text message, subscribe an SMS endpoint \(that is, a mobile phone number\) to the topic\. To receive notifications by email, subscribe an email endpoint \(an email address\) to the topic\. 

   For more information, see [Subscribe to a Topic](https://docs.aws.amazon.com/sns/latest/dg/SubscribeTopic.html) in the *Amazon Simple Notification Service Developer Guide*\.

## Configuring Notifications Using the Amazon SES Console<a name="configure-feedback-notifications-console"></a>

**To configure notifications using the Amazon SES console**

1. Open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane, under **Identity Management**, choose **Domains** or **Email Addresses**\.

1. In the list of verified senders, choose the email address or domain you want to configure notifications for\.
**Important**  
Verified domain notification settings apply to all mail sent from email addresses in that domain *except* for email addresses that are also verified\.

1. Under **Notifications**, choose **Edit Configuration**\.

1. Under **SNS Topic Configuration**, make the following changes to the Amazon SNS topic configuration:

   1. Choose the Amazon SNS topics you want to use to receive notifications\. You can publish multiple event type notifications to the same Amazon SNS topic or to different Amazon SNS topics\. 
**Important**  
The Amazon SNS topics you use for bounce, complaint, and delivery notifications must be in the same AWS Region where you use Amazon SES\.  
Additionally, the topics you select must be subscribed to by one or more endpoints\. For example, if you want to have notifications sent to an email address, you must subscribe an email endpoint to the topic\. For more information, see [Subscribe to a Topic](https://docs.aws.amazon.com/sns/latest/dg/SubscribeTopic.html) in the *Amazon Simple Notification Service Developer Guide*\.

      If you want to use an Amazon SNS topic that you do not own, you must [configure your AWS Identity and Access Management \(IAM\) policy](https://docs.aws.amazon.com/IAM/latest/UserGuide/AccessPolicyLanguage.html) to allow publishing from the Amazon Resource Name \(ARN\) of the Amazon SNS topic\.

   1. If you want the Amazon SNS notifications to contain the original headers of the emails you pass to Amazon SES, choose **Include original headers**\. This option is only available if you have assigned an Amazon SNS topic to the associated notification type\. For information about the contents of the original email headers, see the `mail` object in [Amazon SNS Notification Contents](notification-contents.md)\. 

1. If you choose Amazon SNS topics for both bounces and complaints, you can disable email notifications entirely\. To disable email notifications for bounces and complaints, under **Email Feedback Forwarding**, choose **Disable**\. Delivery notifications are available only through Amazon SNS\.

1. Choose **Save Config**\. The changes you made to your notification settings might take a few minutes to take effect\.

After you configure your settings, you will start receiving bounce, complaint, and/or delivery notifications to your Amazon SNS topic\(s\)\. These notifications will be in [JavaScript Object Notation \(JSON\)](http://www.json.org) format and will follow the structure described in [Amazon SNS Notification Contents](notification-contents.md)\. 

You will be charged standard Amazon SNS rates for bounce, complaint, and delivery notifications\. For more information, see the [Amazon SNS pricing page](https://aws.amazon.com/sns/pricing)\.

**Note**  
If an attempt to publish to your Amazon SNS topic fails because the topic has been deleted or your AWS account no longer has permissions to publish to it, the Amazon SES configuration for that topic for the sending identity will be deleted, bounce and complaint notifications through email will be re\-enabled for that identity, and you will be notified of the change through email\. If you have multiple identities configured to use that topic, each identity will have its topic configuration changed when each identity experiences a failure to publish to that topic\.

## Configuring Notifications Using the Amazon SES API<a name="configure-feedback-notifications-api"></a>

You can also configure bounce, complaint, and delivery notifications by using the Amazon SES API\. Use the following operations to configure notifications:
+ [SetIdentityNotificationTopic](https://docs.aws.amazon.com/ses/latest/APIReference/API_SetIdentityNotificationTopic.html)
+ [SetIdentityFeedbackForwardingEnabled](https://docs.aws.amazon.com/ses/latest/APIReference/API_SetIdentityFeedbackForwardingEnabled.html)
+ [GetIdentityNotificationAttributes](https://docs.aws.amazon.com/ses/latest/APIReference/API_GetIdentityNotificationAttributes.html)
+ [SetIdentityHeadersInNotificationsEnabled](https://docs.aws.amazon.com/ses/latest/APIReference/API_SetIdentityHeadersInNotificationsEnabled.html)

You can use these API actions to write a customized front\-end application for notifications\. For a complete description of the API actions related to notifications, see the [Amazon Simple Email Service API Reference](https://docs.aws.amazon.com/ses/latest/APIReference/)\.