# Authenticating Email with SPF in Amazon SES<a name="send-email-authentication-spf"></a>

*Sender Policy Framework* \(SPF\) is an email validation standard that's designed to prevent email spoofing\. Domain owners use SPF to tell email providers which servers are allowed to send email from their domains\. SPF is defined in [RFC 7208](https://tools.ietf.org/html/rfc7208)\.

To set up SPF, you publish a TXT record to the DNS configuration for your domain\. This record contains a list of the servers that you authorize to send email from your domain\. When an email provider receives a message from your domain, it checks the DNS records for your domain to make sure that the email was sent from an authorized server\.

When you send email through Amazon SES, the messages that you send pass an SPF check by default\. Amazon SES specifies a MAIL FROM domain for each message that is a subdomain of *amazonses\.com*, and the sending mail server for the message aligns with this domain\.

You can optionally publish your own SPF record\. By publishing an SPF record, your email can comply with Domain\-based Message Authentication, Reporting and Conformance \(DMARC\)\. For more information, see [Complying with DMARC](send-email-authentication-dmarc.md)\.

## Adding an SPF Record<a name="send-email-authentication-spf-records"></a>

To publish an SPF record, you have to add a new TXT record to the DNS configuration for your domain\. The procedures for updating DNS records vary depending on which DNS or web hosting provider you use\.

The following table includes links to the documentation for several common providers\. This list isn't exhaustive, and inclusion in this list isn't an endorsement or recommendation of any company's products or services\. If your provider isn't listed in the table, you can probably still publish an SPF record\.


| DNS/Hosting Provider | Documentation Link | 
| --- | --- | 
| Amazon Route 53 | [Creating Records by Using the Amazon Route 53 Console](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resource-record-sets-creating.html) | 
|  GoDaddy  |  [Add an SPF record](https://www.godaddy.com/help/add-an-spf-record-19218) \(external link\)  | 
|  Dreamhost  |  [How do I add an SPF record?](https://help.dreamhost.com/hc/en-us/articles/216106197-How-do-I-add-an-SPF-record-) \(external link\)  | 
|  Cloudflare  |  [Managing DNS records in CloudFlare](https://support.cloudflare.com/hc/en-us/articles/360019093151) \(external link\)  | 
|  HostGator  |  [SPF Records](https://www.hostgator.com/help/article/spf-records) \(external link\)  | 
|  Namecheap  |  [How do I add TXT/SPF/DKIM/DMARC records for my domain? ](https://www.namecheap.com/support/knowledgebase/article.aspx/317/2237/how-do-i-add-txtspfdkimdmarc-records-for-my-domain) \(external link\)  | 
|  Names\.co\.uk  |  [Changing your domains DNS Settings](https://www.names.co.uk/support/1156-changing_your_domains_dns_settings.html) \(external link\)  | 
|  Wix  |  [Adding or Updating SPF Records in Your Wix Account](https://support.wix.com/en/article/adding-or-updating-spf-records-in-your-wix-account) \(external link\)  | 

If your domain doesn't have an existing SPF record, publish a TXT record with the following value\. The name of the record can be blank or @, depending on your DNS service\.

```
1. "v=spf1 include:amazonses.com ~all"
```

SPF records can contain multiple `include` statements\. If your domain already has an SPF record, you can add an `include` statement for Amazon SES by using the following format: 

```
1. "v=spf1 include:example.com include:amazonses.com ~all"
```