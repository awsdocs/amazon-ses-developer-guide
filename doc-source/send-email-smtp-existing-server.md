# Integrating Amazon SES with Your Existing Email Server<a name="send-email-smtp-existing-server"></a>

If you currently administer your own email server, you can use the Amazon SES SMTP endpoint to send all of your outgoing email to Amazon SES\. There is no need to modify your existing email clients and applications; the changeover to Amazon SES will be transparent to them\.

Several mail transfer agents \(MTAs\) support sending email through SMTP relays\. This section provides general guidance on how to configure some popular MTAs to send email using Amazon SES SMTP interface\.

+ To configure Postfix to send email through Amazon SES, see [Integrating Amazon SES with Postfix](postfix.md)\.

+ To configure Sendmail to send email through Amazon SES, see [Integrating Amazon SES with Sendmail](sendmail.md)\.

+ To configure Microsoft Exchange to send email through Amazon SES, see [Integrating Amazon SES with Microsoft Exchange](exchange.md)\.

+ To configure Microsoft Windows Server's IIS SMTP server to send email through Amazon SES, see [Integrating Amazon SES with Microsoft Windows Server IIS SMTP](windows-server-iis-smtp.md)\.

+ To configure Exim to send email through Amazon SES, see [Integrating Amazon SES with Exim](exim.md)\.

The Amazon SES SMTP endpoint requires that all connections be encrypted using Transport Layer Security \(TLS\)\.