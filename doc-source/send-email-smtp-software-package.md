# Sending email through Amazon SES using software packages<a name="send-email-smtp-software-package"></a>

There are a number of commercial and open source software packages that support sending email through SMTP\. Here are some examples:
+ Blogging platforms
+ RSS aggregators
+ List management software
+ Workflow systems

You can configure any such SMTP\-enabled software to send email through the Amazon SES SMTP interface\. For instructions on how to configure SMTP for a particular software package, see the documentation for that software\.

The following procedure shows how to set up Amazon SES sending in JIRA, a popular issue\-tracking solution\. With this configuration, JIRA can notify users through email whenever there is a change in the status of a software issue\.

**To configure JIRA to send email using Amazon SES**

1. Using your web browser, log in to JIRA with administrator credentials\.

1. In the browser window, choose **Administration**\.

1. On the **System** menu, choose **Mail**\.

1. On the **Mail administration** page, choose **Mail Servers**\.

1. Choose **Configure new SMTP mail server**\.

1. On the **Add SMTP Mail Server** form, fill in the following fields:

   1. **Name**—A descriptive name for this server\.

   1. **From address**—The address from which email will be sent\. You must verify this email address with Amazon SES before you can send from it\. For more information about verification, see [Verified identities in Amazon SES](verify-addresses-and-domains.md)\.

   1. **Email prefix**—A string that JIRA prepends to each subject line prior to sending\.

   1. **Protocol**—Choose **SMTP**\.
**Note**  
If you can't connect to Amazon SES using this setting, try **SECURE\_SMTP**\.

   1. **Hostname**—See [Connecting to an Amazon SES SMTP endpoint](smtp-connect.md) for a list of Amazon SES SMTP endpoints\. For example, if you want to use the Amazon SES endpoint in the US West \(Oregon\) Region, the hostname would be *email\-smtp\.us\-west\-2\.amazonaws\.com*\.

   1. **SMTP port**—25, 587, or 2587 \(to connect using STARTTLS\), or 465 or 2465 \(to connect using TLS Wrapper\)\.

   1. **TLS**—Select this check box\.

   1. **User name**—Your SMTP user name\.

   1. **Password**—Your SMTP password\.

   You can see the settings for TLS Wrapper in the following image\.  
![\[SMTP email configuration for JIRA\]](http://docs.aws.amazon.com/ses/latest/dg/images/SMTP_jira.png)

1. Choose **Test Connection**\. If the test email that JIRA sends through Amazon SES arrives successfully, then your configuration is complete\.