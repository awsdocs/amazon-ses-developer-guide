# Integrating Amazon SES with your existing email server<a name="send-email-smtp-existing-server"></a>

If you currently administer your own email server, you can use the Amazon SES SMTP endpoint to send all of your outgoing email to Amazon SES\. There is no need to modify your existing email clients and applications; the changeover to Amazon SES will be transparent to them\.

Several mail transfer agents \(MTAs\) support sending email through SMTP relays\. This section provides general guidance on how to configure some popular MTAs to send email using Amazon SES SMTP interface\.

The Amazon SES SMTP endpoint requires that all connections be encrypted using Transport Layer Security \(TLS\)\.

**Topics**
+ [Integrating Amazon SES with Postfix](postfix.md)
+ [Integrating Amazon SES with Sendmail](send-email-sendmail.md)
+ [Integrating Amazon SES with Microsoft Windows Server IIS SMTP](send-email-windows-server.md)
+ [Integrating Amazon SES with Exim](send-email-exim.md)