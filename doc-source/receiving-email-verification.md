# Verifying your domain for Amazon SES email receiving<a name="receiving-email-verification"></a>

As with any domain you want to use for sending or receiving email with Amazon SES, you must first prove that you own it\. The verification procedure, which includes initiating domain verification with Amazon SES and then publishing a TXT record to your DNS server, is described in [Verifying domains in Amazon SES](verify-domains.md)\.

**Note**  
Although Amazon SES enables you to verify single email addresses, you must verify a domain if you want to use Amazon SES for email receiving\.

You can also start the domain verification process when you set up receipt rules in [Creating receipt rules](receiving-email-receipt-rules.md)\. The recipient list will indicate which recipients are not verified, and enable you to initiate verification\. In any case, you must complete domain verification by publishing a TXT record to your DNS server, as described in [Amazon SES domain verification TXT records](dns-txt-records.md)\.

You can confirm that your email address or domain is verified by looking at its status in the **Email Address Identities** or **Domain Identities** list in the Amazon SES console or by using the Amazon SES `GetIdentityVerificationAttributes` API\.
