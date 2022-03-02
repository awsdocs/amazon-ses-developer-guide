# Setting up Amazon SES email receiving<a name="receiving-email-setting-up"></a>

This section describes the prerequisites that are required before you can begin to configure Amazon SES to receive your mail\. It's important that you've read [Email receiving concepts & use cases](receiving-email-concepts.md) to understand the concepts of how Amazon SES works and to consider how you want to receive, filter, and process your email\.

Before you can configure email receiving by creating a *rule set*, *receipt rules*, and *IP address filters*, you must first complete the following set up prerequisites:
+ Verify your domain with Amazon SES by publishing DNS records to prove that you own it\.
+ Permit Amazon SES to receive email for your domain by publishing an MX record\.
+ Give Amazon SES permission to access other AWS resources in order to execute receipt rule actions\.

When you create and verify a domain identity, you're publishing records to your DNS settings to complete the verification process, but this alone is not enough to use email receiving\. Specific to email receiving, it's also required to publish an MX record for specifying a custom mail\-from domain\. This record is used in your domain’s DNS settings to permit SES to receive email for your domain\. Giving permissions is required because the actions you choose in your receipt rules won’t work unless Amazon SES has permission to use the respective AWS service required for those actions\.

**Topics**
+ [Verifying your domain for Amazon SES email receiving](receiving-email-verification.md)
+ [Publishing an MX record for Amazon SES email receiving](receiving-email-mx-record.md)
+ [Giving permissions to Amazon SES for email receiving](receiving-email-permissions.md)