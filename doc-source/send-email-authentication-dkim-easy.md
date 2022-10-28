# Easy DKIM in Amazon SES<a name="send-email-authentication-dkim-easy"></a>

When you set up Easy DKIM for a domain identity, Amazon SES automatically adds a 2048\-bit DKIM key to every email that you send from that identity\. You can configure Easy DKIM by using the Amazon SES console, or by using the API\.

**Note**  
To set up Easy DKIM, you have to modify the DNS settings for your domain\. If you use Route 53 as your DNS provider, Amazon SES can automatically create the appropriate records for you\. If you use another DNS provider, see your provider's documentation to learn more about changing the DNS settings for your domain\.

**Warning**  
If you currently have BYODKIM enabled and are transitioning over to Easy DKIM, be aware that Amazon SES will not use BYODKIM to sign your emails while Easy DKIM is being set up and your DKIM status is in a pending state\. Between the moment you make the call to enable Easy DKIM \(either through the API or console\) and the moment when SES can confirm your DNS configuration, your emails may be sent by SES without a DKIM signature\. Therefore, it is advised to use an intermediary step to migrate from one DKIM signing method to the other \(e\.g\., using a subdomain of your domain with BYODKIM enabled and then deleting it once Easy DKIM verification has passed\), or perform this activity during your application's downtime, if any\.

## Setting up Easy DKIM for a verified domain identity<a name="send-email-authentication-dkim-easy-setup-domain"></a>

The procedure in this section is streamlined to just show the steps necessary to configure Easy DKIM on a domain identity that you've already created\. If you haven't yet created a domain identity or you want to see all available options for customizing a domain identity, such as using a default configuration set, custom MAIL FROM domain, and tags, see [Creating a domain identity](creating-identities.md#verify-domain-procedure)\. 

Part of creating an Easy DKIM domain identity is configuring its DKIM\-based verification where you will have the choice to either accept the Amazon SES default of 2048 bits, or to override the default by selecting 1024 bits\. See [DKIM signing key length](send-email-authentication-dkim.md#send-email-authentication-dkim-1024-2048) to learn more about DKIM signing key lengths and how to change them\.

**To set up Easy DKIM for a domain**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane, under **Configuration**, choose **Verified identities**\.

1. In the list of identities, choose an identity where the **Identity type** is *Domain*\.
**Note**  
If you need to create or verify a domain, see [Creating a domain identity](creating-identities.md#verify-domain-procedure)\.

1. Under the **Authentication** tab, in the **DomainKeys Identified Mail \(DKIM\)** container, choose **Edit**\.

1. In the **Advanced DKIM settings** container, choose the **Easy DKIM** button in the **Identity type** field\.

1. In the **DKIM signing key length** field, choose either [**RSA\_2048\_BIT** or **RSA\_1024\_BIT**](send-email-authentication-dkim.md#send-email-authentication-dkim-1024-2048)\.

1. In the **DKIM signatures** field, check the **Enabled** box\.

1. Choose **Save changes**\.

1. Now that you’ve configured your domain identity with Easy DKIM, you must complete the verification process with your DNS provider \- proceed to [Verifying a DKIM domain identity with your DNS provider](creating-identities.md#just-verify-domain-proc) and follow the DNS authentication procedures for Easy DKIM\.

## Change the Easy DKIM signing key length for an identity<a name="send-email-authentication-dkim-easy-managing-change-key-length"></a>

The procedure in this section shows how you can easily change the Easy DKIM bits required for the signing algorithm\. While a signing length of 2048 bits is always preferred for the enhanced security it provides, there may be situations that require you to use the 1024 bit length, such as having to use a DNS provider who only supports DKIM 1024\.

To preserve the deliverability of in transit emails, there are restrictions on the frequency at which you can change or flip your DKIM key length\.

When your email is in transit, DNS is using your public key to authenticate your email; therefore, if you change keys too quickly or frequently, DNS may not be able to DKIM authenticate your email as the former key may already be invalidated, thus, the following restrictions safeguard against that:
+ You can't switch to the same key length as is already configured\.
+ You can't switch to a different key length more than once in a 24 hour period \(unless it’s the first downgrade to 1024 in that period\)\.

In using the following procedures to change your key length, if you violate one of these restrictions, the console will return an error banner stating that *the input you provided is invalid* along with the reason of why it was invalid\.

**To change the DKIM signing key length bits**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane, under **Configuration**, choose **Verified identities**\.

1. In the list of identities, choose the identity you want to change the DKIM signing key length for\.

1. Under the **Authentication** tab, in the **DomainKeys Identified Mail \(DKIM\)** container, choose **Edit**\.

1. In the **Advanced DKIM settings** container, choose either [**RSA\_2048\_BIT** or **RSA\_1024\_BIT**](send-email-authentication-dkim.md#send-email-authentication-dkim-1024-2048) in the **DKIM signing key length** field\.

1. Choose **Save changes**\.