# Connecting to an Amazon SES SMTP endpoint<a name="smtp-connect"></a>

To send email using the Amazon SES SMTP interface, you connect to an SMTP endpoint\. For a complete list of Amazon SES SMTP endpoints, see [Amazon Simple Email Service endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/ses.html) in the *AWS General Reference*\.

The Amazon SES SMTP endpoint requires that all connections be encrypted using Transport Layer Security \(TLS\)\. \(Note that TLS is often referred to by the name of its predecessor protocol, SSL\.\) Amazon SES supports two mechanisms for establishing a TLS\-encrypted connection: STARTTLS and TLS Wrapper\. Check the documentation for your software to determine whether it supports STARTTLS, TLS Wrapper, or both\.

**Note**  
Amazon Elastic Compute Cloud \(Amazon EC2\) restricts email traffic over port 25 by default\. To avoid timeouts when sending email through the SMTP endpoint from Amazon EC2, you can request that these restrictions be removed\. For more information, see [How do I remove the restriction on port 25 from my Amazon EC2 instance or AWS Lambda function?](https://aws.amazon.com/premiumsupport/knowledge-center/ec2-port-25-throttle/) in the AWS Knowledge Center\.  
Alternatively, you can use a different port, or use an [Amazon VPC endpoint](send-email-set-up-vpc-endpoints.md)\.

## STARTTLS<a name="smtp-connect-starttls"></a>

STARTTLS is a means of upgrading an unencrypted connection to an encrypted connection\. There are versions of STARTTLS for a variety of protocols; the SMTP version is defined in [RFC 3207](https://www.ietf.org/rfc/rfc3207.txt)\.

To set up a STARTTLS connection, the SMTP client connects to the Amazon SES SMTP endpoint on port 25, 587, or 2587, issues an EHLO command, and waits for the server to announce that it supports the STARTTLS SMTP extension\. The client then issues the STARTTLS command, initiating TLS negotiation\. When negotiation is complete, the client issues an EHLO command over the new encrypted connection, and the SMTP session proceeds normally\.

## TLS Wrapper<a name="smtp-connect-tlswrapper"></a>

TLS Wrapper \(also known as SMTPS or the Handshake Protocol\) is a means of initiating an encrypted connection without first establishing an unencrypted connection\. With TLS Wrapper, the Amazon SES SMTP endpoint does not perform TLS negotiation: it is the client's responsibility to connect to the endpoint using TLS, and to continue using TLS for the entire conversation\. TLS Wrapper is an older protocol, but many clients still support it\.

To set up a TLS Wrapper connection, the SMTP client connects to the Amazon SES SMTP endpoint on port 465 or 2465\. The server presents its certificate, the client issues an EHLO command, and the SMTP session proceeds normally\.