# Complying with DMARC Using Amazon SES<a name="send-email-authentication-dmarc"></a>

Domain\-based Message Authentication, Reporting and Conformance \(DMARC\) is an email authentication protocol that uses Sender Policy Framework \(SPF\) and DomainKeys Identified Mail \(DKIM\) to detect email spoofing\. In order to comply with DMARC, messages must be authenticated through either SPF or DKIM, or both\.

This topic contains information that will help you configure Amazon SES so that the emails you send comply with both SPF and DKIM\. By complying with one of these authentication systems, your emails will comply with DMARC\. For information about the DMARC specification, see [http://www\.dmarc\.org](http://www.dmarc.org)\.

## Setting Up the DMARC Policy on Your Domain<a name="send-email-authentication-dmarc-dns"></a>

To set up DMARC, you have to modify the DNS settings for your domain\. The DNS settings for your domain should include a TXT record that specifies the domain's DMARC settings\. The procedures for adding TXT records to your DNS configuration depend on which DNS or hosting provider you use\. If you use Route 53, see [Working with Records](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/rrsets-working-with.html) in the *Amazon Route 53 Developer Guide*\. If you use another provider, see the DNS configuration documentation for your provider\.

The name of the TXT record you create should be `_dmarc.example.com`, where `example.com` is your domain\. The value of the TXT record contains the DMARC policy that applies to your domain\. The following is an example of a TXT record that contains a DMARC policy:


| Name | Type | Value | 
| --- | --- | --- | 
| \_dmarc\.example\.com | TXT | "v=DMARC1;p=quarantine;pct=25;rua=mailto:dmarcreports@example\.com" | 

In plain language, this policy tells email providers to do the following: 
+ Look for all emails with a "From" domain of *example\.com* that don't pass SPF or DKIM authentication\.
+ Quarantine 25% of the emails that failed authentication by sending them to the Spam folder \(you can also do nothing by using `p=none`, or reject the messages outright by using `p=reject`\)\. 
+ Send reports about all emails that failed authentication in a digest \(that is, a report that aggregates the data for a certain time period, rather than sending individual reports for each event\)\. Email providers typically send these aggregated reports once per day, although these policies differ from provider to provider\. 

To learn more about configuring DMARC for your domain, see the [Overview](https://dmarc.org/overview/) on the DMARC website\.

For complete specifications of the DMARC system, see [RFC 7489](https://tools.ietf.org/html/rfc7489) on the IETF website\. Section 6\.3 of this document contains a complete list of tags that you can use to configure the DMARC policy for your domain\.

## Complying with DMARC through SPF<a name="send-email-authentication-dmarc-spf"></a>

For an email to comply with DMARC based on SPF, both of the following conditions must be met:
+ The email must pass an SPF check\.
+ The domain in the From address of the email header must align with the MAIL FROM domain that the sending mail server specifies to the receiving mail server\. If the domain's DMARC policy for SPF specifies strict alignment, the From and MAIL FROM domains must match exactly\. If the domain's DMARC policy for SPF specifies relaxed alignment, the MAIL FROM domain can be a subdomain of the domain in the From header\.

To comply with these requirements, complete the following steps:
+ Set up a custom MAIL FROM domain by completing the procedures in [Setting up a custom MAIL FROM domain](mail-from.md)\.
+ Ensure that your sending domain uses a relaxed policy for SPF\. If you have not changed your domain's policy alignment, it will use a relaxed policy by default\.
**Note**  
You can determine your domain's DMARC alignment for SPF by typing the following command at the command line, replacing `example.com` with your domain:  

  ```
  nslookup -type=TXT _dmarc.example.com
  ```
In the output of this command, under **Non\-authoritative answer**, look for a record that begins with `v=DMARC1`\. If this record includes the string `aspf=r`, or if the `aspf` string is not present at all, then your domain uses relaxed alignment for SPF\. If the record includes the string `aspf=s`, then your domain uses strict alignment for SPF\. Your system administrator will need to remove this tag from the DMARC TXT record in your domain's DNS configuration\.  
Alternatively, you can use a web\-based DMARC lookup tool, such as the [DMARC Inspector](https://dmarcian.com/dmarc-inspector/) from the dmarcian website or the [DMARC Check](https://stopemailfraud.proofpoint.com/dmarc/) tool from the Proofpoint website, to determine your domain's policy alignment for SPF\.

## Complying with DMARC through DKIM<a name="send-email-authentication-dmarc-dkim"></a>

For an email to comply with DMARC based on DKIM, both of the following conditions must be met:
+ The message must have a valid DKIM signature\.
+ The From address in the email header must align with the `d=` domain in the DKIM signature\. If the domain's DMARC policy specifies strict alignment for DKIM, these domains must match exactly\. If the domain's DMARC policy specifies relaxed alignment for DKIM, the `d=` domain can be a subdomain of the From domain\.

To comply with these requirements, complete the following steps:
+ Set up Easy DKIM by completing the procedures in [Easy DKIM in Amazon SES](send-email-authentication-dkim-easy.md)\. When you use Easy DKIM, Amazon SES will automatically sign your emails\.
**Note**  
Rather than use Easy DKIM, you can also [manually sign your messages](send-email-authentication-dkim-manual.md)\. However, you must be very careful if you choose to do so, because Amazon SES does not validate the DKIM signature that you construct\. For this reason, we highly recommend using Easy DKIM\.
+ Ensure that your sending domain uses a relaxed policy for DKIM\. If you have not changed your domain's policy alignment, it will use a relaxed policy by default\.
**Note**  
You can determine your domain's DMARC alignment for DKIM by typing the following command at the command line, replacing `example.com` with your domain:  

  ```
  nslookup -type=TXT _dmarc.example.com
  ```
In the output of this command, under **Non\-authoritative answer**, look for a record that begins with `v=DMARC1`\. If this record includes the string `adkim=r`, or if the `adkim` string is not present at all, then your domain uses relaxed alignment for DKIM\. If the record includes the string `adkim=s`, then your domain uses strict alignment for DKIM\. Your system administrator will need to remove this tag from the DMARC TXT record in your domain's DNS configuration\.  
Alternatively, you can use a web\-based DMARC lookup tool, such as the [DMARC Inspector](https://dmarcian.com/dmarc-inspector/) from the dmarcian website or the [DMARC Check](https://stopemailfraud.proofpoint.com/dmarc/) tool from the Proofpoint website, to determine your domain's policy alignment for DKIM\.