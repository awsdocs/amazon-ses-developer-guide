# Authenticating Email with DKIM in Amazon SES<a name="send-email-authentication-dkim"></a>

*DomainKeys Identified Mail* \(*DKIM*\) is an email security standard designed to make sure that an email that claims to have come from a specific domain was indeed authorized by the owner of that domain\. It uses public\-key cryptography to sign an email with a private key\. Recipient servers can then use a public key published to a domain's DNS to verify that parts of the email have not been modified during the transit\.

DKIM signatures are optional\. You might decide to sign your email using a DKIM signature to enhance deliverability with DKIM\-compliant email providers\. Amazon SES provides three options for signing your messages using a DKIM signature:
+ **Easy DKIM**: To set up a sending identity so that Amazon SES generates a public\-private key pair and automatically adds a DKIM signature to every message that you send from that identity, see [Easy DKIM in Amazon SES](send-email-authentication-dkim-easy.md)\.
+ **BYODKIM \(Bring Your Own DKIM\)**: To provide your own public\-private key pair for so SES adds a DKIM signature to every message that you send from that identity, see [Provide your own DKIM authentication token \(BYODKIM\) in Amazon SES](send-email-authentication-dkim-bring-your-own.md)\.
+ **Manually add DKIM signature**: To add your own DKIM signature to email that you send using the `SendRawEmail` API, see [Manual DKIM signing in Amazon SES](send-email-authentication-dkim-manual.md)\.

## DKIM signing key length<a name="send-email-authentication-dkim-1024-2048"></a>

Since many DNS providers now fully support DKIM 2048 bit RSA encryption, Amazon SES also supports DKIM 2048 to allow more secure authentication of emails and therefore uses it as the default key length when you configure Easy DKIM either from the API or the console\. 2048 bit keys can be setup and used in Bring Your Own DKIM \(BYODKIM\) as well, where your signing key length must be at least 1024 bits and no more than 2048 bits\.

For the sake of security as well as your email’s deliverability, when configured with Easy DKIM, you have the choice to use either 1024 and 2048 bit key lengths along with the flexibility of flipping back to 1024 in the event there are problems caused by any DNS providers who still don’t support 2048\. *When you create a new identity, it will be created with DKIM 2048 by default unless you specify 1024\.*

To preserve the deliverability of in transit emails, there are restrictions on the frequency at which you can change you DKIM key length\. Restrictions include:
+ Not being able to switch to to the same key length as is already configured\.
+ Not being able to switch to different key length more than once in a 24 hour period \(unless it’s the first downgrade to 1024 in that period\)\.

When your email is in transit, DNS is using your public key to authenticate your email; therefore, if you change keys too quickly or frequently, DNS may not be able to DKIM authenticate your email as the former key may already be invalidated, thus, these restrictions safeguard against that\.

## DKIM considerations<a name="send-email-authentication-dkim-easy-considerations"></a>

When you use DKIM to authenticate your email, the following rules apply:
+ You only need to set up DKIM for the domain that you use in your "From" address\. You don't need to set up DKIM for domains that you use in "Return\-Path" or "Reply\-to" addresses\.
+ Amazon SES is available in several AWS Regions\. If you use more than one AWS Region to send email, you have to complete the DKIM setup process in each of those Regions to ensure that all of your email is DKIM\-signed\.
+ Because DKIM properties are inherited from the parent domain, when you verify a domain with DKIM authentication:
  + DKIM authentication will also apply to all subdomains of that domain\.
    + DKIM settings for a subdomain can override the settings for the parent domain by disabling the inheritance if you don't want the subdomain to use DKIM authentication, as well as the ability to re\-enable later\.
  + DKIM authentication will also apply to all email sent from an email identity that references the DKIM verified domain in its address\.
    + DKIM settings for an email address can override the settings for the subdomain \(if applicable\) and the parent domain by disabling the inheritance if you want to send mail without DKIM authentication, as well as the ability to re\-enable later\.

## Understanding inherited DKIM signing properties<a name="dkim-easy-setup-email-key-points"></a>

It's important to first understand that an email address identity inherits its DKIM signing properties from its parent domain if that domain was configured with DKIM, regardless of whether Easy DKIM or BYODKIM was used\. Therefore, disabling or enabling DKIM signing on the email address identity, is in effect, overriding the domain's DKIM signing properties based on these key facts:
+ If you already set up DKIM for the domain that an email address belongs to, you do not need to enable DKIM signing for the email address identity as well\.
  + When you set up DKIM for a domain, Amazon SES automatically authenticates every email from every address on that domain through the inherited DKIM properties from the parent domain\.
+ DKIM settings for a specific email address identity *automatically override the settings of the parent domain or subdomain \(if applicable\)* that the address belongs to\.

Since the email address identity's DKIM signing properties are inherited from the parent domain, if you're planning on overriding these properties, you must keep in mind the hierarchical rules of overriding as explained in the table below\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/ses/latest/dg/send-email-authentication-dkim.html)

It’s generally never recommended to disable your DKIM signing as it risks tarnishing your sender reputation, and it increases the risk of having your sent mail go to junk or spam folders or having your domain spoofed\.

However, the capability exists to override the domain inherited DKIM signing properties on an email address identity for any particular use case or outlying business decision that you might have to either permanently or temporarily disable DKIM signing, or to re\-enable it at a later time\. See [Overriding inherited DKIM signing on an email address identity](send-email-authentication-dkim-easy-managing.md#send-email-authentication-dkim-easy-setup-email)\.