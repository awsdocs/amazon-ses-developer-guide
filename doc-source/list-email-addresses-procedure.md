# Listing email identities in Amazon SES<a name="list-email-addresses-procedure"></a>

You can display a list of email identities by using the Amazon SES console or the [ListIdentities](https://docs.aws.amazon.com/ses/latest/APIReference/API_ListIdentities.html) API operation\.

## Viewing a list of email identities in Amazon SES<a name="list-email-addresses-procedure-console"></a>

You can use the Amazon SES console and API to view a list of email addresses that are verified or are pending verification, as well as those that failed the verification process\.

**To view a list of verified email addresses**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the console, use the region selector to choose the AWS Region where you want to list email identities, as shown in the following image\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/verify-email-address-region.png)
**Note**  
These procedures only display a list of email addresses for the selected AWS Region\.

1. In the navigation pane, under **Identity Management**, choose **Email Addresses**\.

   The Email Addresses page displays a list of email addresses that are verified, that are pending verification, and that failed the verification process\. Click an email address to view additional information about it\.

## Viewing a list of email identities using the Amazon SES API<a name="list-email-addresses-procedure-console"></a>

Use the [ListIdentities](https://docs.aws.amazon.com/ses/latest/APIReference/API_ListIdentities.html) API operation to view a list of all email identities, regardless of their statuses\. You can also use the [GetIdentityVerificationAttributes](https://docs.aws.amazon.com/ses/latest/APIReference/API_GetIdentityVerificationAttributes.html) operation to find the verification status of a given identity\.

To view a list of identities by using the AWS CLI, type the following command at the command line: aws ses list\-identities

When you execute the `ListIdentities` operation, it returns a list of all of the identities in your Amazon SES account, regardless of their verification statuses\. To see the verification status for one or more identities, use the `GetIdentityVerificationAttributes` operation\. To find the verification status of an identity using the AWS CLI, type the following command at the command line: aws ses get\-identity\-verification\-attributes \-\-identities "*sender@example\.com*"

Replace *sender@example\.com* in the preceding command with the identity that you want to find the verification status of\. You can also use this command to find the verification statuses of multiple identities in a single API call\. For example, to find the status of the domain *example\.com* and the email address *sender@example\.co\.uk*, type the following command: aws ses get\-identity\-verification\-attributes \-\-identities "example\.com" "sender@example\.co\.uk"
