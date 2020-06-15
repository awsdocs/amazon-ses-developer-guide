# Setting up a custom MAIL FROM domain<a name="mail-from"></a>

When an email is sent, it has two addresses that indicate its source: a From address that's displayed to the message recipient, and a MAIL FROM address that indicates where the message originated\. The MAIL FROM address is sometimes called the *envelope sender*, *envelope from*, *bounce address*, or *Return Path* address\. Mail servers use the MAIL FROM address to return bounce messages and other error notifications\. The MAIL FROM address is usually only viewable by recipients if they view the source code for the message\.

 Amazon SES sets the MAIL FROM domain for the messages that you send to a default value unless you specify your own domain\. This section discusses the benefits of setting up a custom MAIL FROM domain, and includes setup procedures\.

## Why use a custom MAIL FROM domain?<a name="mail-from-overview"></a>

By default, messages that you send through Amazon SES use a subdomain of *amazonses\.com* as the MAIL FROM domain\. Sender Policy Framework \(SPF\) authentication successfully validates these messages because the default MAIL FROM domain matches the application that sent the email— in this case, Amazon SES\.

While this level of authentication is sufficient for many senders, other senders prefer to set the MAIL FROM domain to a domain that they own\. By setting up a custom MAIL FROM domain, your emails can comply with [Domain\-based Message Authentication, Reporting and Conformance \(DMARC\)](send-email-authentication-dmarc.md)\. DMARC enables a sender's domain to indicate that emails sent from the domain are protected by one or more authentication systems\.

There are two ways to achieve DMARC validation: using [Sender Policy Framework](send-email-authentication-spf.md) \(SPF\), and using [DomainKeys Identified Mail](send-email-authentication-dkim.md) \(DKIM\)\. The only way to comply with DMARC through SPF is to use a custom MAIL FROM domain, because SPF validation requires the domain in the From address to match the MAIL FROM domain\. By using your own MAIL FROM domain, you have the flexibility to use SPF, DKIM, or both to achieve DMARC validation\.

## Choosing a MAIL FROM domain<a name="mail-from-requirements"></a>

The subdomain you use for your MAIL FROM domain has to meet the following requirements:
+ The MAIL FROM domain has to be a subdomain of the verified identity \(email address or domain\) that you send email from\. For example, *mail\.example\.com* is a valid MAIL FROM domain for the domain *example\.com*\.
+ The MAIL FROM domain shouldn't be a domain that you send email from\. If you have to use the MAIL FROM domain in a From address, either [disable email feedback forwarding](monitor-sending-activity-using-notifications-email.md#monitor-sending-activity-using-notifications-email-disabling) and receive your bounces through Amazon SNS notifications, or ensure that your MAIL FROM domain is not the destination for feedback forwarding\. To determine the destination of email forwarding feedback, see [Email Feedback Forwarding Destination](monitor-sending-activity-using-notifications-email.md#monitor-sending-activity-using-notifications-email-destination)\.
+ The MAIL FROM domain shouldn't be a domain that you use to receive email\.

## Configuring the MAIL FROM domain<a name="mail-from-set"></a>

The process of setting up a custom MAIL FROM domain requires you to add records to the DNS configuration for the domain\. You have to publish an MX record so that your domain can receive the bounce and complaint notifications that email providers send you\. You also have to publish an SPF record in order to prove that Amazon SES is authorized to send email from your domain\.

You can set up a custom MAIL FROM domain for an entire domain, or for individual email addresses\. The following procedures show how to use the Amazon SES console to configure a custom MAIL FROM domain\. You can also configure a custom MAIL FROM domain using the [SetIdentityMailFromDomain](https://docs.aws.amazon.com/ses/latest/APIReference/API_SetIdentityMailFromDomain.html) API operation\.

### Setting Up a MAIL FROM Domain for a Verified Domain<a name="mail-from-setup-procedure-domain"></a>

You can configure a MAIL FROM domain for an entire domain\. When you do, all of the messages that you send from addresses on that domain use the same MAIL FROM domain\.

**To configure a verified domain to use a specified MAIL FROM domain**

1. Open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane, under **Identity Management**, choose **Domains**\.

1. In the list of domains, confirm that the parent domain of the MAIL FROM domain is verified\. If the domain isn't verified, complete the procedures at [Verifying domains in Amazon SES](verify-domains.md) to verify the domain\. Otherwise, choose the domain and proceed to the next step\.

1. Under **MAIL FROM Domain**, choose **Set MAIL FROM Domain**\.

1. On the **Set MAIL FROM Domain** window, do the following:

   1. For **MAIL FROM domain**, enter the subdomain that you want to use as the MAIL FROM domain\.

   1. For **Behavior if MX record not found**, choose one of the following options:
      + **Use *region*\.amazonses\.com as MAIL FROM** – If the custom MAIL FROM domain's MX record is not set up correctly, Amazon SES will use a subdomain of *amazonses\.com*\. The subdomain varies based on the AWS Region in which you use Amazon SES\.
      + **Reject message** – If the custom MAIL FROM domain's MX record is not set up correctly, Amazon SES will return a `MailFromDomainNotVerified` error\. Emails that you attempt to send from this domain will be automatically rejected\.

   1. Choose **Set MAIL FROM Domain**\. A window appears that contains the MX and SPF records that you have to add to your domain's DNS configuration\. These records use the formats shown in the following table\.   
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/mail-from.html)

      In the preceding records, replace *subdomain*\.*domain*\.*com* with your MAIL FROM subdomain, and replace *region* with the name of the AWS Region where you want to verify the MAIL FROM domain \(such as `us-west-2`, `us-east-1`, or `eu-west-1`\)\. Note that the value of the TXT record has to include the quotation marks\.

      Note these values, and then proceed to the next step\.

1. Publish an MX record to the DNS server of the custom MAIL FROM domain\.
**Important**  
To successfully set up a custom MAIL FROM domain with Amazon SES, you must publish exactly one MX record to the DNS server of your MAIL FROM domain\. If the MAIL FROM domain has multiple MX records, the custom MAIL FROM setup with Amazon SES will fail\.

   If Route 53 provides the DNS service for your MAIL FROM domain, and you are signed in to the AWS Management Console under the same account that you use for Route 53, then choose ** Publish Records Using Route 53**\. The DNS records are automatically applied to your domain's DNS configuration\.

   If you use a different DNS provider, you have to publish the DNS records to the MAIL FROM domain's DNS server manually\. The procedure for adding DNS records to your domain's DNS server varies based on your web hosting service or DNS provider\. 

   The procedures for publishing DNS records for your domain depend on which DNS provider you use\. The following table includes links to the documentation for several common DNS providers\. This list isn't a complete list of providers\. If your provider isn't listed below, you can probably still set up a MAIL FROM domain\. Inclusion on this list isn’t an endorsement or recommendation of any company’s products or services\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/mail-from.html)

   When Amazon SES detects that the records are in place, you receive an email informing you that your custom MAIL FROM domain was set up successfully\. Depending on your DNS provider, there might be a delay of up to 72 hours before Amazon SES detects the MX record\.

### Setting Up a MAIL FROM Domain for a Verified Email Address<a name="mail-from-setup-procedure-email-address"></a>

You can also set up a custom MAIL FROM domain for a specific email address\. In order to set up a custom MAIL FROM domain for an email address, you have to be able to modify the DNS records for the domain that the email address is associated with\.

**Note**  
You can't set up a custom MAIL FROM domain for addresses on a domain that you don't own \(for example, you can't create a custom MAIL FROM domain for an address on the *gmail\.com* domain, because you can't add the necessary DNS records to the domain\)\.

**To configure a verified email address to use a specified MAIL FROM domain**

1. Open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane, under **Identity Management**, choose **Email Addresses**\.

1. In the list of email addresses, confirm that the email address that you want to set up a custom MAIL FROM domain for is verified\. If the email address isn't verified, complete the procedures at [Verifying email addresses in Amazon SES](verify-email-addresses.md) to verify the email address\. Otherwise, choose the email address and proceed to the next step\.

1. Under **MAIL FROM Domain**, choose **Set MAIL FROM Domain**\.

1. On the **Set MAIL FROM Domain** window, do the following:

   1. For **MAIL FROM domain**, enter the subdomain that you want to use as the MAIL FROM domain\.

   1. For **Behavior if MX record not found**, choose one of the following options:
      + **Use *region*\.amazonses\.com as MAIL FROM** – If the custom MAIL FROM domain's MX record is not set up correctly, Amazon SES will use a subdomain of *amazonses\.com*\. The subdomain varies based on the AWS Region that you use Amazon SES in\.
      + **Reject message** – If the custom MAIL FROM domain's MX record is not set up correctly, Amazon SES will return a `MailFromDomainNotVerified` error\. Emails that you attempt to send from this email address will be automatically rejected\.

   1. Choose **Set MAIL FROM Domain**\. A window appears that contains the MX and SPF records that you have to add to the DNS configuration for the domain that the email address belongs to\. These records use the formats shown in the following table\.   
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/mail-from.html)

      In the preceding records, replace *subdomain*\.*domain*\.*com* with your MAIL FROM subdomain, and replace *region* with the name of the AWS Region where you want to verify the MAIL FROM domain \(such as `us-west-2`, `us-east-1`, or `eu-west-1`\)\. Note that the value of the TXT record has to include the quotation marks\.

      Note these values, and then proceed to the next step\.

1. Publish the DNS records to the DNS server of the custom MAIL FROM domain\.
**Important**  
To successfully set up a custom MAIL FROM domain with Amazon SES, you must publish exactly one MX record to the DNS server of your MAIL FROM domain\. If the MAIL FROM domain has multiple MX records, the custom MAIL FROM setup with Amazon SES will fail\.

   If Route 53 provides the DNS service for your MAIL FROM domain, and you are signed in to the AWS Management Console under the same account that you use for Route 53, then choose ** Publish Records Using Route 53**\. The DNS records are automatically applied to your domain's DNS configuration\.

   If you use a different DNS provider, you have to publish the DNS records to the MAIL FROM domain's DNS server manually\. The procedure for adding DNS records to your domain's DNS server varies based on your web hosting service or DNS provider\. 

   The procedures for publishing DNS records for your domain depend on which DNS provider you use\. The following table includes links to the documentation for several common DNS providers\. This list isn't a complete list of providers\. If your provider isn't listed below, you can probably still set up a MAIL FROM domain\. Inclusion on this list isn’t an endorsement or recommendation of any company’s products or services\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/mail-from.html)

   When Amazon SES detects that the records are in place, you receive an email informing you that your custom MAIL FROM domain was set up successfully\. Depending on your DNS provider, there might be a delay of up to 72 hours before Amazon SES detects the MX record\.

## MAIL FROM domain setup states with Amazon SES<a name="mail-from-states"></a>

After you configure an identity to use a custom MAIL FROM domain, the state of the setup is "pending" while Amazon SES attempts to detect the required MX record in your DNS settings\. The state then changes depending on whether Amazon SES detects the MX record\. The following table describes the email\-sending behavior, and the Amazon SES actions associated with each state\. Each time the state changes, Amazon SES sends a notification to the email address associated with your AWS account\.


****  

| State | Email Sending Behavior | Amazon SES Actions | 
| --- | --- | --- | 
|  Pending  |  Uses custom MAIL FROM fallback setting  |  Amazon SES attempts to detect the required MX record for 72 hours\. If unsuccessful, the state changes to "Failed"\.  | 
|  Success  |  Uses custom MAIL FROM domain  |  Amazon SES continuously checks that the required MX record is in place\.   | 
|  TemporaryFailure  |  Uses custom MAIL FROM fallback setting  |  Amazon SES attempts to detect the required MX record for 72 hours\. If unsuccessful, the state changes to "Failed"; if successful, the state changes to "Success"\.  | 
|  Failed  |  Uses custom MAIL FROM fallback setting  |  Amazon SES no longer attempts to detect the required MX record\. To use a custom MAIL FROM domain, you have to restart the setup process in [](#mail-from-set)\.   | 