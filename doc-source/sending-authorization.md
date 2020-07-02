# Using sending authorization with Amazon SES<a name="sending-authorization"></a>

You can configure Amazon SES to authorize other users to send emails from addresses or domains that you own \(your *identities*\) using their own Amazon SES accounts\. This feature, called *sending authorization*, lets you maintain control over your identities so that you can change or revoke the permissions at any time\. For example, if you are a business owner, you can use sending authorization to enable a third party \(such as an email marketing company\) to send email from a domain you own\.

If you want to authorize someone to send emails on your behalf, then you are an *identity owner*\. If you are an identity owner, we recommend that you read the following sections:
+ [Overview of Sending Authorization](sending-authorization-overview.md)
+ [Sending Authorization Policies](sending-authorization-policies.md)
+ [Sending Authorization Policy Examples](sending-authorization-policy-examples.md)
+ [Identity Owner Tasks](sending-authorization-identity-owner-tasks.md)

If you have been authorized to send emails on behalf of someone else, then you are a *delegate sender*\. If you are a delegate sender, we recommend that you read the following sections:
+ [Overview of Sending Authorization](sending-authorization-overview.md)
+ [Delegate Sender Tasks](sending-authorization-delegate-sender-tasks.md)

**Note**  
You can also control access to Amazon SES by using IAM policies\. IAM policies constrain what individual IAM users can do, while sending authorization policies constrain how individual verified identities can be used\. Further, only sending authorization policies can grant cross\-account access\. For more information about using IAM policies with Amazon SES, see [Controlling access to Amazon SES](control-user-access.md)\.


****  

|  | 
| --- |
| For information and discussions about a variety of topics related to Amazon SES, see the [AWS Messaging and Targeting Blog](https://aws.amazon.com//blogs/messaging-and-targeting/)\. To browse and post questions, go to the [Amazon SES Forum](https://forums.aws.amazon.com/forum.jspa?forumID=90)\. | 