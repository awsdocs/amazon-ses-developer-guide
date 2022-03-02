# Email authentication methods<a name="email-authentication-methods"></a>

Amazon Simple Email Service \(Amazon SES\) uses the Simple Mail Transfer Protocol \(SMTP\) to send email\. Because SMTP does not provide any authentication by itself, spammers can send email messages that claim to originate from someone else, while hiding their true origin\. By falsifying email headers and spoofing source IP addresses, spammers can mislead recipients into believing that the email messages that they are receiving are authentic\.

Most ISPs that forward email traffic take measures to evaluate whether email is legitimate\. One such measure that ISPs take is to determine whether an email is authenticated\. Authentication requires senders to verify that they are the owner of the account that they are sending from\. In some cases, ISPs refuse to forward email that is not authenticated\. To ensure optimal deliverability, we recommend that you authenticate your emails\. 

**Topics**
+ [Authenticating Email with DKIM in Amazon SES](send-email-authentication-dkim.md)
+ [Using a custom MAIL FROM domain](mail-from.md)
+ [Complying with DMARC using Amazon SES](send-email-authentication-dmarc.md)
+ [Authenticating Email with SPF in Amazon SES](send-email-authentication-spf.md)