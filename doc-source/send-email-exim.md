# Integrating Amazon SES with Exim<a name="send-email-exim"></a>

Exim is a Mail Transfer Agent \(MTA\) that is highly flexible and configurable\. To learn more about Exim, visit the [Exim website](http://www.exim.org)\.

**Note**  
Exim is a third\-party application, and isn't developed or supported by Amazon Web Services\. The procedures in this section are provided for informational purposes only, and are subject to change without notice\.

**To configure Exim to send email through Amazon SES**

1. In a text editor, open the file `/etc/exim.conf.local`\. If the file doesn't exist, copy the template from `/etc/exim4/exim4.conf.template`\.

1. In `/etc/exim.conf.local`, make the following changes:

   1. In the `routers` section, after the `begin routers` line, add the following:

      ```
      send_via_ses:
      driver = manualroute
      domains = ! +local_domains
      transport = ses_smtp
      route_list = * email-smtp.us-west-2.amazonaws.com;
      ```

      In the preceding code, replace *email\-smtp\.us\-west\-2\.amazonaws\.com* with the SMTP endpoint that you want to use to send the message\. For more information, see [Regions and Amazon SES](regions.md)\.

   1. In the `transports` section, after the `begin transports` line, add the following:

      ```
      ses_smtp:
      driver = smtp
      port = 587
      hosts_require_auth = *
      hosts_require_tls = *
      ```

   1. In the `authenticators` section, after the `begin authenticators` line, add the following:

      ```
      ses_login:
      driver = plaintext
      public_name = LOGIN
      client_send = : USERNAME : PASSWORD
      ```

      In the preceding code, replace *USERNAME* with your SMTP username, and *PASSWORD* with your SMTP password\.
**Important**  
Your SMTP credentials are not the same as your AWS Access Key ID and Secret Access Key\. For information about obtaining your SMTP credentials, see [Obtaining your Amazon SES SMTP credentials](smtp-credentials.md)\.

1. Save `/etc/exim.conf.local`\.

1. When you finish updating the configuration, enter the following command to restart Exim\.

   ```
   sudo /etc/init.d/exim4 restart
   ```
**Note**  
This command might differ depending on which operating system you use\.

1. At the command line, complete the following steps to send a test message:

   1. Enter the following command:

      ```
      exim -v recipient@example.com
      ```

      In the preceding command, replace *recipient@example\.com* with the address that you want to send the message to\.

   1. Enter the following, pressing Enter at the end of each line:

      ```
      From: sender@example.com
      Subject: Test message
      This is a test.
      
      .
      ```

      In the preceding command, replace *sender@example\.com* with the address that you want to send the message from\.

      When you press Enter after the final period \(\.\), Exim begins the conversation with the SMTP server\. If the connection remains open after the message is sent, press Ctrl\+D to close it\.
**Tip**  
If the message isn't delivered, check your system's mail log for errors\. The Exim mail log is usually located at `/var/log/exim4/mainlog`\.