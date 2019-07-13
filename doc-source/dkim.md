# Authenticating Email with DKIM in Amazon SES<a name="dkim"></a>

*DomainKeys Identified Mail* \(*DKIM*\) is a standard that allows senders to sign their email messages with a cryptographic key\. Email providers then use these signatures to verify that the messages weren't modified by a third party while in transit\.

An email message that is sent using DKIM includes a *DKIM\-Signature* header field that contains a cryptographically signed representation of the message\. A provider that receives the message can use a public key, which is published in the sender's DNS record, to decode the signature\. Email providers then use this information to determine whether messages are authentic\. To learn more about DKIM, see [http://dkim\.org](http://dkim.org)\.

DKIM signatures are optional\. You might decide to sign your email using a DKIM signature to enhance deliverability with DKIM\-compliant email providers\. Amazon SES provides two options to sign your messages using a DKIM signature:
+ To set up a sending identity \(such as a domain or an email address\) so that Amazon SES automatically adds a DKIM signature to every message that you send from that identity, see [Easy DKIM in Amazon SES](easy-dkim.md)\.
+ To add your own DKIM signature to email that you send using the `SendRawEmail` API, see [Manual DKIM Signing in Amazon SES](manual-dkim.md)\.