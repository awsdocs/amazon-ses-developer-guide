# Using Notifications for Amazon SES Email Receiving<a name="receiving-email-notifications"></a>

When you receive an email, Amazon SES executes the rules in the active receipt rule set\. You can configure receipt rules to send you notifications using Amazon SNS\. Your receipt rules can send two different types of notifications:

+ **Notifications sent from SNS actions** – When you add an [SNS](receiving-email-action-sns.md) action to a receipt rule, it sends information about the email\. If the message is 150KB or smaller, this notification type also includes the complete MIME body of the email\.

+ **Notifications sent from other action types** – When you add any other action type \(including [Bounce](receiving-email-action-bounce.md), [Lambda](receiving-email-action-lambda.md), [Stop Rule Set](receiving-email-action-stop.md), or [WorkMail](receiving-email-action-workmail.md) actions\) to a receipt rule, you can optionally specify an Amazon SNS topic\. If you do, you will receive notifications when these actions are performed\. These notifications contain information about the email, but do not contain the content of the email\.

This section describes the contents of these notifications, and provides an example of each type of notification\.


+ [Contents of Notifications for Amazon SES Email Receiving](receiving-email-notifications-contents.md)
+ [Examples of Notifications for Amazon SES Email Receiving](receiving-email-notifications-examples.md)