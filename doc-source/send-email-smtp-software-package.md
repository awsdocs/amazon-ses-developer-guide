# Sending Email Through Amazon SES From Software Packages<a name="send-email-smtp-software-package"></a>

There are a number of commercial and open source software packages that support sending email via SMTP\. Here are some examples:
+ Blogging platforms
+ RSS aggregators
+ List management software
+ Workflow systems

You can configure any such SMTP\-enabled software to send email through the Amazon SES SMTP interface\. For instructions on how to configure SMTP for a particular software package, see the documentation for that software\.

The following procedure shows how to set up Amazon SES sending in JIRA, a popular issue\-tracking solution\. With this configuration, JIRA can notify users via email whenever there is a change in the status of a software issue\.

**To Configure JIRA to Send Email Using Amazon SES**

1. Using your web browser, log in to JIRA with administrator credentials\.

1. In the browser window, choose **Administration**\.

1. On the **System** menu, choose **Mail**\.

1. On the **Mail administration** page, choose **Mail Servers**\.

1. Choose **Configure new SMTP mail server**\.

1. On the **Add SMTP Mail Server** form, fill in the following fields:

   1. **Name**—A descriptive name for this server\.

   1. **From address**—The address from which email will be sent\. You will need to verify this email address with Amazon SES before you can send from it\. For more information about verification, see [Verifying identities in Amazon SES](verify-addresses-and-domains.md)\.

   1. **Email prefix**—A string that JIRA prepends to each subject line prior to sending\.

   1. **Protocol**—Choose **SMTP**\.
**Note**  
If you cannot connect to Amazon SES using this setting, try **SECURE\_SMTP**\.

   1. **Host Name**—See [Connecting to the Amazon SES SMTP Endpoint](smtp-connect.md) for a list of Amazon SES SMTP endpoints\. For example, if you want to use the Amazon SES endpoint in the US West \(Oregon\) region, the host name would be *email\-smtp\.us\-west\-2\.amazonaws\.com*\.

   1. **SMTP Port**—25, 587, or 2587 \(to connect using STARTTLS\), or 465 or 2465 \(to connect using TLS Wrapper\)\.

   1. **TLS**—Select this check box\.

   1. **Username**—Your SMTP username\.

   1. **Password**—Your SMTP password\.

   Settings for TLS Wrapper are shown below\.  
![\[SMTP email configuration for JIRA\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/SMTP_jira.png)

1. Choose **Test Connection**\. If the test email that JIRA sends through Amazon SES arrives successfully, then your configuration is complete\.