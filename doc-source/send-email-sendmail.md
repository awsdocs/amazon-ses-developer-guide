# Integrating Amazon SES with Sendmail<a name="send-email-sendmail"></a>

Sendmail was released in the early 1980s, and has been continuously improved ever since\. It is a flexible and configurable message transfer agent \(MTA\) with a large community of users\. Sendmail was acquired by Proofpoint in 2013, but Proofpoint continues to offer an open source version of Sendmail\. You can download the [open source version of Sendmail](https://www.proofpoint.com/us/open-source-email-solution) from the Proofpoint website, or through the package managers of most Linux distributions\.

The procedure in this section shows you how to configure Sendmail to send email through Amazon SES\. This procedure was tested on a server running Ubuntu 18\.04\.2 LTS\.

**Note**  
Sendmail is a third\-party application, and isn't developed or supported by Amazon Web Services\. The procedures in this section are provided for informational purposes only, and are subject to change without notice\.

## Prerequisites<a name="send-email-sendmail-prerequisites"></a>

Before you complete the procedure in this section, you should complete the following steps:
+ Install the Sendmail package on your server\. 
**Note**  
Depending on which operating system distribution you use, you might also need to install the following packages: `sendmail-cf`, `m4`, and `cyrus-sasl-plain`\.
+ Verify an identity to use as your From address\. For more information, see [Verifying Email Addresses in Amazon SES](verify-email-addresses.md)

  If your account is still in the Amazon SES sandbox, you also have to verify the addresses that you send email to\. For more information, see [Moving Out of the Amazon SES Sandbox](request-production-access.md)\.

If you're using Amazon SES to send email from an Amazon EC2 instance, you should also complete the following steps:
+ If you're using Amazon SES to send email from an Amazon EC2 instance, you might need to assign an Elastic IP Address to your Amazon EC2 instance in order for receiving email providers to accept your email\. For more information, see [Amazon EC2 Elastic IP Addresses](https://aws.amazon.com/articles/1346)\.
+ If you're using Amazon SES to send email from an Amazon EC2 instance, you should complete the [Request to Remove Email Sending Limitations](https://aws-portal.amazon.com/gp/aws/html-forms-controller/contactus/ec2-email-limit-rdns-request) form\. Requesting this change removes the restrictions that Amazon EC2 applies to port 25 by default\.

## Configuring Sendmail<a name="send-email-sendmail-procedure"></a>

Complete the steps in this section to configure Sendmail to send email by using Amazon SES\.

**Important**  
The procedure in this section assumes that you want to use Amazon SES in the US West \(Oregon\) AWS Region\. If you want to use a different Region, replace all instances of *email\-smtp\.us\-west\-2\.amazonaws\.com* in this procedure with the SMTP endpoint of the desired region\. For a list of SMTP endpoint URLs for the AWS Regions where Amazon SES is available, see [Amazon Simple Email Service \(Amazon SES\)](https://docs.aws.amazon.com/general/latest/gr/rande.html#ses_region) in the *AWS General Reference*\.

**To configure Sendmail**

1. In a file editor, open the file `/etc/mail/authinfo`\. If the file doesn't exist, create it\.

   Add the following line to */etc/mail/authinfo*:

   ```
   AuthInfo:email-smtp.us-west-2.amazonaws.com "U:root" "I:smtpUsername" "P:smtpPassword" "M:PLAIN"
   ```

   In the preceding example, make the following changes:
   + Replace *email\-smtp\.us\-west\-2\.amazonaws\.com* with the Amazon SES SMTP endpoint that you want to use\.
   + Replace *smtpUsername* with your Amazon SES SMTP username\.
   + Replace *smtpPassword* with your Amazon SES SMTP password\.
**Note**  
Your SMTP username and password are different from your AWS Access Key ID and Secret Access Key\. For more information about obtaining your SMTP username and password, see [Obtaining Your Amazon SES SMTP Credentials](smtp-credentials.md)\.

   When you finish, save `authinfo`\.

1. At the command line, enter the following command to generate the `/etc/mail/authinfo.db` file:

   ```
   sudo sh -c 'makemap hash /etc/mail/authinfo.db < /etc/mail/authinfo'
   ```

1. At the command line, type the following command to add support for relaying to the Amazon SES SMTP endpoint\.

   ```
   sudo sh -c 'echo "Connect:email-smtp.us-west-2.amazonaws.com RELAY" >> /etc/mail/access'
   ```

   In the preceding command, replace *email\-smtp\.us\-west\-2\.amazonaws\.com* with the address of the Amazon SES SMTP endpoint that you want to use\.

1. At the command line, type the following command to regenerate */etc/mail/access\.db*:

   ```
   sudo sh -c 'makemap hash /etc/mail/access.db < /etc/mail/access'
   ```

1. At the command line, type the following command to create backups of the `sendmail.cf` and `sendmail.mc` files:

   ```
   sudo sh -c 'cp /etc/mail/sendmail.cf /etc/mail/sendmail_cf.backup && cp /etc/mail/sendmail.mc /etc/mail/sendmail_mc.backup'
   ```

1. Add the following lines to the */etc/mail/sendmail\.mc* file before any `MAILER()` definitions\.

   ```
   define(`SMART_HOST', `email-smtp.us-west-2.amazonaws.com')dnl
   define(`RELAY_MAILER_ARGS', `TCP $h 25')dnl
   define(`confAUTH_MECHANISMS', `LOGIN PLAIN')dnl
   FEATURE(`authinfo', `hash -o /etc/mail/authinfo.db')dnl
   MASQUERADE_AS(`example.com')dnl
   FEATURE(masquerade_envelope)dnl
   FEATURE(masquerade_entire_domain)dnl
   ```

   In the preceding text, do the following:
   + Replace *email\-smtp\.us\-west\-2\.amazonaws\.com* with the Amazon SES SMTP endpoint that you want to use\.
   + Replace *example\.com* with the domain that you're sending email from\.

   When you finish, save the file\.

1. At the command line, type the following command to make *sendmail\.cf* writeable:

   ```
   sudo chmod 666 /etc/mail/sendmail.cf
   ```

1. At the command line, type the following command to regenerate *sendmail\.cf*:

   ```
   sudo sh -c 'm4 /etc/mail/sendmail.mc > /etc/mail/sendmail.cf'
   ```
**Note**  
If you encounter errors such as "Command not found" and "No such file or directory," make sure that the `m4` and `sendmail-cf` packages are installed on your system\.

1. At the command line, type the following command to reset the permissions of *sendmail\.cf* to read only:

   ```
   sudo chmod 644 /etc/mail/sendmail.cf
   ```

1. At the command line, type the following command to restart Sendmail:

   ```
   sudo /etc/init.d/sendmail restart
   ```

1. Complete the following steps to send a test email:

   1. At the command line, enter the following command\.

      ```
      /usr/sbin/sendmail -vf sender@example.com recipient@example.com
      ```

      Replace *sender@example\.com* with your From email address\. Replace *recipient@example\.com* with the To address\. When you finish, press Enter\.

   1. Enter the following message content\. Press Enter at the end of each line\.

      ```
      From: sender@example.com
      To: recipient@example.com
      Subject: Amazon SES test email
      
      This is a test message sent from Amazon SES using Sendmail.
      ```

      When you finish entering the content of the email, press Ctrl\+D to send it\.

1. Check the recipient email's client for the email\. If you can't find the email, check the junk mail folder\. If you still can't find the email, check the Sendmail log on your mail server\. The log is often located at */var/log/mail\.log*\. 