# Provide your own DKIM authentication token \(BYODKIM\) in Amazon SES<a name="send-email-authentication-dkim-bring-your-own"></a>

As an alternative to using [Easy DKIM](send-email-authentication-dkim-easy.md), you can instead configure DKIM authentication by using your own public\-private key pair\. This process is known as *Bring Your Own DKIM* \(*BYODKIM*\)\.

With BYODKIM, you can use a single DNS record to configure DKIM authentication for your domains, as opposed to Easy DKIM, which requires you to publish three separate DNS records\. Additionally, with BYODKIM you can rotate the DKIM keys for your domains as often as you want\.

**Topics**
+ [Step 1: Create the key pair](#send-email-authentication-dkim-bring-your-own-create-key-pair)
+ [Step 2: Add the selector and public key to your DNS provider's domain configuration](#send-email-authentication-dkim-bring-your-own-update-dns)
+ [Step 3: Configure and verify a domain to use BYODKIM](#send-email-authentication-dkim-bring-your-own-configure-identity)

**Warning**  
If you currently have Easy DKIM enabled and are transitioning over to BYODKIM, be aware that Amazon SES will not use Easy DKIM to sign your emails while BYODKIM is being set up and your DKIM status is in a pending state\. Between the moment you make the call to enable BYODKIM \(either through the API or console\) and the moment when SES can confirm your DNS configuration, your emails may be sent by SES without a DKIM signature\. Therefore, it is advised to use an intermediary step to migrate from one DKIM signing method to the other \(e\.g\., using a subdomain of your domain with Easy DKIM enabled and then deleting it once BYODKIM verification has passed\), or perform this activity during your application's downtime, if any\.

## Step 1: Create the key pair<a name="send-email-authentication-dkim-bring-your-own-create-key-pair"></a>

To use the Bring Your Own DKIM feature, you first have to create an RSA key pair\.

The private key that you generate must use at least 1024\-bit RSA encryption and up to 2048\-bit, and be encoded using base64 [\(PEM\)](https://en.wikipedia.org/wiki/Privacy-Enhanced_Mail) encoding\. See [DKIM signing key length](send-email-authentication-dkim.md#send-email-authentication-dkim-1024-2048) to learn more about DKIM signing key lengths and how to change them\.

**Note**  
You can use third\-party applications and tools to generate RSA key pairs as long as the private key is generated with at least 1024\-bit RSA encryption and up to 2048\-bit, and is encoded using base64 [\(PEM\)](https://en.wikipedia.org/wiki/Privacy-Enhanced_Mail) encoding\.

In the following procedure, the example code which uses the `openssl genrsa` command that's built into most Linux, macOS, or Unix operating systems to create the key pair will automatically use base64 [\(PEM\)](https://en.wikipedia.org/wiki/Privacy-Enhanced_Mail) encoding\.

**To create the key pair from the Linux, macOS, or Unix command line**

1. At the command line, enter the following command to generate the private key replacing *nnnn* with a bit length of at least 1024 and up to 2048:

   ```
   openssl genrsa -f4 -out private.key nnnn
   ```

1. At the command line, enter the following command to generate the public key:

   ```
   openssl rsa -in private.key -outform PEM -pubout -out public.key
   ```

## Step 2: Add the selector and public key to your DNS provider's domain configuration<a name="send-email-authentication-dkim-bring-your-own-update-dns"></a>

Now that you've created a key pair, you have to add the public key as a TXT record to the DNS configuration for your domain\.

**To add the public key to the DNS configuration for your domain**

1. Sign in to the management console for your DNS or hosting provider\.

1. Add a new text record to the DNS configuration for your domain\. The record should use the following format:    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/ses/latest/dg/send-email-authentication-dkim-bring-your-own.html)

   In the preceding example, make the following changes:
   + Replace *selector* with a unique name that identifies the key\.
**Note**  
A small number of DNS providers don't allow you to include underscores \(\_\) in record names\. However, the underscore in the DKIM record name is required\. If your DNS provider doesn't allow you to enter an underscore in the record name, contact the provider's customer support team for assistance\.
   + Replace *example\.com* with your domain\.
   + Replace *yourPublicKey* with the public key that you created earlier and include the `p=` prefix as shown in the *Value* column above\.
**Note**  
When you publish \(add\) your public key to your DNS provider, it must be formatted as follows:  
You have to delete the first and last lines \(`-----BEGIN PUBLIC KEY-----` and `-----END PUBLIC KEY-----`, respectively\) of the generated public key\. Additionally, you have to remove the line breaks in the generated public key\. The resulting value is a string of characters with no spaces or line breaks\.
You must include the `p=` prefix as shown in the *Value* column in the table above\.

   Different providers have different procedures for updating DNS records\. The following table includes links to the documentation for a few widely used DNS providers\. This list isn't exhaustive and doesn't signify endorsement; likewise, if your DNS provider isn't listed, it doesn't imply you can't use the domain with Amazon SES\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/ses/latest/dg/send-email-authentication-dkim-bring-your-own.html)

## Step 3: Configure and verify a domain to use BYODKIM<a name="send-email-authentication-dkim-bring-your-own-configure-identity"></a>

You can set up BYODKIM for both new domains \(that is, domains that you don't currently use to send email through Amazon SES\) and existing domains \(that is, domains that you've already set up to use with Amazon SES\) by using either the console or AWS CLI\. Before you use the AWS CLI procedures in this section, you first have to install and configure the AWS CLI\. For more information, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\.\.

### Option 1: Creating a new domain identity that uses BYODKIM<a name="send-email-authentication-dkim-bring-your-own-configure-identity-new-domain"></a>

This section contains procedures for creating a new domain identity that uses BYODKIM\. A new domain identity is a domain that you haven't previously set up to send email using Amazon SES\.

If you want to configure an existing domain to use BYODKIM, complete the procedure in [Option 2: Configuring an existing domain identity](#send-email-authentication-dkim-bring-your-own-configure-identity-existing-domain) instead\.

**To create an identity using BYODKIM from the console**
+ Follow the procedures in [Creating a domain identity](creating-identities.md#verify-domain-procedure), and when you get to Step 8, follow the BYODKIM specific instructions\.

**To create an identity using BYODKIM from the AWS CLI**

To configure a new domain, use the `CreateEmailIdentity` operation in the Amazon SES API\.

1. In a text editor, paste the following code:

   ```
   {
       "EmailIdentity":"example.com",
       "DkimSigningAttributes":{
           "DomainSigningPrivateKey":"privateKey",
           "DomainSigningSelector":"selector"
       }
   }
   ```

   In the preceding example, make the following changes:
   + Replace *example\.com* with the domain that you want to create\.
   + Replace *privateKey* with your private key\.
**Note**  
You have to delete the first and last lines \(`-----BEGIN PRIVATE KEY-----` and `-----END PRIVATE KEY-----`, respectively\) of the generated private key\. Additionally, you have to remove the line breaks in the generated private key\. The resulting value is a string of characters with no spaces or line breaks\.
   + Replace *selector* with the unique selector that you specified when you created the TXT record in the DNS configuration for your domain\.

   When you finish, save the file as `create-identity.json`\.

1. At the command line, enter the following command:

   ```
   aws sesv2 create-email-identity --cli-input-json file://path/to/create-identity.json
   ```

   In the preceding command, replace *path/to/create\-identity\.json* with the complete path to the file that you created in the previous step\.

### Option 2: Configuring an existing domain identity<a name="send-email-authentication-dkim-bring-your-own-configure-identity-existing-domain"></a>

This section contains procedures for updating an existing domain identity to use BYODKIM\. An existing domain identity is a domain that you have already set up to send email using Amazon SES\.

**To update a domain identity using BYODKIM from the console**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane, under **Configuration**, choose **Verified identities**\.

1. In the list of identities, choose an identity where the **Identity type** is *Domain*\.
**Note**  
If you need to create or verify a domain, see [Creating a domain identity](creating-identities.md#verify-domain-procedure)\.

1. Under the **Authentication** tab, in the **DomainKeys Identified Mail \(DKIM\)** pane, choose **Edit**\.

1. In the **Advanced DKIM settings** pane, choose the **Provide DKIM authentication token \(BYODKIM\)** button in the **Identity type** field\.

1. For **Private key**, paste the private key you generated earlier\.
**Note**  
You have to delete the first and last lines \(`-----BEGIN PRIVATE KEY-----` and `-----END PRIVATE KEY-----`, respectively\) of the generated private key\. Additionally, you have to remove the line breaks in the generated private key\. The resulting value is a string of characters with no spaces or line breaks\.

1. For **Selector name**, enter the name of the selector that you specified in your domain’s DNS settings\.

1. In the **DKIM signatures** field, check the **Enabled** box\.

1. Choose **Save changes**\.

**To update a domain identity using BYODKIM from the AWS CLI**

To configure an existing domain, use the `PutEmailIdentityDkimSigningAttributes` operation in the Amazon SES API\.

1. In a text editor, paste the following code:

   ```
   {
       "SigningAttributes":{
           "DomainSigningPrivateKey":"privateKey",
           "DomainSigningSelector":"selector"
       },
       "SigningAttributesOrigin":"EXTERNAL"
   }
   ```

   In the preceding example, make the following changes:
   + Replace *privateKey* with your private key\.
**Note**  
You have to delete the first and last lines \(`-----BEGIN PRIVATE KEY-----` and `-----END PRIVATE KEY-----`, respectively\) of the generated private key\. Additionally, you have to remove the line breaks in the generated private key\. The resulting value is a string of characters with no spaces or line breaks\.
   + Replace *selector* with the unique selector that you specified when you created the TXT record in the DNS configuration for your domain\.

   When you finish, save the file as `update-identity.json`\.

1. At the command line, enter the following command:

   ```
   aws sesv2 put-email-identity-dkim-signing-attributes --email-identity example.com --cli-input-json file://path/to/update-identity.json
   ```

   In the preceding command, make the following changes:
   + Replace *path/to/update\-identity\.json* with the complete path to the file that you created in the previous step\.
   + Replace *example\.com* with the domain that you want to update\.

### Verifying the DKIM status for a domain that uses BYODKIM<a name="send-email-authentication-dkim-bring-your-own-configure-identity-check"></a>

**To verify the DKIM status of a domain from the console**

After you configure a domain to use BYODKIM, you can use the SES console to verify that DKIM is properly configured\.

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane, under **Configuration**, choose **Verified identities**\.

1. In the list of identities, choose the identity whose DKIM status you want to verify\.

1. It can take up to 72 hours for changes to DNS settings to propagate\. As soon as Amazon SES detects all of the required DKIM records in your domain’s DNS settings, the verification process is complete\. If everything has been configured correctly, your domain’s **DKIM configuration** field displays **Successful** in the **DomainKeys Identified Mail \(DKIM\)** pane, and the **Identity status** field displays **Verified** in the **Summary** pane\.

**To verify the DKIM status of a domain using the AWS CLI**

After you configure a domain to use BYODKIM, you can use the GetEmailIdentity operation to verify that DKIM is properly configured\.
+ At the command line, enter the following command:

  ```
  aws sesv2 get-email-identity --email-identity example.com
  ```

  In the preceding command, replace *example\.com* with your domain\.

  This command returns a JSON object that contains a section that resembles the following example\.

  ```
  {
      ...
      "DkimAttributes": { 
          "SigningAttributesOrigin": "EXTERNAL",
          "SigningEnabled": true,
          "Status": "SUCCESS",
          "Tokens": [ ]
      },
      ...
  }
  ```

  If all of the following are true, BYODKIM is properly configured for the domain:
  + The value of the `SigningAttributesOrigin` property is `EXTERNAL`\.
  + The value of `SigningEnabled` is `true`\.
  + The value of `Status` is `SUCCESS`\.