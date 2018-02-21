# Editing a MAIL FROM Domain with Amazon SES<a name="mail-from-edit"></a>

The following procedures show how to use the Amazon SES console to edit the custom MAIL FROM domain configuration of a verified email address or domain\. If you want to use the Amazon SES API instead, see the `SetIdentityMailFromDomain` API in the [Amazon Simple Email Service API Reference](http://docs.aws.amazon.com/ses/latest/APIReference/)\.

**To edit the MAIL FROM configuration of a verified email address**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane, under **Identity Management**, choose **Domains**\.

1. In the verified email address list, choose the email address for which you want to configure the MAIL FROM domain\.

1. In the details pane of the verified email address, expand **MAIL FROM Domain**\.

1. Choose **Edit MAIL FROM Domain**\.

1. In the **Edit MAIL FROM Domain** dialog box, edit the settings and then choose **Save MAIL FROM Domain**\.

1. If you changed the MAIL FROM domain name when you edited the settings, you must publish an MX record to the DNS server of the new MAIL FROM domain\.

   1. If Route 53 provides the DNS service for your MAIL FROM domain, and you are signed in to the AWS Management Console under the same account that you use for Route 53, then choose ** Publish Records Using Route 53**\. The appropriate MX record is automatically applied to your domain's DNS configuration\.

   1. If your MAIL FROM domain does not use Route 53, then you must publish the displayed MX record to the MAIL FROM domain's DNS server yourself\. The procedure for adding an MX record to your domain's DNS server varies based on your web hosting service or DNS provider; see your provider's documentation for more information\. 

      After Amazon SES detects the record, emails you send from this verified email address will use the specified MAIL FROM domain\. Until then, Amazon SES will either use the default MAIL FROM domain or reject the message, depending on the preferences you specified earlier in this procedure\. Amazon SES can take up to 72 hours to detect your MX record\.

1. \(Optional\) If you changed the MAIL FROM domain name and you want Sender Policy Framework \(SPF\) checks to succeed, you must publish an SPF record to your MAIL FROM domain's DNS server to show receiving mail servers that you have authorized Amazon SES to send email on behalf of your domain\. For more information, see [Authenticating Email with SPF in Amazon SES](spf.md)\.

**To edit the MAIL FROM configuration of a verified domain**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane, under **Identity Management**, choose **Domains**\.

1. In the verified domain list, choose the domain for which you want to configure the MAIL FROM domain\.

1. In the details pane of the verified domain, expand **MAIL FROM Domain**\.

1. Choose **Edit MAIL FROM Domain**\.

1. In the **Edit MAIL FROM Domain** dialog box, edit the settings and then choose **Save MAIL FROM Domain**\.

1. If you changed the MAIL FROM domain name when you edited the settings, you must publish an MX record to the DNS server of the new MAIL FROM domain\.

   1. If Route 53 provides the DNS service for your MAIL FROM domain, and you are signed in to the AWS Management Console under the same account that you use for Route 53, then choose **Publish Records Using Route 53** if you want to publish the MX record and/or SPF record from within the Amazon SES console\. 

   1. If your domain does not use Route 53, then you must publish the displayed MX record to the MAIL FROM domain's DNS server yourself\. The procedure for adding an MX record to your domain's DNS server depends on who provides your DNS service; please see the documentation for your DNS service\. After Amazon SES detects the record, emails you send from this verified domain will use the specified MAIL FROM domain\. Until then, Amazon SES will either use the default MAIL FROM domain or reject the message, depending on the preferences you specified earlier in this procedure\. Amazon SES can take up to 72 hours to detect your MX record\.

1. \(Optional\) If you changed the MAIL FROM domain name and you want Sender Policy Framework \(SPF\) checks to succeed, you must publish an SPF record to your MAIL FROM domain's DNS server to show receiving mail servers that you have authorized Amazon SES to send email on behalf of your domain\. For more information, see [Authenticating Email with SPF in Amazon SES](spf.md)\.