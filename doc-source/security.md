# Amazon SES and Security Protocols<a name="security"></a>

This topic describes the security protocols that you can use when you connect to Amazon SES, as well as when Amazon SES delivers an email to a receiver\.

## Email Sender to Amazon SES<a name="security-client-to-ses"></a>

The security protocol that you use to connect to Amazon SES depends on whether you are using the Amazon SES API or the Amazon SES SMTP interface, as described next\.

### HTTP<a name="security-client-to-ses-api"></a>

If you are using the Amazon SES API \(either directly or through an AWS SDK\), then all communications are encrypted by TLS through the Amazon SES HTTPS endpoint\. The Amazon SES HTTPS endpoint supports TLS 1\.2, TLS 1\.1, and TLS 1\.0\. 

### SMTP Interface<a name="security-client-to-ses-smtp"></a>

If you are accessing Amazon SES through the SMTP interface, you are required to encrypt your connection using Transport Layer Security \(TLS\)\. Note that TLS is often referred to by the name of its predecessor protocol, Secure Sockets Layer \(SSL\)\.

Amazon SES supports two mechanisms for establishing a TLS\-encrypted connection: STARTTLS and TLS Wrapper\.

+ **STARTTLS**—STARTTLS is a means of upgrading an unencrypted connection to an encrypted connection\. There are versions of STARTTLS for a variety of protocols; the SMTP version is defined in [RFC 3207](https://www.ietf.org/rfc/rfc3207.txt)\. For STARTTLS connections, Amazon SES supports TLS 1\.2, TLS 1\.1, TLS 1\.0 and SSLv2Hello\.

+ **TLS Wrapper**—TLS Wrapper \(also known as SMTPS or the Handshake Protocol\) is a means of initiating an encrypted connection without first establishing an unencrypted connection\. With TLS Wrapper, the Amazon SES SMTP endpoint does not perform TLS negotiation: it is the client's responsibility to connect to the endpoint using TLS, and to continue using TLS for the entire conversation\. TLS Wrapper is an older protocol, but many clients still support it\. For TLS Wrapper connections, Amazon SES supports TLS 1\.2, TLS 1\.1 and TLS 1\.0\.

For information about connecting to the Amazon SES SMTP interface using these methods, see [Connecting to the Amazon SES SMTP Endpoint](smtp-connect.md)\.

## Amazon SES to Receiver<a name="security-ses-to-receiver"></a>

Amazon SES sends messages over a TLS\-protected connection \(TLS version 1\.0 only\) by default\. This method, called *opportunistic TLS*, means that when Amazon SES establishes an SMTP connection with a receiving mail server, Amazon SES upgrades the connection using the STARTTLS protocol if the receiving mail server supports TLS\. If the receiving server does not advertise STARTTLS or if TLS negotiation fails, the connection proceeds in plaintext\. 

Amazon SES supports opportunistic TLS in all regions and you don't need to take any action to enable it\.