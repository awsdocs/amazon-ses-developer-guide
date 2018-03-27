# Easy DKIM in Amazon SES<a name="easy-dkim"></a>

*Easy DKIM* is a feature of Amazon SES that signs every message that you send from a verified email address or domain with a DKIM signature that uses a 1024\-bit DKIM key\. You can use the Amazon SES console to configure Easy DKIM settings, and to enable or disable automatic DKIM signing for your email messages\. You must be able to edit your domain's DNS records to set up Easy DKIM\. With the appropriate DNS records in place, you can enable Easy DKIM signing for any verified email address or domain\. 

The following rules apply:
+ As with other verified identity settings, if you verify a domain, subdomain, and email address that share a root domain, Easy DKIM settings apply at the most granular level you verified\. That is:
  + Verified email address settings override verified domain settings\.
  + Verified subdomain settings override verified domain settings, with lower\-level subdomain settings overriding higher\-level subdomain settings\.
+ If you verify a root domain, and do not verify a particular subdomain, then that subdomain uses the root domain's Easy DKIM settings\. That is, if the root domain has Easy DKIM set up and enabled, the subdomain's emails are automatically DKIM\-signed as well\.

Once you set up Easy DKIM, your messages will automatically be DKIM\-signed regardless of whether you call Amazon SES through the SMTP interface or the API \(`SendEmail` or `SendRawEmail`\)\. Note that you only need to set up Easy DKIM for the domain you use in your From address, not for the domain in a "Return\-Path" or "Reply\-To" address\.

If you are verifying a new domain, you can set up Easy DKIM at that time\. If you already have a verified domain or email address, you can add Easy DKIM capability to it whenever you want\.

**Note**  
Amazon SES has endpoints in multiple AWS regions, and Easy DKIM setup applies to each AWS region separately\. You must perform the Easy DKIM setup procedure for each region in which you want to use Easy DKIM\. For information about using Amazon SES in multiple AWS regions, see [Regions and Amazon SES](regions.md)\.

This topic contains the following sections:
+ To set up Easy DKIM while you verify a new domain, see [Setting Up Easy DKIM for a New Domain](#easy-dkim-new-domain)\.
+ To set up Easy DKIM for an email address or domain that you have already verified, see [Setting Up Easy DKIM for an Existing Verified Identity](#easy-dkim-existing-domain)\.

## Setting Up Easy DKIM for a New Domain<a name="easy-dkim-new-domain"></a>

When you use the AWS Management Console to verify a new domain, you can also set up Easy DKIM at the same time\. 

These instructions are for new domains only\. If you want to set up Easy DKIM for an email address or domain that you have already verified, see [Setting Up Easy DKIM for an Existing Verified Identity](#easy-dkim-existing-domain)\.

**To set up Easy DKIM for a new domain**

1. Go to your [verified domain list](https://console.aws.amazon.com/ses/home?#verified-senders-domain:) in the Amazon SES console, or follow these instructions to navigate to it:

   1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

   1. In the navigation pane, under **Identities**, click **Domains**\.

1. Click **Verify a New Domain**\.

1. In the **Verify a New Domain** dialog box, enter your domain name, select the **Generate DKIM settings** check box, and then click **Verify This Domain**\.  
![\[Verify a New Domain and Set Up Easy DKIM\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/dkim_new_verify.png)

   In the resulting dialog box, you will see all of the DNS records that you need for setting up domain verification and Easy DKIM\. This information will also be available by clicking the domain name after you close the dialog box\.  
![\[DNS Records For Easy DKIM\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/dkim_new_dns.png)

1. To complete domain verification, you must update your domain's DNS settings with the TXT record information from the **Domain Verification Record** in the **Verify a New Domain** dialog box\. Note that some domain name providers use the term **Host** instead of **Name**\. If your DNS provider does not allow underscores in record names, you can omit *\_amazonses* from the **Name** of the domain verification record\. To help you easily identify this record within your domain's DNS settings, you can optionally prefix the **Value** with *amazonses:* 

   Highlight and copy individual records, or select **Download Record Set as CSV** to download all of the records\. 
**Important**  
DNS providers may append the domain name to the end of DNS records\. Adding a record that already contains the domain name \(such as *\_amazonses\.example\.com*\) may result in the duplication of the domain name \(such as *\_amazonses\.example\.com\.example\.com*\)\. To avoid duplication of the domain name, add a period to the end of the domain name in the DNS record\. This will indicate to your DNS provider that the record name is fully qualified \(that is, no longer relative to the domain name\), and prevent the DNS provider from appending an additional domain name\.

1. To set up DKIM, you must update your domain's DNS settings with the CNAME record information from the dialog box\. Note that you cannot omit the underscore from *\_domainkey* because the underscore is required by [RFC 4871](https://www.ietf.org/rfc/rfc4871.txt)\.

   Highlight and copy individual CNAME records, or select **Download Record Set as CSV** to download all of the records\.

   1. If Route 53 provides the DNS service for the domain you are verifying, and you are logged in to Amazon SES console with the same email address and password you use for Route 53, then you will have the option of immediately updating your DNS settings for both domain verification and DKIM from within the Amazon SES console\.

   1. If you are not using Route 53, you will need to update your DNS settings according to the procedure established by your DNS service provider\. \(Ask your system administrator if you are not sure who provides your DNS service\.\) Amazon Web Services will eventually detect that you have updated your DNS records; this detection process may take up to 72 hours\.

   When verification is complete, the domain's **Status** in the Amazon SES console will change from *pending verification* to *verified*, and you will receive an *Amazon SES Domain Verification SUCCESS* confirmation email from Amazon Web Services\. \(AWS emails are sent to the email address you used when you signed up for Amazon SES\.\)

   When Amazon SES has successfully detected the changes to your DNS records, the **DKIM Verification Status** for that domain in the Amazon SES console will change from *in progress* to *success*, and you will receive an *Amazon SES DKIM Setup Successful* confirmation email from Amazon Web Services\.

1. You can now use Amazon SES to send email that is signed using a DKIM signature from any valid address in the verified domain\. To send a test email using the Amazon SES console, check the box next to the verified domain, and then click **Send a Test Email**\. View the email headers in the email you receive\. Email providers typically provide this capability through an option such as **Show original** or **View message source**\. Look for a header named *DKIM\-Signature* with the *"d"* tag set to your domain\. Note that when DKIM is enabled, there will be two *DKIM\-Signature* headers added to the message: one header for your domain, and one header with *d=amazonses\.com*\. \(Amazon SES adds a signature for *amazonses\.com* automatically whether you have DKIM enabled or not\. You can ignore it\.\) For example, for a domain called *ses\-example\.com*, the DKIM signature header you are looking for might look like:

   ```
   DKIM-Signature: v=1; a=rsa-sha256; q=dns/txt; c=relaxed/simple;
   s=xtk53kxcy4p3t6ztbrffs6d54rsrrhh6; d=ses-example.com;
   t=1366720445;
   h=From:To:Subject:MIME-Version:Content-Type:Content-Transfer-Encoding:Date:Message-ID;
   bh=lcj/Sl5qKl6K6zwFUwb7Flgnngl892pW574kmS1hrS0=;
   b=nhVMQLmSh7/DM5PW7xPV4K/PN4iVY0a5OF4YYk2L7jgUq9hHQlckopxe82TaAr64
   eVTcBhHHj9Bwtzkmuk88g4G5UUN8J+AAsd/JUNGoZOBS1OofSkuAQ6cGfRGanF68Ag7
   nmmEjEi+JL5JQh//u+EKTH4TVb4zdEWlBuMlrdTg=
   ```

**Important**  
How you update the DNS settings depends on who provides your DNS service\. DNS service may be provided by a domain name registrar such as GoDaddy or Network Solutions, or by a separate service such as Amazon Route 53\.

**What if Easy DKIM fails?**

If your DNS settings are not correctly updated, you will first receive an *Amazon SES DKIM FAILURE* email from Amazon Web Services, and you will see a status of *failed* in the Domains area when you click on the DKIM tab\.

**Note**  
If this happens, Amazon SES will still send your email, but *it will not be signed using a DKIM signature*\.

## Setting Up Easy DKIM for an Existing Verified Identity<a name="easy-dkim-existing-domain"></a>

If you have already verified a domain or email address, you can use the AWS Management Console to set up Easy DKIM for that identity at any time\.

These instructions are for adding DKIM signing to a domain that has already been verified\. If you are verifying a new domain and want to set up Easy DKIM at the same time, see [Setting Up Easy DKIM for a New Domain](#easy-dkim-new-domain)\.

**Important**  
Easy DKIM only works with fully qualified domain names \(FQDNs\)\. If you wanted to set up Easy DKIM for both *example\.com* and *newyork\.example\.com*, you would need to set up Easy DKIM for each of these FQDNs separately\.

**To set up Easy DKIM for an existing verified domain**

1. Go to your [verified domain list](https://console.aws.amazon.com/ses/home?#verified-senders-domain:) in the Amazon SES console, or follow these instructions to navigate to it:

   1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

   1. In the navigation pane, under **Identities**, click **Domains**\.

1. In the content pane, click the verified domain for which you would like to set up Easy DKIM\.

1. On the domain's Details page, expand **DKIM**\.  
![\[Generate DKIM Settings\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/dkim_existing_verify.png)

1. Click **Generate DKIM Settings**\.

   Your DKIM records will be displayed\.  
![\[\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/dkim_existing_dns.png)

1. To set up DKIM, you must update your domain's DNS settings with the displayed CNAME record information\. You can copy the records or click the **Download Record Set as CSV** link\.

   1. If Route 53 provides the DNS service for the domain you are verifying, and you are logged in to Amazon SES console with the same email address and password you use for Route 53, then Amazon SES will give you the option of immediately updating your DNS settings for Easy DKIM\. If you would like to do this, click the **Use Route 53** button\.

      Next, click **Create Record Sets** in the **Use Route 53** dialog box to complete the process\.

   1.  If you are not using Route 53, you will need to update your DNS settings according to the procedure established by your DNS service provider\. \(Ask your system administrator if you are not sure who provides your DNS service\.\) Amazon Web Services will eventually detect that you have updated your DNS records; this detection process may take up to 72 hours\.

1. When Amazon SES has successfully detected the changes to your DNS records, the **DKIM Verification Status** for that domain in the Amazon SES console will change from *in progress* to *success*, and you will receive an Amazon SES DKIM Setup Successful confirmation email from Amazon Web Services\. \(Amazon Web Services emails are sent to the email address you used when you signed up for Amazon SES\.\)

1. *\(This step is only required if DKIM setup was initiated before 09\-13\-16, 2:00 PDT\)* To sign your messages using a DKIM signature, you must enable Easy DKIM for the appropriate verified sending identity as follows:

   1. In the navigation pane, under **Identities**, click either **Email Addresses** or **Domains**, depending whether you want to enable Easy DKIM signing for an email address or a domain\.

   1. Click the email address or domain for which you wish to enable Easy DKIM signing\.

   1. On the Details page of the email address or domain, expand **DKIM**\.

   1. In the **DKIM:** field, click **enable**\.

1. You can now use Amazon SES to send email that is signed using a DKIM signature from any valid address in the verified domain\. To send a test email using the Amazon SES console, check the box next to the verified domain, and then click **Send a Test Email**\. View the email headers in the email you receive\. Email providers typically provide this capability through an option such as **Show original** or **View message source**\. Look for a header named *DKIM\-Signature* with the *"d"* tag set to your domain\. Note that when DKIM is enabled, there will be two *DKIM\-Signature* headers added to the message: one header for your domain, and one header with *d=amazonses\.com*\. \(Amazon SES adds a signature for *amazonses\.com* automatically whether you have DKIM enabled or not\. You can ignore it\.\) For example, for a domain called *ses\-example\.com*, the DKIM signature header you are looking for might look like:

   ```
   DKIM-Signature: v=1; a=rsa-sha256; q=dns/txt; c=relaxed/simple;
   s=xtk53kxcy4p3t6ztbrffs6d54rsrrhh6; d=ses-example.com;
   t=1366720445;
   h=From:To:Subject:MIME-Version:Content-Type:Content-Transfer-Encoding:Date:Message-ID;
   bh=lcj/Sl5qKl6K6zwFUwb7Flgnngl892pW574kmS1hrS0=;
   b=nhVMQLmSh7/DM5PW7xPV4K/PN4iVY0a5OF4YYk2L7jgUq9hHQlckopxe82TaAr64
   eVTcBhHHj9Bwtzkmuk88g4G5UUN8J+AAsd/JUNGoZOBS1OofSkuAQ6cGfRGanF68Ag7
   nmmEjEi+JL5JQh//u+EKTH4TVb4zdEWlBuMlrdTg=
   ```

**Important**  
 How you update the DNS settings depends on who provides your DNS service\. DNS service may be provided by a domain name registrar such as GoDaddy or Network Solutions, or by a separate service such as Route 53\.

**What if Easy DKIM fails?**

If your DNS settings are not correctly updated, you will first receive an *Amazon SES DKIM FAILURE* email from Amazon Web Services, and you will see a status of *failed* in the Domains area when you click on the DKIM tab\.

**Note**  
If this happens, Amazon SES will still send your email, but *it will not be signed using a DKIM signature*\.