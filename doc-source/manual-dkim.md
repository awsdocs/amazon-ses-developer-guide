# Manual DKIM Signing in Amazon SES<a name="manual-dkim"></a>

If you prefer not to use Easy DKIM, you can still sign your email messages using a DKIM signature and send them using Amazon SES\. To do this, you must use the `SendRawEmail` API and self\-sign your message content according to the specifications provided at [http://www\.dkim\.org](http://www.dkim.org)\. If you use this approach, be aware that Amazon SES does not validate the DKIM signature that you construct\. If there are any errors in the signature, you will need to correct them yourself\. If you DKIM\-sign your own email messages, we recommend that you use keys that are at least 1024 bits\.

Whether or not you DKIM\-sign your messages, Amazon SES automatically adds a DKIM header with *d=amazonses\.com*, which you can ignore\. If you do DKIM\-sign your messages, it is expected that there will be two DKIM headers: one for your domain, and one for *amazonses\.com*\.

**Important**  
To ensure maximum deliverability, do *not* sign any of the following headers using a DKIM signature:  
Message\-ID
Date
Return\-Path
Bounces\-To

**Note**  
If you are using the Amazon SES SMTP interface to send email, and your client software automatically performs DKIM signing, you should check to ensure that your client does not sign any of the headers listed above\. We recommend that you check the documentation for your software to find out exactly what headers are signed with DKIM\.  
For more information about the Amazon SES SMTP interface, see [Using the Amazon SES SMTP Interface to Send Email](send-email-smtp.md)\. 