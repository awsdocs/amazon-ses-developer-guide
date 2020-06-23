# Verifying a domain with Amazon SES<a name="verify-domain-procedure"></a>

The following procedure shows you how to verify a domain using the Amazon SES console\. If you want to use the Amazon SES API instead, see the [Amazon Simple Email Service API Reference](https://docs.aws.amazon.com/ses/latest/APIReference/)\. 

**Note**  
As an alternative to completing the procedure in this section, you can also enable [Easy DKIM](send-email-authentication-dkim-easy.md)\. When Amazon SES detects that you've added the DKIM records to the DNS configuration for a domain, you can start sending email from that domain, even if you haven't already completed the procedure in this section\.

**To verify a domain**

1. Go to your [ verified domain list](https://console.aws.amazon.com/ses/home?#verified-senders-domain:) in the Amazon SES console, or follow these instructions to navigate to it:

   1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

   1. In the navigation pane, under **Identity Management**, choose **Domains**\.

1. Choose **Verify a New Domain**\.

1. In the **Verify a New Domain** dialog box, enter the domain name\.
**Tip**  
If your domain is *www\.example\.com*, enter *example\.com* as your domain\. The "www\." part isn't necessary, and the domain verification process won't succeed if you include it\.

1. If you want to set up DKIM signing for this domain, choose **Generate DKIM Settings**\. For information about DKIM signing, see [Authenticating Email with DKIM in Amazon SES](send-email-authentication-dkim.md)\.

1. Choose **Verify This Domain**\.

1. In the **Verify a New Domain** dialog box, you will see a **Domain Verification Record Set** containing a **Name**, a **Type**, and a **Value**\. \(This information will also be available by choosing the domain name after you close the dialog box\.\)

   To complete domain verification, add a TXT record with the displayed **Name** and **Value** to your domain's DNS server\. For information about Amazon SES TXT records and general guidance about how to add a TXT record to a DNS server, see [Amazon SES domain verification TXT records](dns-txt-records.md)\. In particular:
   + If your DNS provider does not allow underscores in record names, you can omit *\_amazonses* from the **Name**\.
   + To help you easily identify this record within your domain's DNS settings, you can optionally prefix the **Value** with *amazonses:* 
   + Some DNS providers automatically append the domain name to DNS record names\. To avoid duplication of the domain name, you can add a period to the end of the domain name in the DNS record\. This indicates that the record name is fully qualified and the DNS provider need not append an additional domain name\.  
![\[The Domain verification window. On the window, there's a table that shows the name, type, and value of the TXT record that you need to add to the DNS configuration for your domain.\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/to_complete_verification.png)

1. If Route 53 provides the DNS service for the domain that you're verifying, and you're signed in to the AWS Management Console under the same account that you use for Route 53, then Amazon SES gives you the option of updating your DNS server immediately from within the Amazon SES console\.

   If you use a different DNS provider, the procedures for updating the DNS records vary depending on which DNS or web hosting provider you use\. The following table lists links to the documentation for several common providers\. This list isn't exhaustive and inclusion in this list isn’t an endorsement or recommendation of any company’s products or services\. If your provider isn't listed in the table, you can probably use the domain with Amazon SES\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/verify-domain-procedure.html)

   When verification is complete, the domain's status in the Amazon SES console changes from "pending verification" to "verified," and you receive a notification email from Amazon SES\.

1. You can now use Amazon SES to send email from any address in the verified domain\. To send a test email, check the box next to the verified domain, and then choose **Send a Test Email**\.

If the DNS settings are not correctly updated, you will receive a domain verification failure email from Amazon SES, and the domain will display a status of failed on the **Domains** tab\. If this happens, complete the steps on the troubleshooting page at [Common domain verification problems](troubleshoot-verification.md#troubleshoot-verification-domain)\. After you verify that your TXT was created correctly, choose the **retry** link next to the failed status notification to restart the domain verification process\.