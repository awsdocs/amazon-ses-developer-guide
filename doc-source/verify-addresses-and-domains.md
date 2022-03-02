# Verified identities in Amazon SES<a name="verify-addresses-and-domains"></a>

In Amazon SES, a *verified identity* is a domain or email address that you use to send or receive email\. Before you can send an email using Amazon SES, you must create and verify each identity that you're going to use as a "From", "Source", "Sender", or "Return\-Path" address\. Verifying an identity with Amazon SES confirms that you own it and helps prevent unauthorized use\.

If your account is still in the Amazon SES sandbox, you also need to verify any email addresses which you plan on sending email to, unless you're sending to test inboxes provided by the [Amazon SES mailbox simulator](send-an-email-from-console.md#send-email-simulator)\. For more information, see [Using the mailbox simulator manually](send-an-email-from-console.md#send-email-simulator)\.

You can create an identity by using the Amazon SES console or the Amazon SES API\. The identity verification process depends on which type of identity you choose to create\.

**Topics**
+ [Creating and verifying identities in Amazon SES](creating-identities.md)
+ [Managing identities in Amazon SES](managing-identities.md)
+ [Configuring identities in Amazon SES](configure-identities.md)
+ [Sending test emails in Amazon SES with the simulator](send-an-email-from-console.md)