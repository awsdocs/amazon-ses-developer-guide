# Integrating Amazon SES with Exim<a name="send-email-exim"></a>

Exim is an MTA that was originally developed for Unix\-like systems\. It is a general purpose mail program that is very flexible and configurable\.

To learn more about Exim, go to [http://www\.exim\.org](http://www.exim.org)\.

**To configure integration with the Amazon SES US West \(Oregon\) endpoint using STARTTLS**

1. Open the */etc/exim/exim\.conf* file for editing\. If the file does not exist, create it\. 
**Important**  
These instructions assume that you want to use Amazon SES in the US West \(Oregon\) AWS Region\. If you want to use a different region, replace all instances of *email\-smtp\.us\-west\-2\.amazonaws\.com* in these instructions with the SMTP endpoint of the desired region\. For a list of SMTP endpoint URLs for the AWS Regions where Amazon SES is available, see [Amazon Simple Email Service \(Amazon SES\)](https://docs.aws.amazon.com/general/latest/gr/rande.html#ses_region) in the *AWS General Reference*\.

1. In */etc/exim/exim\.conf*, make the following changes:

   1. In the *routers* section, after the *begin routers* line, add the following:

      ```
      1. send_via_ses:
      2. driver = manualroute
      3. domains = ! +local_domains
      4. transport = ses_smtp
      5. route_list = * email-smtp.us-west-2.amazonaws.com;
      ```

   1. In the *transports* section, after the *begin transports* line, add the following:

      ```
      1. ses_smtp:
      2. driver = smtp
      3. port = 587
      4. hosts_require_auth = *
      5. hosts_require_tls = *
      ```

   1. In the *authenticators* section, after the *begin authenticators* line, add the following, replacing USERNAME and PASSWORD with your SMTP user name and password:
**Important**  
Use your SMTP user name and password, not your AWS access key ID and secret access key\. Your SMTP credentials and your AWS credentials are not the same\. For information about how to obtain your SMTP credentials, see [Obtaining Your Amazon SES SMTP Credentials](smtp-credentials.md)\.

      ```
      1. ses_login:
      2. driver = plaintext
      3. public_name = LOGIN
      4. client_send = : USERNAME : PASSWORD
      ```

1. Save the */etc/exim/exim\.conf* file\.

When you have finished updating the configuration, restart Exim\. At the command line, type the following command and press ENTER\.

 `sudo /etc/init.d/exim restart` 

**Note**  
This command may not be exactly the same on your particular server\.

When you have completed this procedure, your outgoing email will be sent via Amazon SES\. To test your configuration, send an email message through your Exim server, and then verify that arrives at its destination\. If the message is not delivered, then check your system's mail log for errors\. On many systems, this is the /var/log/exim/main\.log file\.