# Amazon SES global suppression list<a name="sending-email-global-suppression-list"></a>

Amazon SES maintains an internal *global suppression list* which operates and is managed in the background by SES\. When any SES customer sends an email that results in a hard bounce, SES adds the email address that produced the bounce to a global suppression list\. The global suppression list is *global* in the sense that it applies to all SES customers\. In other words, if a different customer attempts to send an email to an address that's on the global suppression list, SES accepts the message, but doesn't send it, because the email address is suppressed\.

The global suppression list email address removal request feature *is no longer customer facing and you no longer interact with it* to manage suppression lists\. To replace this functionality, Amazon SES now offers a new way for you to manage your suppression lists by making available **account\-level suppression lists** and **configuration set\-level suppression lists** that offer you more customized control over how you handle email suppression for your own account\. For more information, see [Using the Amazon SES account\-level suppression list](sending-email-suppression-list.md) and [Using configuration set\-level suppression to override your account\-level suppression list](sending-email-suppression-list-config-level.md)\.

**Important**  
The global suppression list email address removal request form is no longer in the Amazon SES console because the account\-level suppression list has superseded it\. To learn how to use the account\-level suppression list, see [Using the Amazon SES account\-level suppression list](sending-email-suppression-list.md)\.

## Global suppression list considerations<a name="sending-email-global-suppression-list-considerations"></a>

Key factors regarding the global suppression list:
+ The global suppression list operates and is managed in the background by SES \- you cannot interact with it directly; however, you can override it by using your own [account\-level suppression list](sending-email-suppression-list.md)\.
+ The global suppression list is enabled by default for all SES accounts\. You can't disable it\.
+ Because SES applies the global suppression list to all customers, you can't query the global suppression list or add addresses to it manually\.
+ When an email address produces a hard bounce, SES adds the address to the global suppression list for a short period of time\. After that period of time elapses, SES removes the address from the list\. If the address produces another hard bounce, SES adds it back to the global suppression list for a longer period of time, and removes it at the end of that period\. The amount of time that an address remains on the global suppression list increases each time the address produces a hard bounce\. An address can remain on the global suppression list for up to 14 days\.
+ If you attempt to send a message to an address that's on the global suppression list, SES accepts the message, but doesn't send it\. SES generates a bounce notification with a `bounceType` value of `Permanent`, and a `bounceSubType` value of `Suppressed`\. Receiving this type of bounce notification is the only way to know if an address is on the global suppression list\. You can't query the global suppression list\.
+ SES counts the messages that you send to addresses on the global suppression list toward the bounce rate for your account and toward your daily sending quota\.
+ As with any email address that produces a hard bounce, you should remove addresses that cause a suppression list bounce from your mailing list unless you're certain that the address is valid\.
+ Suppression list bounces count towards your account's bounce rate\. If your bounce rate gets too high, your account might be placed under review or your account's ability to send email could be paused\.

**Note**  
It's important to understand how the three SES suppression lists are interrelated and their hierarchy, see [Overview of the three types of suppression lists](lists-and-subscriptions.md#3-suppression-overview)\.