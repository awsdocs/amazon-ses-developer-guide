# Managing Amazon SES default configuration sets<a name="managing-configuration-sets-default"></a>

You can add a default configuration set to an email identity when you create the identity, or you can add a default configuration set to an existing identity\. You can apply default configuration sets to both email address and domain identities\.

A default configuration set automatically applies its rules to all messages that you send from the email identity associated with that configuration set\.

**Default configuration set considerations**
+ The configuration set must be created first before associating it with an identity\.
+ Default configuration sets will only be applied if the identity is verified\.
+ An email identity can be associated with only one configuration set at a time\. However, you can apply the same configuration set to multiple identities\.
+ A default configuration set at the email address level overrides a default configuration set at the domain level\. For example, a default configuration set associated with *joe@example\.com* overrides the configuration set for the domain of *example\.com*\.
+ A default configuration set at the domain level applies to all email addresses for that domain \(unless you verify specific addresses for the domain\)\.
+ If you delete a configuration set that's designated as the default configuration set for an identity, and then attempt to send email through that identity, your call to Amazon SES fails with a "bad request" error\.

## Creating and verifying an identity with a default configuration set using the Amazon SES API<a name="managing-configuration-sets-default-creating"></a>

You can use the [CreateEmailIdentity](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_CreateEmailIdentity.html) operation in the Amazon SES API v2 to create a new email identity and set its default configuration set at the same time\.

**Note**  
Before you complete the procedure in this section, you have to install and configure the AWS CLI\. For more information, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\.

**To set a default configuration set using the AWS CLI**
+ At the command line, enter the following command to use the [CreateEmailIdentity](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_CreateEmailIdentity.html) operation\.

```
aws sesv2 create-email-identity --email-identity ADDRESS-OR-DOMAIN --configuration-set-name CONFIG-SET
```

In the preceding commands, replace *ADDRESS\-OR\-DOMAIN* with the email identity that you want to verify\. Replace *CONFIG\-SET* with the name of the configuration set you want to set as the default configuration set for the identity\.

If the command executes successfully, it exits without providing any output\.

**To verify your email address**

1. Check the inbox for the email address that you're verifying\. You'll receive a message with the following subject line: "Amazon Web Services \- Email Address Verification Request in region *RegionName*," where *RegionName* is the name of the AWS Region that you attempted to verify the email address in\.

   Open the message, and then click the link in it\.
**Note**  
The link in the verification message expires 24 hours after the message was sent\. If 24 hours have passed since you received the verification email, repeat steps 1–5 to receive a verification email with a valid link\.

1. In the Amazon SES console, under **Identity Management**, choose **Email Addresses**\. In the list of email addresses, locate the email address you're verifying\. If the email address was verified, the value in the **Status** column is "verified"\.

**To verify your domain**

To verify your domain, see [Verifying a domain with Amazon SES](verify-domain-procedure.md) for more information\.

## Adding a default configuration set to an existing email identity using the Amazon SES API<a name="managing-configuration-sets-default-adding"></a>

You can use the [PutEmailIdentityConfigurationSetAttributes](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_PutEmailIdentityConfigurationSetAttributes.html) operation to add or remove a default configuration set from an existing email identity\.

**Note**  
Before you complete the procedure in this section, you have to install and configure the AWS CLI\. For more information, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\.

**To add a default configuration set using the AWS CLI**
+ At the command line, enter the following command to use the [PutEmailIdentityConfigurationSetAttributes](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_PutEmailIdentityConfigurationSetAttributes.html) operation\.

```
aws sesv2 put-email-identity-configuration-set-attributes --email-identity ADDRESS-OR-DOMAIN --configuration-set-name CONFIG-SET
```

In the preceding commands, replace *ADDRESS\-OR\-DOMAIN* with the email identity that you want to verify\. Replace *CONFIG\-SET* with the name of the configuration set you wish to set as the identity's default configuration set\.

If the command executes successfully, it exits without providing any output\.

**To remove a default configuration set using the AWS CLI**
+ At the command line, enter the following command to use the [PutEmailIdentityConfigurationSetAttributes](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_PutEmailIdentityConfigurationSetAttributes.html) operation\.

```
aws sesv2 put-email-identity-configuration-set-attributes --email-identity ADDRESS-OR-DOMAIN
```

In the preceding commands, replace *ADDRESS\-OR\-DOMAIN* with the email identity that you want to verify\.

If the command executes successfully, it exits without providing any output\.

## Returning a default configuration set using the Amazon SES API<a name="managing-configuration-sets-default-returning"></a>

You can use the [GetEmailIdentity](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_GetEmailIdentity.html) operation to return the default configuration set for an email identity, if applicable\.

**Note**  
Before you complete the procedure in this section, you have to install and configure the AWS CLI\. For more information, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\.

**To return a default configuration set using the AWS CLI**
+ At the command line, enter the following command to use the [GetEmailIdentity](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_GetEmailIdentity.html) operation\.

```
aws sesv2 get-email-identity --email-identity ADDRESS-OR-DOMAIN
```

In the preceding commands, replace *ADDRESS\-OR\-DOMAIN* with with email identity for which you wish to know the default configuration set, if any\.

If the command executes successfully, it provides a JSON object with the email identity details\.

## Overriding a default configuration set using the Amazon SES API<a name="managing-configuration-sets-default-overriding"></a>

You can use the [SendEmail](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_SendEmail.html) operation to send email with a different configuration set\. If you do, the configuration set that you specify overrides the default configuration set for the identity\.

**Note**  
Before you complete the procedure in this section, you have to install and configure the AWS CLI\. For more information, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\.

**To override a default configuration set using the AWS CLI**
+ At the command line, enter the following command to use the [SendEmail](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_SendEmail.html) operation\.

```
aws sesv2 send-email --destination file://DESTINATION-JSON --content file://CONTENT-JSON --from-email-address ADDRESS-OR-DOMAIN --configuration-set-name CONFIG-SET
```

In the preceding commands, replace *DESTINATION\-JSON* with your destination JSON file, *CONTENT\-JSON* with your content JSON file, *ADDRESS\-OR\-DOMAIN* with your FROM email address, and *CONFIG\-SET* with the name of the configuration set you wish to use instead of the default configuration set for the identity\.

If the command executes successfully, it outputs a `MessageId`\.
