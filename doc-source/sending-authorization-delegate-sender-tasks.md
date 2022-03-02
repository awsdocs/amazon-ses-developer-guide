# Delegate sender tasks for Amazon SES sending authorization<a name="sending-authorization-delegate-sender-tasks"></a>

As a delegate sender, you're sending  emails on behalf of an identity that you don't own, but are authorized to use\. Even though you're sending on the identity owner's behalf, bounces and complaints count toward the bounce and complaint metrics for your AWS account, and the number of messages you send counts toward your sending quota\. You're also responsible for requesting any sending quota increases that you might need in order to send the identity owner's emails\.

As a delegate sender, you must complete the following tasks:
+ [Providing information to the identity owner](sending-authorization-delegate-sender-tasks-information.md)
+ [Using delegate sender notifications](sending-authorization-delegate-sender-tasks-notifications.md)
+ [Sending emails for the identity owner](sending-authorization-delegate-sender-tasks-email.md)