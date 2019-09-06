# Amazon SES and Security Protocols<a name="security"></a>

This topic describes the security protocols that you can use when you connect to Amazon SES, as well as when Amazon SES delivers an email to a receiver\.

## Email Sender to Amazon SES<a name="security-client-to-ses"></a>

The security protocol that you use to connect to Amazon SES depends on whether you are using the Amazon SES API or the Amazon SES SMTP interface, as described next\.

### HTTPS<a name="security-client-to-ses-api"></a>

If you are using the Amazon SES API \(either directly or through an AWS SDK\), then all communications are encrypted by TLS through the Amazon SES HTTPS endpoint\. The Amazon SES HTTPS endpoint supports TLS 1\.2, TLS 1\.1, and TLS 1\.0\. 

### SMTP Interface<a name="security-client-to-ses-smtp"></a>

If you are accessing Amazon SES through the SMTP interface, you are required to encrypt your connection using Transport Layer Security \(TLS\)\. Note that TLS is often referred to by the name of its predecessor protocol, Secure Sockets Layer \(SSL\)\.

Amazon SES supports two mechanisms for establishing a TLS\-encrypted connection: STARTTLS and TLS Wrapper\.
+ **STARTTLS**—STARTTLS is a means of upgrading an unencrypted connection to an encrypted connection\. There are versions of STARTTLS for a variety of protocols; the SMTP version is defined in [RFC 3207](https://www.ietf.org/rfc/rfc3207.txt)\. For STARTTLS connections, Amazon SES supports TLS 1\.2, TLS 1\.1, TLS 1\.0 and SSLv2Hello\.
+ **TLS Wrapper**—TLS Wrapper \(also known as SMTPS or the Handshake Protocol\) is a means of initiating an encrypted connection without first establishing an unencrypted connection\. With TLS Wrapper, the Amazon SES SMTP endpoint does not perform TLS negotiation: it is the client's responsibility to connect to the endpoint using TLS, and to continue using TLS for the entire conversation\. TLS Wrapper is an older protocol, but many clients still support it\. For TLS Wrapper connections, Amazon SES supports TLS 1\.2, TLS 1\.1 and TLS 1\.0\.

For information about connecting to the Amazon SES SMTP interface using these methods, see [Connecting to the Amazon SES SMTP Endpoint](smtp-connect.md)\.

## Amazon SES to Receiver<a name="security-ses-to-receiver"></a>

Amazon SES supports TLS 1\.2, TLS 1\.1 and TLS 1\.0 for TLS connections\.

By default, Amazon SES uses *opportunistic TLS*\. This means that Amazon SES always attempts to make a secure connection to the receiving mail server\. If Amazon SES can't establish a secure connection, it sends the message unencrypted\.

You can change this behavior by using configuration sets\. Use the [PutConfigurationSetDeliveryOptions](https://docs.aws.amazon.com/ses/latest/APIReference/API_PutConfigurationSetDeliveryOptions.html) API operation to set the `TlsPolicy` property for a configuration set to `Require`\. You can use the [AWS CLI](https://aws.amazon.com/cli) to make this change\.

**To configure Amazon SES to require TLS connections for a configuration set**
+ At the command line, enter the following command:

  ```
  aws ses put-configuration-set-delivery-options --configuration-set-name MyConfigurationSet --delivery-options TlsPolicy=Require
  ```

  In the preceding example, replace *MyConfigurationSet* with the name of your configuration set\.

  When you send an email using this configuration set, Amazon SES only sends the message to the receiving email server if it can establish a secure connection\. If Amazon SES can't make a secure connection to the receiving email server, it drops the message\.

## End\-to\-End Encryption<a name="security-end-to-end"></a>

You can use Amazon SES to send messages that are encrypted using S/MIME or PGP\. Messages that use these protocols are encrypted by the sender\. Their contents can only be viewed by recipients who possess the public keys that are required to decrypt the messages\.

Amazon SES supports the following MIME types, which you can use to send S/MIME encrypted email:
+ `application/pkcs7-mime`
+ `application/pkcs7-signature`
+ `application/x-pkcs7-mime`
+ `application/x-pkcs7-signature`

Amazon SES also supports the following MIME types, which you can use to send PGP\-encrypted email:
+ `application/pgp-encrypted`
+ `application/pgp-keys`
+ `application/pgp-signature`