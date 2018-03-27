# Authenticating Email with DKIM in Amazon SES<a name="dkim"></a>

DomainKeys Identified Mail \(*DKIM*\) is a standard that allows senders to sign their email messages and ISPs to use those signatures to verify that those messages are legitimate and have not been modified by a third party in transit\.

An email message that is sent using DKIM includes a *DKIM\-Signature* header field that contains a cryptographically signed representation of all, or part, of the message\. An ISP receiving the message can decode the cryptographic signature using a public key, published in the sender's DNS record, to ensure that the message is authentic\. For more information about DKIM, see [http://www\.dkim\.org](http://www.dkim.org)\.

DKIM signatures are optional\. You might decide to sign your email using a DKIM signature to enhance deliverability with DKIM\-compliant ISPs\. Amazon SES provides two options to sign your messages using a DKIM signature:
+ To set up your domain so that Amazon SES automatically adds a DKIM signature to every message sent from that domain, see [Easy DKIM in Amazon SES](easy-dkim.md)\.
+ To add your own DKIM signature to any email that you send using the `SendRawEmail` API, see [Manual DKIM Signing in Amazon SES](manual-dkim.md)\.