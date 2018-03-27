# Setting a MAIL FROM Domain with Amazon SES<a name="mail-from-set"></a>

This topic contains procedures for setting up a custom MAIL FROM domain using the Amazon SES console\.

**Note**  
You can use the same MAIL FROM address in multiple AWS Regions\. For more information, see [Regions and Amazon SES](regions.md)\.

## Setting the MAIL FROM Domain<a name="mail-from-setup-procedure"></a>

The following procedures show how to use the Amazon SES console to configure a verified email address or domain to send emails using a specified MAIL FROM domain\. You can also configure a MAIL FROM domain using the `SetIdentityMailFromDomain` API operation\. For more information, see the [Amazon Simple Email Service API Reference](http://docs.aws.amazon.com/ses/latest/APIReference/)\.

**To configure a verified email address to use a specified MAIL FROM domain**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane, under **Identity Management**, choose **Email Addresses**\.

1. In the list of verified email addresses, confirm that the status of the email address for which you want to set the MAIL FROM domain is **verified**\. If the status is **failure**, choose **retry** and then click the link within the verification email you receive in your email client\. Otherwise, choose the email address and proceed to the next step\.

1. In the details pane of the verified email address, expand **MAIL FROM Domain**\.

1. Choose **Set MAIL FROM Domain**\.

1. In the **Set MAIL FROM Domain** dialog box, type the name of the MAIL FROM domain that you want to use\. Note that this must be a subdomain of the domain of the verified email address, such as *mail\.example\.com*\.

1. For **Behavior if MX record not found**, choose one of the following options:
   + **Use *region*\.amazonses\.com as MAIL FROM** – If the custom MAIL FROM domain's MX record is not set up correctly, Amazon SES will use a subdomain of *amazonses\.com*\. The subdomain varies based on the AWS Region in which you use Amazon SES\.
   + **Reject message** – If the custom MAIL FROM domain's MX record is not set up correctly, Amazon SES will return a `MailFromDomainNotVerified` error\. Emails that you attempt to send from this address will be automatically rejected\.

1. Choose **Set MAIL FROM Domain**\. A window appears that contains the MX and SPF records that you must add to your domain's DNS configuration\. Note these values, and then proceed to the next step\.

1. Publish the MX record to the DNS server of the custom MAIL FROM domain\.
**Important**  
If the DNS configuration for the MAIL FROM domain contains multiple MX records, the custom MAIL FROM setup with Amazon SES will fail\.

   1. If Route 53 provides the DNS service for your MAIL FROM domain, and you are signed in to the AWS Management Console under the same account that you use for Route 53, then choose ** Publish Records Using Route 53**\. The appropriate MX record is automatically applied to your domain's DNS configuration\.

   1. If your MAIL FROM domain does not use Route 53, then you must publish the displayed MX record to the MAIL FROM domain's DNS server yourself\. The procedure for adding an MX record to your domain's DNS server varies based on your web hosting service or DNS provider; see your provider's documentation for more information\. 

      After Amazon SES detects the record, emails you send from this verified email address will use the specified MAIL FROM domain\. Until then, Amazon SES will either use the default MAIL FROM domain or reject the message, depending on the preferences you specified earlier in this procedure\. Amazon SES can take up to 72 hours to detect your MX record\.

1. Publish an SPF record to your MAIL FROM domain's DNS server to show receiving mail servers that you have authorized Amazon SES to send email on behalf of your domain\. For more information, see [Authenticating Email with SPF in Amazon SES](spf.md)\.

**To configure a verified domain to use a specified MAIL FROM domain**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane, under **Identity Management**, choose **Domains**\.

1. In the verified domain list, confirm that the status of the domain for which you want to set the MAIL FROM domain is **verified**\. If the status is **failure**, choose **retry** and then add the displayed TXT record to your DNS server, as described in [Amazon SES Domain Verification TXT Records](dns-txt-records.md)\. Otherwise, choose the domain and continue this procedure\.

1. In the details pane of the verified domain, expand **MAIL FROM Domain**\.

1. Choose **Set MAIL FROM Domain**\.

1. In the **Set MAIL FROM Domain** dialog box, type the name of the MAIL FROM domain that you want to use\. Note that this must be a subdomain of the verified domain, such as *mail\.example\.com*\.

1. For **Behavior if MX record not found**, choose one of the following options:
   + **Use *region*\.amazonses\.com as MAIL FROM** – If the custom MAIL FROM domain's MX record is not set up correctly, Amazon SES will use a subdomain of *amazonses\.com*\. The subdomain varies based on the AWS Region in which you use Amazon SES\.
   + **Reject message** – If the custom MAIL FROM domain's MX record is not set up correctly, Amazon SES will return a `MailFromDomainNotVerified` error\. Emails that you attempt to send from this address will be automatically rejected\.

1. Choose **Set MAIL FROM Domain**\.

1. Next, publish an MX record to the DNS server of the custom MAIL FROM domain\.
**Important**  
To successfully set up a custom MAIL FROM domain with Amazon SES, you must publish exactly one MX record to the DNS server of your MAIL FROM domain\. If the MAIL FROM domain has multiple MX records, the custom MAIL FROM setup with Amazon SES will fail\.

   1. If Route 53 provides the DNS service for your MAIL FROM domain, and you are signed in to the AWS Management Console under the same account that you use for Route 53, then choose ** Publish Records Using Route 53**\. The appropriate MX record is automatically applied to your domain's DNS configuration\.

   1. If your MAIL FROM domain does not use Route 53, then you must publish the displayed MX record to the MAIL FROM domain's DNS server yourself\. The procedure for adding an MX record to your domain's DNS server varies based on your web hosting service or DNS provider; see your provider's documentation for more information\. 

      After Amazon SES detects the record, emails you send from this verified email address will use the specified MAIL FROM domain\. Until then, Amazon SES will either use the default MAIL FROM domain or reject the message, depending on the preferences you specified earlier in this procedure\. Amazon SES can take up to 72 hours to detect your MX record\.

1. Publish an SPF record to your MAIL FROM domain's DNS server to show receiving mail servers that you have authorized Amazon SES to send email on behalf of your domain\. For more information, see [Authenticating Email with SPF in Amazon SES](spf.md)\.