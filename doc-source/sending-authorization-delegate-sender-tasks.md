# Delegate Sender Tasks for Amazon SES Sending Authorization<a name="sending-authorization-delegate-sender-tasks"></a>

As a delegate sender, you are sending *cross\-account* emails\. This means that you are sending emails on behalf of an identity that you do not own, but are authorized to use\. Even though you are sending on the identity owner's behalf, bounces and complaints count toward the bounce and complaint metrics for your AWS account, and the number of messages you send counts toward your sending quota\. You are also responsible for requesting any sending limit increases that you might need in order to send the identity owner's emails\.

As a delegate sender, you must complete the following tasks:

+ [Providing Information to the Identity Owner](sending-authorization-delegate-sender-tasks-information.md)

+ [Using Delegate Sender Notifications](sending-authorization-delegate-sender-tasks-notifications.md)

+ [Sending Emails for the Identity Owner](sending-authorization-delegate-sender-tasks-email.md)