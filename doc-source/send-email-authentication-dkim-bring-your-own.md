# Provide Your Own DKIM Authentication Token in Amazon SES<a name="send-email-authentication-dkim-bring-your-own"></a>

As an alternative to using [Easy DKIM](send-email-authentication-dkim-easy.md), you can instead configure DKIM authentication by using your own public\-private key pair\. This process is known as *Bring Your Own DKIM* \(*BYODKIM*\)\.

With BYODKIM, you can use a single DNS record to configure DKIM authentication for your domains, as opposed to Easy DKIM, which requires you to publish three separate DNS records\. Additionally, using BYODKIM lets you rotate the DKIM keys for your domains as often as you want\.

**Topics**
+ [Step 1: Create the Key Pair](#send-email-authentication-dkim-bring-your-own-create-key-pair)
+ [Step 2: Add the Public Key to the DNS Configuration for Your Domain](#send-email-authentication-dkim-bring-your-own-update-dns)
+ [Step 3: Configure a Domain to Use BYODKIM](#send-email-authentication-dkim-bring-your-own-configure-identity)

## Step 1: Create the Key Pair<a name="send-email-authentication-dkim-bring-your-own-create-key-pair"></a>

To use the Bring Your Own DKIM feature, you first have to create a key pair\.

The private key that you generate has to use 1024\-bit RSA encoding\. The private key has to be in PKCS \#1 format\. 

This section shows you how to use the `openssl` command that's built in to most Linux, macOS, or Unix operating systems to create the key pair\.

**Note**  
If you use a Windows computer, you can use third\-party applications to generate RSA key pairs\. If you use a third\-party application, it has to be able to generate a 1024\-bit RSA key pair in PKCS \#1 format\.

**To create the key pair from the Linux, macOS, or Unix command line**

1. At the command line, enter the following command to generate the private key:

   ```
   openssl genrsa -f4 -out private.key 1024
   ```

1. At the command line, enter the following command to generate the public key:

   ```
   openssl rsa -in private.key -outform PEM -pubout -out public.key
   ```

## Step 2: Add the Public Key to the DNS Configuration for Your Domain<a name="send-email-authentication-dkim-bring-your-own-update-dns"></a>

Now that you've created a key pair, you have to add the public key to the DNS configuration for your domain as a TXT record\.

**To add the public key to the DNS configuration for your domain**

1. Sign in to the management console for your DNS or hosting provider\.

1. Add a new text record to the DNS configuration for your domain\. The record should use the following format:    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/send-email-authentication-dkim-bring-your-own.html)

   In the preceding example, make the following changes:
   + Replace *selector* with a unique name that identifies the key\.
   + Replace *example\.com* with your domain\.
   + Replace *yourPublicKey* with the public key that you created earlier\.
**Note**  
You have to delete the first and last lines \(`-----BEGIN PUBLIC KEY-----` and `-----END PUBLIC KEY-----`, respectively\) of the generated public key\. Additionally, you have to remove the line breaks in the generated public key\. The resulting value is a string of characters with no spaces or line breaks\.

   Different providers have different procedures for updating DNS records\. The following table lists links to the documentation for several common providers\. This list isn't exhaustive and inclusion in this list isn’t an endorsement or recommendation of any company's products or services\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/send-email-authentication-dkim-bring-your-own.html)

## Step 3: Configure a Domain to Use BYODKIM<a name="send-email-authentication-dkim-bring-your-own-configure-identity"></a>

You can set up BYODKIM for both new domains \(that is, domains that you don't currently use to send email through Amazon SES\) and existing domains \(that is, domains that you've already set up to use with Amazon SES\)\. To set up a new domain, use the `CreateEmailIdentity` operation in the Amazon SES API\. To configure an existing domain, use the `PutEmailIdentityDkimSigningAttributes` operation\.

This section includes procedures for setting up new and existing domains by using the AWS CLI\. Before you complete the procedures in this section, you first have to install and configure the AWS CLI\. For more information, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\.

### Option 1: Creating a New Domain Identity That Uses BYODKIM<a name="send-email-authentication-dkim-bring-your-own-configure-identity-new-domain"></a>

This section contains a procedure for creating a new domain identity that uses BYODKIM\. A new domain identity is a domain that you haven't previously set up to send email using Amazon SES\.

If you want to configure an existing domain to use BYODKIM, complete the procedure in [Option 2: Configuring an Existing Domain Identity](#send-email-authentication-dkim-bring-your-own-configure-identity-existing-domain) instead\.

**To create the identity**

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

### Option 2: Configuring an Existing Domain Identity<a name="send-email-authentication-dkim-bring-your-own-configure-identity-existing-domain"></a>

This section contains a procedure for updating an existing domain identity to use BYODKIM\. A an existing domain identity is a domain that you have already set up to send email using Amazon SES\.

**To update the domain identity**

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

### Checking the DKIM Status for a Domain That Uses BYODKIM<a name="send-email-authentication-dkim-bring-your-own-configure-identity-check"></a>

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

  BYODKIM is properly configured for the domain if all of the following are true:
  + The value of the `SigningAttributesOrigin` property is `EXTERNAL`\.
  + The value of `SigningEnabled` is `true`\.
  + The value of `Status` is `SUCCESS`\.