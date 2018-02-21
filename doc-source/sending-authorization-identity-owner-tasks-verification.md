# Verifying an Identity for Amazon SES Sending Authorization<a name="sending-authorization-identity-owner-tasks-verification"></a>

The first step in configuring sending authorization is to prove that you own the email address or domain from which the delegate sender will send email\. The verification procedure is described in [Verifying Identities](verify-addresses-and-domains.md)\.

You can confirm that an email address or domain is verified by checking its status in the Identity Management section of the [Amazon SES console](https://console.aws.amazon.com/ses/) or by using the `GetIdentityVerificationAttributes` API operation\.