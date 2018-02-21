# Disabling Easy DKIM in Amazon SES<a name="disable-easy-dkim"></a>

If you want to temporarily stop Amazon SES from signing your messages using DKIM, you can disable Easy DKIM for your email address or domain\. You can reenable it at any time\.

**To disable Easy DKIM signing**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane, under **Identities**, click either **Email Addresses** or **Domains**, depending whether you want to disable Easy DKIM signing for an email address or a domain\.

1. Click the email address or domain for which you wish to disable Easy DKIM signing\.

1. On the Details page of the email address or domain, expand **DKIM**\.

1. In the **DKIM:** field, click **disable**\. Amazon SES will no longer DKIM\-sign emails that you send from this identity\.
**Note**  
If you do not see the **disable** option as in the figure below, then DKIM is already disabled\.  
![\[Disabling DKIM\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/dkim_disable.png)

**Note**  
If you want to *permanently* disable DKIM signing from any email address on that domain, you should also remove the CNAME records from your DNS\.