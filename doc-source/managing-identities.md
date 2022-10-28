# Managing identities in Amazon SES<a name="managing-identities"></a>

In the Amazon SES console, you can view a list of identities, open an identity to see and edit its detail settings, associate a default configuration set, or delete one or more identities\.

**Note**  
The procedures outlined in this section apply only to identities in the selected AWS Region\. To manage identities that were created in more than one Region, repeat the procedures for each AWS Region\.

## Viewing a list of identities in Amazon SES<a name="view-verified-domains"></a>

You can use the Amazon SES console or API to view a list of domain and email address identities that are verified or are pending verification\. You can also view those identifies for which verification was unsuccessful\.

**To view your domain and email address identities \(console\)**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the console, use the Region selector to choose the AWS Region for which you want to view your list of identities\.
**Note**  
This procedure only displays a list of identities for the selected AWS Region\.

1. In the navigation pane, under **Configuration**, choose **Verified identities**\. The **Loaded identities** table displays both domain and email address identities\. The **Status** column displays whether an identity has been verified, is pending verification, or has failed the verification process \- definitions of all possible status values are as follows:
   + **Verified** – your identity is successfully verified for sending in SES\.
   + **Failure** – SES was unable to verify your identity\. If it's a domain, it means SES was unable to detect the DNS records within 72 hours\. If it's an email address, it means the verification email that was sent to the email address was not acknowledged within 24 hours\.
   + **Pending** – SES is still trying to verify the identity\.
   + **Temporary Failure** – for a previously verified domain, SES will periodically check for the DNS record required for verification\. If at some point, SES is unable to detect the record, the status would change to *Temporary Failure*\. SES will recheck for the DNS record for 72 hours, and if it’s unable to detect the record, the domain status would change to *Failure*\. If it’s able to detect the record, the domain status would change to *Verified*\.
   + **Not started** – you have not yet started the verification process\.

1. To sort identities by verification status, choose the **Status** column\.

1. To view an identity’s details page, select the identity that you want to view\.

## Deleting an identity in Amazon SES<a name="remove-verified-domain"></a>

You can use the Amazon SES console or API to remove a domain or email address identity from your account in the selected AWS Region\.

**To remove a domain or email address identity \(console\)**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the console, use the Region selector to choose the AWS Region from which you want to delete one or more identities\.

1. In the navigation pane, under **Configuration**, choose **Verified identities**\. 

   The **Loaded identities** table displays a list of both domain and email address identities\.

1. In the **Identity** column, select the identity that you want to delete\. You can delete multiple identities by checking the box next to each identity that you want to delete\.

1. Choose **Delete**\.

## Editing an existing identity in Amazon SES<a name="edit-verified-domain"></a>

You can use the Amazon SES console or API to edit a domain or email address identity in your account in the selected AWS Region\.

**To edit a domain or email address identity \(console\)**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the console, use the Region selector to choose the AWS Region from which you want to edit one or more identities\.

1. In the navigation pane, under **Configuration**, choose **Verified identities**\. 

   The **Loaded identities** table displays a list of both domain and email address identities\.

1. In the **Identity** column, select the identity that you want to edit \(by clicking directly on the identity name as opposed to selecting its checkbox\)\.

1. On the identity's detail page, select the tab containing the categories you'd like to edit\.

1. In any of the selected tab's categorical containers, choose the **Edit** button of the attribute you wish to edit, make your changes, then choose **Save changes**\.

   1. If you wish to edit attributes under the **Authentication** tab and your domain identity is hosted in Amazon Route 53, and you haven't already published its DNS records, there will be a **Publish DNS records to Route53** button \(next to the **Edit** button\) in either or both of the **DomainKeys Identified Mail \(DKIM\)** or **Custom MAIL FROM domain** containers\.
**Note**  
The **Authentication** tab is only present when your account has a verified domain or an email address that uses a verified domain in your account\.

   1. You can publish the DNS records directly from the **Publish DNS records to Route53** button \- just click it, a confirmation banner will be displayed, and the **Publish DNS records to Route53** button will no longer be visible for the respective container\.

1. Repeat steps 5 & 6 for each attribute of the identity you'd like to edit\.

## Edit an identity to use a default configuration set using the API<a name="managing-configuration-sets-default-adding"></a>

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

## Retrieve the default configuration set used by the identity \(API\)<a name="managing-configuration-sets-default-returning"></a>

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

## Override the current default configuration set used by the identity \(API\)<a name="managing-configuration-sets-default-overriding"></a>

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