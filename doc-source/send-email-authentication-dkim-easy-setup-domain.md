# Setting Up Easy DKIM for a Domain<a name="send-email-authentication-dkim-easy-setup-domain"></a>

The procedure in this section shows you how to set up Easy DKIM for a domain\. If you setup Easy DKIM for a domain, then you can start sending email from that domain, even if you haven't completed the [procedure to verify a domain](verify-domains.md)\.

**To set up Easy DKIM for a domain**

1. Open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane, under **Identity Management**, choose **Domains**\.

1. In the list of domains, choose the domain that you want to set up Easy DKIM for\.
**Note**  
If you haven't started the verification process for the domain yet, see the procedures at [Verifying a domain with Amazon SES](verify-domain-procedure.md)\.

1. Under **DKIM**, choose **Generate DKIM Settings**\.

1. Copy the three CNAME records that appear in this section\. Alternatively, you can choose **Download Record Set as CSV** to save a copy of the records to your computer\. 

   The following image shows an example of the **DKIM** section\.  
![\[\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/dkim_existing_dns.png)

1. Add the CNAME records to the DNS configuration for your domain\. To update the DNS records for your domain:
   + **If you use Route 53 as your DNS provider** – If you use Route 53 on the same account that you use when you send email using Amazon SES, choose **Use Route 53** to automatically update the DNS settings for your domain\. Otherwise, complete the procedures shown in [Editing Records](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resource-record-sets-editing.html) in the *Amazon Route 53 Developer Guide*\.
   + **If you use another DNS provider** – Different providers have different procedures for updating DNS records\. The following table lists links to the documentation for several common providers\. This list isn't exhaustive and inclusion in this list isn’t an endorsement or recommendation of any company’s products or services\. If your provider isn't listed in the table, you can probably use the domain with Amazon SES\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/send-email-authentication-dkim-easy-setup-domain.html)
**Note**  
A small number of DNS providers don't allow you to include underscores \(\_\) in record names\. However, the underscore in the DKIM record name is required\. If your DNS provider doesn't allow you to enter an underscore in the record name, contact the provider's customer support team for assistance\.
   + **If you're not sure who your DNS provider is** – Ask your system administrator for more information\.

   Amazon SES usually detects changes to your DNS configuration within 72 hours\.
