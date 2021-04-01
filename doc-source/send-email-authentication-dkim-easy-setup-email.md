# Setting Up Easy DKIM for an Email Address<a name="send-email-authentication-dkim-easy-setup-email"></a>

The procedure in this section shows you how to set up Easy DKIM for a specific email address that you've already verified with Amazon SES\. You can only configure Easy DKIM for email addresses that belong to domains you already own, because you have to change the DNS settings for the domain in order to set up Easy DKIM for an email address\.

**Important**  
You can't set up Easy DKIM for email addresses on domains that you don't own\. For example, you can't set up Easy DKIM for a *gmail\.com* or *hotmail\.com* address\.

If you already set up Easy DKIM for the domain that the email address belongs to, you don't need to set up Easy DKIM for the email address as well\. When you set up Easy DKIM for a domain, Amazon SES automatically authenticates every email from every address on that domain\. Easy DKIM settings for a specific email address automatically override the settings for the domain that the address belongs to\.

**To set up Easy DKIM for an email address**

1. Open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane, under **Identity Management**, choose **Email Addresses**\.

1. In the list of email addresses, choose the address that you want to set up Easy DKIM for\.

1. Under **DKIM**, choose **Generate DKIM Settings**\.

1. Copy the three CNAME records that appear in this section\. Alternatively, you can choose **Download Record Set as CSV** to save a copy of the records to your computer\.

1. Add the CNAME records to the DNS configuration for your domain\. To update the DNS records for your domain:
   + **If you use Route 53 as your DNS provider** – Complete the procedures shown in [Editing Records](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resource-record-sets-editing.html) in the *Amazon Route 53 Developer Guide*\.
   + **If you use another DNS provider** – Different providers have different procedures for updating DNS records\. See the documentation provided by your DNS provider for more information\.
**Note**  
A small number of DNS providers don't allow you to include underscores \(\_\) in record names\. However, the underscore in the DKIM record name is required\. If your DNS provider doesn't allow you to enter an underscore in the record name, contact the provider's customer support team for assistance\.
   + **If you're not sure who your DNS provider is** – Ask your system administrator for more information\.

   Amazon SES usually detects changes to your DNS configuration within 72 hours\.
