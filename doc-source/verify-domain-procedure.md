# Verifying a Domain With Amazon SES<a name="verify-domain-procedure"></a>

The following procedure shows you how to verify a domain using the Amazon SES console\. If you want to use the Amazon SES API instead, see the [Amazon Simple Email Service API Reference](http://docs.aws.amazon.com/ses/latest/APIReference/)\. 

**To verify a domain**

1. Go to your [ verified domain list](https://console.aws.amazon.com/ses/home?#verified-senders-domain:) in the Amazon SES console, or follow these instructions to navigate to it:

   1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

   1. In the navigation pane, under **Identity Management**, choose **Domains**\.

1. Choose **Verify a New Domain**\.

1. In the **Verify a New Domain** dialog box, enter the domain name\. If you want to set up DKIM signing for this domain, select the **Generate DKIM Settings** option\. \(For information about DKIM signing, see [Authenticating Email with DKIM in Amazon SES](dkim.md)\.\) Choose **Verify This Domain**\.

1. In the **Verify a New Domain** dialog box, you will see a **Domain Verification Record Set** containing a **Name**, a **Type**, and a **Value**\. \(This information will also be available by choosing the domain name after you close the dialog box\.\)

   To complete domain verification, add a TXT record with the displayed **Name** and **Value** to your domain's DNS server\. For information about Amazon SES TXT records and general guidance about how to add a TXT record to a DNS server, see [Amazon SES Domain Verification TXT Records](dns-txt-records.md)\. In particular:
   + If your DNS provider does not allow underscores in record names, you can omit *\_amazonses* from the **Name**\.
   + To help you easily identify this record within your domain's DNS settings, you can optionally prefix the **Value** with *amazonses:* 
   + Some DNS providers automatically append the domain name to DNS record names\. To avoid duplication of the domain name, you can add a period to the end of the domain name in the DNS record\. This indicates that the record name is fully qualified and the DNS provider need not append an additional domain name\.  
![\[to complete verification\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/to_complete_verification.png)

1. If Route 53 provides the DNS service for the domain that you are verifying, and you are signed in to the AWS Management Console under the same account that you use for Route 53, then Amazon SES will give you the option of updating your DNS server immediately from within the Amazon SES console\. If you are not using Route 53, Amazon SES needs to verify that a TXT record with the specified **Name** and **Value** have been added to your domain's DNS server\. This may take up to 72 hours\.

   When verification is complete, the domain's status in the Amazon SES console will change from "pending verification" to "verified," and you will receive a confirmation success email from Amazon SES to the email address associated with your AWS account\.

1. You can now use Amazon SES to send email from any address in the verified domain\. To send a test email, check the box next to the verified domain, and then choose **Send a Test Email**\.

If the DNS settings are not correctly updated, you will receive a domain verification failure email from Amazon SES, and the domain will display a status of "failed" in the **Domains** tab\. If this happens, read our troubleshooting page at [Amazon SES Email Address and Domain Verification Problems](domain-verification-problems.md)\. When you have verified that your TXT record is correctly in place, choose the "retry" link next to the "failed" status notification\. This will reinitiate the domain verification process\.