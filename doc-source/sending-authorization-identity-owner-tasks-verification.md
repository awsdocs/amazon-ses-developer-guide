# Verifying an Identity for Amazon SES Sending Authorization<a name="sending-authorization-identity-owner-tasks-verification"></a>

The first step in configuring sending authorization is to prove that you own the email address or domain that the delegate sender will use to send email\. The verification procedure is described in [Verifying Identities](verify-addresses-and-domains.md)\.

You can confirm that an email address or domain is verified by checking its status in the Identity Management section of the [Amazon SES console](https://console.aws.amazon.com/ses/) or by using the `GetIdentityVerificationAttributes` API operation\.

Before you or the delegate sender can send email to non\-verified email addresses, you have to submit a request to have your account removed from the Amazon SES sandbox\. For more information, see [Moving Out of the Amazon SES Sandbox](request-production-access.md)\.

**Important**  
The AWS accounts of **both** the identity owner and the delegate sender have to be removed from the sandbox before either account can send email to non\-verified addresses\.