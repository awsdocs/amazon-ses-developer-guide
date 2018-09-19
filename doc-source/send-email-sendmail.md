# Integrating Amazon SES with Sendmail<a name="send-email-sendmail"></a>

Sendmail was released in the early 1980s, and has been continuously improved ever since\. It is a flexible and configurable message transfer agent \(MTA\) with a large community of users\. 

Sendmail was acquired by Proofpoint in 2013, but Proofpoint continues to offer an open source version of Sendmail\. You can download the [open source version of Sendmail](https://www.proofpoint.com/us/open-source-email-solution) from the Proofpoint website\.

The instructions in this section show you how to configure Sendmail to send email through Amazon SES\. These instructions were tested on a 64\-bit Amazon EC2 instance using the following Amazon Machine Image \(AMI\):
+ Amazon Linux AMI 2015\.09\.2 \(ami\-8fcee4e5\)

For more information about launching an Amazon EC2 instance, which includes selecting an AMI, see [Amazon Machine Images \(AMIs\)](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-launch-instance_linux.html)\.

 **Prerequisites** 

Before you perform one of the following procedures, verify the following:
+ The Sendmail package is installed on your computer, and you are able to successfully send an email using Sendmail without Amazon SES\. 
**Note**  
To see if a package is installed on a computer running Red Hat Linux, type `rpm -qa | grep <package>`, where `<package>` is the package name\. To see if a package is installed on a computer running Ubuntu Linux, type `dpkg -s <package>`\.
+ In addition to the Sendmail package, the following packages are installed on your computer: sendmail\-cf, m4, and cyrus\-sasl\-plain\.
+ You have verified your "From" address and, if your account is still in the sandbox, you have also verified your "To" addresses\. For more information, see [Verifying Email Addresses in Amazon SES](verify-email-addresses.md)\.
+ \(Optional\) If you are sending email through Amazon SES from an Amazon EC2 instance, you may need to assign an Elastic IP Address to your Amazon EC2 instance for the receiving ISP to accept your email\. For more information, see [Amazon EC2 Elastic IP Addresses](https://aws.amazon.com/articles/1346)\.
+ \(Optional\) If you are sending email through Amazon SES from an Amazon EC2 instance, you can fill out a [Request to Remove Email Sending Limitations](https://aws-portal.amazon.com/gp/aws/html-forms-controller/contactus/ec2-email-limit-rdns-request) to remove the additional sending limit restrictions that are applied to port 25 by default\.

**To configure Sendmail to send email through the Amazon SES endpoint in US West \(Oregon\) using STARTTLS**

1. Open the */etc/mail/authinfo* file for editing\. If the file does not exist, create it\. 
**Important**  
These instructions assume that you want to use Amazon SES in the US West \(Oregon\) AWS region\. If you want to use a different region, replace all instances of *email\-smtp\.us\-west\-2\.amazonaws\.com* in these instructions with the SMTP endpoint of the desired region\. For a list of SMTP endpoints, see [Regions and Amazon SES](regions.md)\.

1. Add the following line to */etc/mail/authinfo*, where:
   + U:root—Do not modify\.
   + I:USERNAME—Replace USERNAME with the Amazon SES username you obtained using the instructions in [Obtaining Your Amazon SES SMTP Credentials](smtp-credentials.md)\. This is NOT the same as your AWS Access Key ID\.
   + P:PASSWORD—Replace PASSWORD with the Amazon SES password you obtained using the instructions in [Obtaining Your Amazon SES SMTP Credentials](smtp-credentials.md)\. This is NOT the same as your AWS Secret Key\.
   + M:LOGIN—Replace LOGIN with the method of authentication to use\. For example, PLAIN, DIGEST\-MD5, etc\.

   ```
   1. AuthInfo:email-smtp.us-west-2.amazonaws.com "U:root" "I:USERNAME" "P:PASSWORD" "M:LOGIN"
   ```

   If Sendmail cannot authenticate with the Amazon SES SMTP endpoint because the hostname does not match, try adding the additional line specified in [Amazon SES SMTP Issues](smtp-issues.md)\.

1. Save the *authinfo* file\.

1. At a command prompt, type the following command to generate */etc/mail/authinfo\.db*:

   ```
   sudo makemap hash /etc/mail/authinfo.db < /etc/mail/authinfo 
   ```

1. Open the */etc/mail/access* file and include support for relaying to the Amazon SES SMTP endpoint by adding the following line\. If Sendmail cannot authenticate with the Amazon SES SMTP endpoint because the hostname does not match, try adding the additional line specified in [Amazon SES SMTP Issues](smtp-issues.md)\.

   ```
   1. Connect:email-smtp.us-west-2.amazonaws.com RELAY
   ```

   Save the file\.

1. At a command prompt, type the following command to regenerate */etc/mail/access\.db*:

    `sudo makemap hash /etc/mail/access.db < /etc/mail/access ` 

1. Save a back\-up copy of */etc/mail/sendmail\.mc* and */etc/mail/sendmail\.cf*\.

1. Add the following group of lines to the */etc/mail/sendmail\.mc* file before any MAILER\(\) definitions\. If you add a FEATURE\(\) line after a MAILER\(\) definition, when you run `m4` in a subsequent step, you will get the following error: `"ERROR: FEATURE() should be before MAILER()."`:
**Important**  
If you are using an AWS region other than US West \(Oregon\), replace the `SMART_HOST` value with the Amazon SES SMTP endpoint of the region you're using, and be sure to use the ` character and the apostrophe exactly as shown\.

   ```
   1. define(`SMART_HOST', `email-smtp.us-west-2.amazonaws.com')dnl
   2. define(`RELAY_MAILER_ARGS', `TCP $h 25')dnl
   3. define(`confAUTH_MECHANISMS', `LOGIN PLAIN')dnl
   4. FEATURE(`authinfo', `hash -o /etc/mail/authinfo.db')dnl
   5. MASQUERADE_AS(`YOUR_DOMAIN')dnl
   6. FEATURE(masquerade_envelope)dnl
   7. FEATURE(masquerade_entire_domain)dnl
   ```

1. In the text you just added to *sendmail\.mc*, in the line that starts with `MASQUERADE_AS`, replace *YOUR\_DOMAIN* with the domain name from which you are sending your email\. By adding this masquerade, you are making email from this host appear to be sent from your domain\. Otherwise, the email will appear as if the email is being sent from the host name of the mail server, and you may get an "Email address not verified" error when you try to send an email\.

1. Save the *sendmail\.mc* file\.

1. At a command prompt, type the following command to make *sendmail\.cf* writeable:

    `sudo chmod 666 /etc/mail/sendmail.cf` 

1. At a command prompt, type the following command to regenerate *sendmail\.cf*:

    `sudo m4 /etc/mail/sendmail.mc > /etc/mail/sendmail.cf` 
**Note**  
If you encounter errors such as "Command not found" and "No such file or directory," make sure you have installed the m4 and sendmail\-cf packages as specified in the prerequisites section above\.

1. At a command prompt, type the following command to reset the permissions of *sendmail\.cf* to read only:

    `sudo chmod 644 /etc/mail/sendmail.cf` 

1. At a command prompt, type the following command to restart Sendmail:

    `sudo /etc/init.d/sendmail restart` 

1. Send a test email by doing the following:

   1. At a command prompt, type the following\. Note that you should replace *from@example\.com* with your "From" email address, which you must have verified with Amazon SES\. Replace *to@example\.com* with your "To" address\. If your account is still in the sandbox, the "To" address must also be verified\.

      ```
      1. sudo /usr/sbin/sendmail -f from@example.com to@example.com
      ```

   1. Press <Enter>\. Type the body of the message, pressing <Enter> after each line\.

   1. When you are finished typing the email, press CTRL\+D to send the email\.

1. Check the recipient email's client for the email\. If you cannot find the email, check the Junk box in the recipient's email client\. If you still cannot find the email, look at the Sendmail log on the mail server\. The log is typically in */var/spool/mail/<user>*\. 