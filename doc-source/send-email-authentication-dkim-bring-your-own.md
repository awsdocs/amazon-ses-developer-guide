# Provide your own DKIM authentication token \(BYODKIM\) in Amazon SES<a name="send-email-authentication-dkim-bring-your-own"></a>

As an alternative to using [Easy DKIM](send-email-authentication-dkim-easy.md), you can instead configure DKIM authentication by using your own public\-private key pair\. This process is known as *Bring Your Own DKIM* \(*BYODKIM*\)\.

With BYODKIM, you can use a single DNS record to configure DKIM authentication for your domains, as opposed to Easy DKIM, which requires you to publish three separate DNS records\. Additionally, with BYODKIM you can rotate the DKIM keys for your domains as often as you want\.

**Topics**
+ [Step 1: Create the key pair](#send-email-authentication-dkim-bring-your-own-create-key-pair)
+ [Step 2: Add the public key to the DNS configuration for your domain](#send-email-authentication-dkim-bring-your-own-update-dns)
+ [Step 3: Configure a domain to use BYODKIM](#send-email-authentication-dkim-bring-your-own-configure-identity)

## Step 1: Create the key pair<a name="send-email-authentication-dkim-bring-your-own-create-key-pair"></a>

To use the Bring Your Own DKIM feature, you first have to create a key pair\.

The private key that you generate must use at least 1024\-bit RSA encryption and up to 2048\-bit, and be encoded using base64 encoding\. See [DKIM signing key length](send-email-authentication-dkim.md#send-email-authentication-dkim-1024-2048) to learn more about DKIM signing key lengths and how to change them\.

This section shows you how to use the `openssl` command that's built in to most Linux, macOS, or Unix operating systems to create the key pair\.

**Note**  
If you use a Windows computer, you can use third\-party applications to generate RSA key pairs\. If you use a third\-party application, it has to be able to generate at least a 1024\-bit RSA key pair\.

**To create the key pair from the Linux, macOS, or Unix command line**

1. At the command line, enter the following command to generate the private key replacing *nnnn* with a bit length of at least 1024 and up to 2048:

   ```
   openssl genrsa -f4 -out private.key nnnn
   ```

1. At the command line, enter the following command to generate the public key:

   ```
   openssl rsa -in private.key -outform PEM -pubout -out public.key
   ```

## Step 2: Add the public key to the DNS configuration for your domain<a name="send-email-authentication-dkim-bring-your-own-update-dns"></a>

Now that you've created a key pair, you have to add the public key as a TXT record to the DNS configuration for your domain\.

**To add the public key to the DNS configuration for your domain**

1. Sign in to the management console for your DNS or hosting provider\.

1. Add a new text record to the DNS configuration for your domain\. The record should use the following format:    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/ses/latest/dg/send-email-authentication-dkim-bring-your-own.html)

   In the preceding example, make the following changes:
   + Replace *selector* with a unique name that identifies the key\.
   + Replace *example\.com* with your domain\.
   + Replace *yourPublicKey* with the public key that you created earlier\.
**Note**  
You have to delete the first and last lines \(`-----BEGIN PUBLIC KEY-----` and `-----END PUBLIC KEY-----`, respectively\) of the generated public key\. Additionally, you have to remove the line breaks in the generated public key\. The resulting value is a string of characters with no spaces or line breaks\.

   Different providers have different procedures for updating DNS records\. The following table lists links to the documentation for several common providers\. This list isn't exhaustive and inclusion in this list isn’t an endorsement or recommendation of any company's products or services\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/ses/latest/dg/send-email-authentication-dkim-bring-your-own.html)

## Step 3: Configure a domain to use BYODKIM<a name="send-email-authentication-dkim-bring-your-own-configure-identity"></a>

You can set up BYODKIM for both new domains \(that is, domains that you don't currently use to send email through Amazon SES\) and existing domains \(that is, domains that you've already set up to use with Amazon SES\) by using either the console or AWS CLI\. Before you use the AWS CLI procedures in this section, you first have to install and configure the AWS CLI\. For more information, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\.\.

### Option 1: Creating a new domain identity that uses BYODKIM<a name="send-email-authentication-dkim-bring-your-own-configure-identity-new-domain"></a>

This section contains procedures for creating a new domain identity that uses BYODKIM\. A new domain identity is a domain that you haven't previously set up to send email using Amazon SES\.

If you want to configure an existing domain to use BYODKIM, complete the procedure in [Option 2: Configuring an existing domain identity](#send-email-authentication-dkim-bring-your-own-configure-identity-existing-domain) instead\.

**To create an identity using BYODKIM from the console**
+ Follow the procedures in [Creating and verifying a domain identity](creating-identities.md#verify-domain-procedure) and follow the BYODKIM option in the configure domain step\.

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
If you need to create or verify a domain, see [Creating and verifying a domain identity](creating-identities.md#verify-domain-procedure)\.

1. Under the **Authentication** tab, in the **DomainKeys Identified Mail \(DKIM\)** container, choose **Edit**\.

1. In the **Advanced DKIM settings** container, choose the **Provide DKIM authentication token** button in the **Identity type** field\.

1. For **Private key**, paste the private key\. The private key must use [at least 1024\-bit RSA encryption and up to 2048\-bit](send-email-authentication-dkim.md#send-email-authentication-dkim-1024-2048), and must be encoded using base64 encoding\.

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
   + Replace *selector* with the unique selector that you specified when you created the TXT record in the DNS configuration for your domain\.

   When you finish, save the file as `update-identity.json`\.

1. At the command line, enter the following command:

   ```
   aws sesv2 put-email-identity-dkim-signing-attributes --email-identity example.com --cli-input-json file://path/to/update-identity.json
   ```

   In the preceding command, make the following changes:
   + Replace *path/to/update\-identity\.json* with the complete path to the file that you created in the previous step\.
   + Replace *example\.com* with the domain that you want to update\.

### Checking the DKIM status for a domain that uses BYODKIM<a name="send-email-authentication-dkim-bring-your-own-configure-identity-check"></a>

After you configure a domain to use BYODKIM, you can use the GetEmailIdentity operation to confirm that DKIM is properly configured\.

**To check the DKIM status of a domain**
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