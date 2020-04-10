# Verifying Identities in Amazon SES<a name="verify-addresses-and-domains"></a>

In Amazon SES, an *identity* is an email address or domain that you use to send email\. Before you can send an email using Amazon SES, you must verify each identity that you're going to use as a "From", "Source", "Sender", or "Return\-Path" address to prove that you own it\. If your account is still in the Amazon SES sandbox, you also need to verify any email addresses that you send emails to, except for email addresses provided by the [Amazon SES mailbox simulator](send-email-simulator.md)\.

You can verify an identity by using the Amazon SES console or the Amazon SES API\.

**Topics**
+ [Verifying Email Addresses in Amazon SES](verify-email-addresses.md)
+ [Verifying Domains in Amazon SES](verify-domains.md)