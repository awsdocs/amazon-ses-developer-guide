# Deleting an email identity in Amazon SES<a name="delete-email-addresses-procedure"></a>

If you no longer need to use a verified email address, you can delete it by using the Amazon SES console or the [DeleteIdentity](https://docs.aws.amazon.com/ses/latest/APIReference/API_DeleteIdentity.html) API operation\.

**Warning**  
This action can't be undone\. However, you can repeat the verification process for an identity that was previously deleted\.

## Deleting an email identity in Amazon SES<a name="delete-email-addresses-procedure-console"></a>

**To remove verified email addresses**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the console, use the region selector to choose the AWS Region where you want to delete an email identity, as shown in the following image\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/verify-email-address-region.png)
**Note**  
These procedures only delete the email address in the selected AWS Region\. To delete an email address that was verified in more than one region, repeat the procedure in this section for each region\.

1. Select each email address that you want to remove, and then choose **Remove**\.

## Deleting an email identity using the Amazon SES API<a name="delete-email-addresses-procedure-api"></a>

Use the [DeleteIdentity](https://docs.aws.amazon.com/ses/latest/APIReference/API_DeleteIdentity.html) API operation to delete email address and domain identities\.

To delete an identity using the AWS CLI, type the following command at the command line: aws ses delete\-identity \-\-identity "*sender@example\.com*"

Replace *sender@example\.com* in the preceding command with the identity that you want to delete\.
