# Verifying domains in Amazon SES<a name="verify-domains"></a>

Amazon SES requires that you verify your email address or domain, to confirm that you own it and to prevent others from using it\. When you verify an entire domain, you are verifying all email addresses from that domain, so you don't need to verify email addresses from that domain individually\. For example, if you verify the domain *example\.com*, you can send email from *user1@example\.com*, *user2@example\.com*, or any other user at *example\.com*\.

You can manage your verified domains by using the Amazon SES console or the Amazon SES API\. For a complete description of API actions related to domain verification, go to the [Amazon Simple Email Service API Reference](https://docs.aws.amazon.com/ses/latest/APIReference/)\. This section, which demonstrates the actions using the Amazon SES console, contains the following topics:
+ [Verifying a domain with Amazon SES](verify-domain-procedure.md)
+ [Listing domain identities in Amazon SES](view-verified-domains.md)
+ [Deleting a domain identity in Amazon SES](remove-verified-domain.md)
+ [Amazon SES domain verification revocation](verified-domain-revocation.md)
+ [Amazon SES domain verification TXT records](dns-txt-records.md)

Important notes about domain verification are as follows:
+ Amazon SES has endpoints in multiple AWS regions, and domain verification applies to each AWS region separately\. You must perform the entire domain verification procedure for each region in which you want to send from a given domain\. If you want to verify the same domain in multiple regions and your DNS provider does not allow you to have multiple TXT records with the same name, see the workarounds in [Common Domain Verification Problems](troubleshoot-verification.md#troubleshoot-verification-domain)\. 
+ If you verify a domain with Amazon SES, you can send from any subdomain of that domain without specifically verifying the subdomain\. For example, if you verify *example\.com*, you do not need to verify *a\.example\.com* or *a\.b\.example\.com*\. As specified in [RFC 1034](https://tools.ietf.org/html/rfc1034#section-3.6), each DNS label can have up to 63 characters and the whole domain name must not exceed a total length of 255 characters\. 
+ If you verify a domain, subdomain\(s\), and/or email address\(es\) that share a root domain, the verified identity settings \(such as feedback notifications and Easy DKIM\) apply at the most granular level you verified\. That is:
  + Verified email address settings override verified domain settings\.
  + Verified subdomain settings override verified domain settings, with lower\-level subdomain settings overriding higher\-level subdomain settings\.

  For example, assume you verify *user@a\.b\.example\.com*, *a\.b\.example\.com*, *b\.example\.com*, and *example\.com*\. These are the verified identity settings that will be used in the following scenarios:
  + Emails sent from *user@example\.com* \(an address that is not specifically verified\) will use the settings for *example\.com*\.
  + Emails sent from *user@a\.b\.example\.com* \(an address that *is* specifically verified\) will use the settings for *user@a\.b\.example\.com*\.
  + Emails sent from *user@b\.example\.com* \(an address that is not specifically verified\) will use the settings for *b\.example\.com*\.
+ Domain names are case\-insensitive\. If you verify *example\.com*, you can send from *EXAMPLE\.com* also\.
+ In each AWS Region, you can verify as many as 10,000 identities \(domains and email addresses, in any combination\)\.