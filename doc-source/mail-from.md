# Using a Custom MAIL FROM Domain with Amazon SES<a name="mail-from"></a>

When an email is sent, it has two addresses that indicate its source: a From address provided in the email header, and a MAIL FROM address that the sending mail server specifies to the receiving server to indicate the source of the message\. The MAIL FROM address is sometimes called the *envelope sender*, *envelope from*, *bounce address*, or *Return Path* address\.

When recipients view an email in their inbox, they see the email's From address\. In contrast, the MAIL FROM address, which is used by mail servers to return bounce messages and other error notifications, is typically only viewable by recipients if they inspect the email's headers in the raw message source\. Amazon SES sets the MAIL FROM domain to a default value unless you choose to use your own\. 

## Why Use a Custom MAIL FROM Domain?<a name="mail-from-overview"></a>

By default, messages that you send through Amazon SES use *amazonses\.com* \(or a subdomain of that\) as the MAIL FROM domain\. Sender Policy Framework \(SPF\) authentication successfully validates these messages because the default MAIL FROM domain matches the sending mail server, Amazon SES\. Although this level of authentication is enough for many senders, you might want to set the MAIL FROM domain to a domain that you own to enable your emails to authenticate with Domain\-based Message Authentication, Reporting and Conformance \(DMARC\) through SPF, which requires an additional check for SPF domain alignment\. DMARC enables a sender's domain to indicate, using a DNS record, that its emails are protected by SPF, DomainKeys Identified Mail \(DKIM\), or both\.

There are two ways to achieve DMARC validation: using SPF and using DKIM\. Unless you use your own MAIL FROM domain, you cannot achieve DMARC validation using SPF because that validation requires the domain in the From address to match the MAIL FROM domain\. By using your own MAIL FROM domain, you have the flexibility to use SPF, DKIM, or both to achieve DMARC validation\. For more information, see [Authenticating Email with SPF](spf.md)\.

## Choosing a MAIL FROM Domain<a name="mail-from-requirements"></a>

If you choose to use your own MAIL FROM domain with Amazon SES, the domain you use must meet the following requirements:
+ The MAIL FROM domain must be a subdomain of the verified identity \(email address or domain\) from which you will send your emails\. For example, *mail\.example\.com* is a valid MAIL FROM domain for the email address *user@example\.com* or the domain *example\.com*\.
+ In most cases, the MAIL FROM domain should not be a domain that you send email from\. If you must use the MAIL FROM domain in a From address, either [disable email feedback forwarding](notifications-via-email.md#notifications-via-email-disabling) and receive your bounces through Amazon SNS notifications, or ensure that your MAIL FROM domain is not the destination for feedback forwarding\. To determine the destination of email forwarding feedback, see [Email Feedback Forwarding Destination](notifications-via-email.md#notifications-via-email-destination)\.
+ The MAIL FROM domain should not be a domain that you use to receive email\.

## Setup Process<a name="mail-from-process"></a>

To set the MAIL FROM domain for a verified identity, you configure the verified identity using the Amazon SES console or API and publish an MX record \(and optionally, an SPF record\) to your MAIL FROM domain's DNS server\. If at any point you want to return to using the default Amazon SES MAIL FROM domain, you can remove your MAIL FROM domain from the verified identity's settings\. These procedures are described in the following sections:
+ [Setting a MAIL FROM Domain](mail-from-set.md)
+ [Removing a MAIL FROM Domain](mail-from-remove.md)
+ [Editing a MAIL FROM Domain](mail-from-edit.md)

For a description of custom MAIL FROM domain setup states, see [MAIL FROM Domain Setup States](mail-from-states.md)\.