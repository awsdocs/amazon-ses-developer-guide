# Authenticating Email with DKIM in Amazon SES<a name="send-email-authentication-dkim"></a>

*DomainKeys Identified Mail* \(*DKIM*\) is a standard that allows senders to sign their email messages with a cryptographic key\. Email providers then use these signatures to verify that the messages weren't modified by a third party while in transit\.

An email message that is sent using DKIM includes a *DKIM\-Signature* header field that contains a cryptographically signed representation of the message\. A provider that receives the message can use a public key, which is published in the sender's DNS record, to decode the signature\. Email providers then use this information to determine whether messages are authentic\.

DKIM signatures are optional\. You might decide to sign your email using a DKIM signature to enhance deliverability with DKIM\-compliant email providers\. Amazon SES provides three options for signing your messages using a DKIM signature:
+ To set up a sending identity \(such as a domain or an email address\) so that Amazon SES automatically adds a DKIM signature to every message that you send from that identity, see [Easy DKIM in Amazon SES](send-email-authentication-dkim-easy.md)\.
+ To use your own public\-private key pair for DKIM authentication, see [Provide Your Own DKIM Authentication Token in Amazon SES](send-email-authentication-dkim-bring-your-own.md)\.
+ To add your own DKIM signature to email that you send using the `SendRawEmail` API, see [Manual DKIM Signing in Amazon SES](send-email-authentication-dkim-manual.md)\.
