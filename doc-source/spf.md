# Authenticating Email with SPF in Amazon SES<a name="spf"></a>

Sender Policy Framework \(SPF\) is an email validation standard, defined in [RFC 7208](https://tools.ietf.org/html/rfc7208), designed to combat email spoofing\. SPF enables domain owners to specify which mail servers are authorized to send email for their domain\. To indicate compliance with SPF, the domain owner publishes a list of authorized mail servers in a DNS record on the domain's DNS server\. When a receiving mail server receives an email that contains the domain in the MAIL FROM address, it checks the domain's DNS records to compare the sending mail server to the authorized mail servers and takes action on the email accordingly\.

An SPF record indicates to ISPs that you have authorized Amazon SES to send email for your domain\. When you use Amazon SES, your decision about whether to publish an SPF record depends on whether you only require your email to pass an SPF check by the receiving mail server, or if you want your email to comply with the additional requirements needed to pass Domain\-based Message Authentication, Reporting and Conformance \(DMARC\) authentication based on SPF\. You can use DKIM to achieve DMARC validation, but it is a best practice to use both DKIM and SPF for maximum deliverability\.
+ **To pass an SPF check**—When you use Amazon SES, there are two setups with which you can pass an SPF check\. The first setup is to use the default MAIL FROM domain of Amazon SES, and to not publish an SPF record at all\. This setup enables you to pass an SPF check because by default, Amazon SES uses its own MAIL FROM domain to send your emails\. In this case, an SPF check will pass because the default MAIL FROM domain is *amazonses\.com* \(or a subdomain of that\) and the sending mail server is Amazon SES\. 

  The other setup with which you can pass an SPF check is to configure Amazon SES to use your own MAIL FROM domain, in which case you must publish an SPF record because the MAIL FROM domain and the domain of the sending mail server, Amazon SES, are different\. Instructions for configuring your domain to send emails using a custom MAIL FROM domain are provided in [Using a Custom MAIL FROM Domain](mail-from.md)\.
+ **To pass DMARC validation based on SPF**—If you want DMARC validation to succeed based on SPF, you must [set up a custom MAIL FROM domain](mail-from.md) and publish an SPF record\. Note that the alignment mode in the DMARC policy must be relaxed, which is the default\. For more information about DMARC policies, see [https://dmarc\.org/](https://dmarc.org/)\.

## Adding an SPF Record<a name="spf-records"></a>

The procedure for adding a TXT record to your domain's DNS settings depends on who provides your DNS service, but for general instructions, see *Adding a TXT Record to Your Domain's DNS Server* in [Amazon SES Domain Verification TXT Records](dns-txt-records.md)\. For information about SPF records, see [RFC 7208](https://tools.ietf.org/html/rfc7208)\.

### Adding a New SPF Record<a name="spf-records-new"></a>

If your custom MAIL FROM domain does not have an existing SPF record, publish a TXT record with the following value\. The name of the record can be blank or @, depending on your DNS service\.

```
1. "v=spf1 include:amazonses.com ~all"
```

### Adding to an Existing SPF Record<a name="spf-records-preexisting"></a>

If your domain already has an SPF record, then you must add the following SPF mechanism to the existing record\.

```
1. include:amazonses.com
```