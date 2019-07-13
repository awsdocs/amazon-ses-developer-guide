# Connecting to the Amazon SES SMTP Endpoint<a name="smtp-connect"></a>

The following table shows the Amazon SES SMTP endpoints for the AWS Regions where Amazon SES is available\.


| Region name | SMTP endpoint | 
| --- | --- | 
|  US East \(N\. Virginia\)  |  email\-smtp\.us\-east\-1\.amazonaws\.com  | 
|  US West \(Oregon\)  |  email\-smtp\.us\-west\-2\.amazonaws\.com  | 
|  EU \(Ireland\)  |  email\-smtp\.eu\-west\-1\.amazonaws\.com  | 

The Amazon SES SMTP endpoint requires that all connections be encrypted using Transport Layer Security \(TLS\)\. \(Note that TLS is often referred to by the name of its predecessor protocol, SSL\.\) Amazon SES supports two mechanisms for establishing a TLS\-encrypted connection: STARTTLS and TLS Wrapper\. Check the documentation for your software to determine whether it supports STARTTLS, TLS Wrapper, or both\.

**Important**  
Amazon Elastic Compute Cloud \(Amazon EC2\) throttles email traffic over port 25 by default\. To avoid timeouts when sending email through the SMTP endpoint from EC2, use a different port \(587 or 2587\) or fill out a [Request to Remove Email Sending Limitations](https://aws-portal.amazon.com/gp/aws/html-forms-controller/contactus/ec2-email-limit-rdns-request) to remove the throttle\.

## STARTTLS<a name="smtp-connect-starttls"></a>

STARTTLS is a means of upgrading an unencrypted connection to an encrypted connection\. There are versions of STARTTLS for a variety of protocols; the SMTP version is defined in [RFC 3207](https://www.ietf.org/rfc/rfc3207.txt)\.

To set up a STARTTLS connection, the SMTP client connects to the Amazon SES SMTP endpoint on port 25, 587, or 2587, issues an EHLO command, and waits for the server to announce that it supports the STARTTLS SMTP extension\. The client then issues the STARTTLS command, initiating TLS negotiation\. When negotiation is complete, the client issues an EHLO command over the new encrypted connection, and the SMTP session proceeds normally\.

## TLS Wrapper<a name="smtp-connect-tlswrapper"></a>

TLS Wrapper \(also known as SMTPS or the Handshake Protocol\) is a means of initiating an encrypted connection without first establishing an unencrypted connection\. With TLS Wrapper, the Amazon SES SMTP endpoint does not perform TLS negotiation: it is the client's responsibility to connect to the endpoint using TLS, and to continue using TLS for the entire conversation\. TLS Wrapper is an older protocol, but many clients still support it\.

To set up a TLS Wrapper connection, the SMTP client connects to the Amazon SES SMTP endpoint on port 465 or 2465\. The server presents its certificate, the client issues an EHLO command, and the SMTP session proceeds normally\.