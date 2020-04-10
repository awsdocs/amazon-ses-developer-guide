# Deleting Personal Data from Amazon SES<a name="deleting-personal-data"></a>

Depending on how you use it, Amazon SES might store certain data that could be considered personal\. For example, in order to send email using Amazon SES, you must provide at least one verified identity \(an email address or a domain\)\. You can use the Amazon SES console or the Amazon SES API to permanently delete this personal data\.

This chapter provides procedures for deleting various types of data that might be considered personal\.

**Topics**
+ [Delete Email Addresses From the Account\-Level Suppression List](#deleting-personal-data-account-suppression-list)
+ [Delete Data About Email Sent Using Amazon SES](#deleting-personal-data-message-data)
+ [Delete Data About Identities](#deleting-personal-data-identities)
+ [Delete Sender Authentication Data](#deleting-personal-data-sender-authentication)
+ [Delete Data Related to Receiving Rules](#deleting-personal-data-receiving-rules)
+ [Delete Data Related to IP Address Filters](#deleting-personal-data-ip-address-filters)
+ [Delete Data in Email Templates](#deleting-personal-data-email-templates)
+ [Delete Data in Custom Verification Email Templates](#deleting-personal-data-cve-templates)
+ [Delete All Personal Data by Closing Your AWS Account](#deleting-personal-data-closing-account)

## Delete Email Addresses From the Account\-Level Suppression List<a name="deleting-personal-data-account-suppression-list"></a>

Amazon SES includes an optional account\-level suppression list\. When you enable this feature, email addresses are automatically added to a suppression list when they result in a bounce or complaint\. Email addresses remain on this list until you delete them\. For more information about the account\-level suppression list, see [Using the Account\-Level Suppression List](sending-email-suppression-list.md)\.

You can remove email addresses from the account\-level suppression list by using the `DeleteSuppressedDestination` operation in the [Amazon SES API v2](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_DeleteSuppressedDestination.html)\. This section includes a procedure for deleting email addresses by using the AWS CLI\. For more information about installing and configuring the AWS CLI, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\.

**To remove an address from the account\-level suppression list by using the AWS CLI**
+ At the command line, enter the following command:

  ```
  aws sesv2 delete-suppressed-destination --email-address recipient@example.com
  ```

  In the preceding command, replace *recipient@example\.com* with the email address that you want to remove from the account\-level suppression list\.

## Delete Data About Email Sent Using Amazon SES<a name="deleting-personal-data-message-data"></a>

When you use Amazon SES to send an email, you can send information about that email to other AWS services\. For example, you can send information about email events \(such as deliveries, opens, and clicks\) to Kinesis Data Firehose\. This event data typically contains your email address and the IP address the email was sent from\. It also contains the email addresses of all the recipients the email was sent to\.

You can use Kinesis Data Firehose to stream email event data to several destinations—including Amazon Simple Storage Service, Amazon Elasticsearch Service, and Amazon Redshift\. To remove this data, you should first stop streaming data to Kinesis Data Firehose, and then delete the data that has already been streamed\. To stop streaming Amazon SES event data to Kinesis Data Firehose, you must delete the Kinesis Data Firehose event destination\.

**To remove a Kinesis Data Firehose event destination by using the Amazon SES console**

1. Open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. Under **Email Sending**, choose **Configuration Sets**\.

1. In the list of configuration sets, choose the configuration set that contains the Kinesis Data Firehose event destination\.

1. Next to the Kinesis Data Firehose event destination that you want to delete, choose the **delete** \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/delete_icon.png)\) button\.

1. If necessary, remove the data that Kinesis Data Firehose wrote to other services\. For more information, see [Remove Stored Event Data](#deleting-personal-data-message-data-storage)\.

You can also use the Amazon SES API to delete event destinations\. The following procedure uses the AWS Command Line Interface \(AWS CLI\) to interact with the Amazon SES API\. You can also interact with the API by using an AWS SDK, or by making HTTP requests directly\.

**To remove a Kinesis Data Firehose event destination by using the AWS CLI**

1. At the command line, type the following command:

   ```
   aws ses delete-configuration-set-event-destination --configuration-set-name configSet \
   --event-destination-name eventDestination
   ```

   In this command, replace *configSet* with the name of the configuration set that contains the Kinesis Data Firehose event destination\. Replace *eventDestination* with the name of the Kinesis Data Firehose event destination\.

1. If necessary, remove the data that Kinesis Data Firehose wrote to other services\. For more information, see [Remove Stored Event Data](#deleting-personal-data-message-data-storage)\.

### Remove Stored Event Data<a name="deleting-personal-data-message-data-storage"></a>

For more information about deleting information from other AWS services, see the following documents:
+ [Delete an Object and Bucket](https://docs.aws.amazon.com/AmazonS3/latest/gsg/DeletingAnObjectandBucket.html) in the *Amazon Simple Storage Service Getting Started Guide*
+ [Delete an Amazon ES Domain](https://docs.aws.amazon.com/elasticsearch-service/latest/developerguide/es-gsg-deleting.html) in the *Amazon Elasticsearch Service Developer Guide*
+ [Deleting a Cluster](https://docs.aws.amazon.com/redshift/latest/mgmt/managing-clusters-console.html#delete-cluster) in the *Amazon Redshift Cluster Management Guide*

You can also use Kinesis Data Firehose to stream email data to Splunk, a third\-party service that isn't supported by AWS or managed in the AWS Management Console\. For more information about removing data from Splunk, consult your system administrator or the documentation on the [Splunk website](http://docs.splunk.com/Documentation)\.

## Delete Data About Identities<a name="deleting-personal-data-identities"></a>

Identities include the email addresses and domains that you use to send email using Amazon SES\. In some jurisdictions, email addresses or domains might be considered personally identifiable data\.

**To delete an identity by using the Amazon SES console**

1. Open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. Under **Identity Management**, do one of the following:
   + Choose **Domains** if you want to delete a domain\.
   + Choose **Email Addresses** if you want to delete an email address\.

1. Choose the identity that you want to delete, and then choose **Remove**\.

1. On the confirmation dialog box, choose **Yes, Delete Identity**\.

You can also use the Amazon SES API to delete identities\. The following procedure uses the AWS Command Line Interface \(AWS CLI\) to interact with the Amazon SES API\. You can also interact with the API by using an AWS SDK, or by making HTTP requests directly\.

**To delete an identity by using the AWS CLI**
+ At the command line, type the following command:

  ```
  aws ses delete-identity --identity sender@example.com
  ```

  In this command, replace *sender@example\.com* with the identity that you want to delete\.

## Delete Sender Authentication Data<a name="deleting-personal-data-sender-authentication"></a>

Sender authentication refers to the process of configuring Amazon SES so that another user can send email on your behalf\. To enable sender authorization, you must create a policy, as described in [Using Sending Authorization with Amazon SES](sending-authorization.md)\. These policies contain identities \(which belong to you\), in addition to AWS IDs \(which are associated with the person or group that sends email on your behalf\)\. You can remove this personal data by modifying or deleting the sender authentication policies\. The following procedures show you how to delete these policies\.

**To delete a sender authentication policy by using the Amazon SES console**

1. Open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. Under **Identity Management**, do one of the following:
   + Choose **Domains** if the sender authentication policy you want to delete is associated with a domain\.
   + Choose **Email Addresses** if the sender authentication policy you want to delete is associated with an email address\.

1. Under **Identity Policies**, choose the policy you want to delete, and then choose **Remove Policy**\.

You can also use the Amazon SES API to delete sender authentication policies\. The following procedure uses the AWS Command Line Interface \(AWS CLI\) to interact with the Amazon SES API\. You can also interact with the API by using an AWS SDK, or by making HTTP requests directly\.

**To delete a sender authentication policy by using the AWS CLI**
+ At the command line, type the following command:

  ```
  aws ses delete-identity-policy --identity example.com --policy-name samplePolicy
  ```

  In this command, replace *example\.com* with the identity that contains the sender authentication policy\. Replace *samplePolicy* with the name of the sender authentication policy\.

## Delete Data Related to Receiving Rules<a name="deleting-personal-data-receiving-rules"></a>

If you use Amazon SES to receive incoming email, you can create receipt rules that are applied to one or more identities \(email addresses or domains\)\. These rules determine what Amazon SES does with incoming mail sent to the specified identities\.

**To delete a receipt rule by using the Amazon SES console**

1. Open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. Under **Email Receiving**, choose **Rule Sets**\.

1. If the receipt rule is part of the active rule set, choose **View Active Rule Set**\. Otherwise, choose the rule set that contains the receipt rule that you want to delete\.

1. In the list of receipt rules, choose the rule that you want to delete\.

1. On the **Actions** menu, choose **Delete**\.

1. On the confirmation dialog box, choose **Delete**\.

You can also use the Amazon SES API to delete receipt rules\. The following procedure uses the AWS Command Line Interface \(AWS CLI\) to interact with the Amazon SES API\. You can also interact with the API by using an AWS SDK, or by making HTTP requests directly\.

**To delete a receipt rule by using the AWS CLI**
+ At the command line, type the following command:

  ```
  aws ses delete-receipt-rule --rule-set myRuleSet --rule-name myReceiptRule
  ```

  In this command, replace *myRuleSet* with the name of the receipt rule set that contains the receipt rule\. Replace *myReceiptRule* with the name of the receipt rule that you want to delete\.

## Delete Data Related to IP Address Filters<a name="deleting-personal-data-ip-address-filters"></a>

If you use Amazon SES to receive incoming email, you can create filters to explicitly accept or block messages that are sent from specific IP addresses\. 

**To delete an IP address filter by using the Amazon SES console**

1. Open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. Under **Email Receiving**, choose **IP Address Filters**\.

1. In the list of IP address filters, choose the filter that you want to remove, and then choose **Delete**\.

You can also use the Amazon SES API to delete IP address filters\. The following procedure uses the AWS Command Line Interface \(AWS CLI\) to interact with the Amazon SES API\. You can also interact with the API by using an AWS SDK, or by making HTTP requests directly\.

**To delete an IP address filter by using the AWS CLI**
+ At the command line, type the following command:

  ```
  aws ses delete-receipt-filter --filter-name IPfilter
  ```

  In this command, replace *IPfilter* with the name of the IP address filter you want to delete\.

## Delete Data in Email Templates<a name="deleting-personal-data-email-templates"></a>

If you use email templates for sending email, it's possible that those templates might contain personal data, depending on how you configured them\. For example, you might have added an email address to the template that recipients could contact for more information\. 

You can only delete email templates by using the Amazon SES API\.

**To delete an email template by using the AWS CLI**
+ At the command line, type the following command:

  ```
  aws ses delete-template --template-name sampleTemplate
  ```

  In this command, replace *sampleTemplate* with the name of the email template that you want to delete\.

## Delete Data in Custom Verification Email Templates<a name="deleting-personal-data-cve-templates"></a>

If you use customized templates for verifying new email sending addresses, it's possible that those templates might contain personal data, depending on how you configured them\. For example, you might have added an email address to the verification email template that recipients could contact for more information\. 

You can only delete custom verification email templates by using the Amazon SES API\.

**To delete a custom verification email template by using the AWS CLI**
+ At the command line, type the following command:

  ```
  aws ses delete-custom-verification-email-template --template-name verificationEmailTemplate
  ```

  In this command, replace *verificationEmailTemplate* with the name of the custom verification email template that you want to delete\.

## Delete All Personal Data by Closing Your AWS Account<a name="deleting-personal-data-closing-account"></a>

It's also possible to delete all personal data that's stored in Amazon SES by closing your AWS account\. However, this action also deletes all other data—personal or non\-personal—that you have stored in every other AWS service\.

When you close your AWS account, the data in your AWS account is retained for 90 days\. After that retention period, it's deleted permanently and irreversibly\.

**Warning**  
Don't complete the following procedure unless you're certain that you want to completely remove all data that's stored in your AWS account across all AWS services and regions\.

You can close your AWS account by using the AWS Management Console\.

**To close your AWS account**

1. Open the AWS Management Console at [https://console\.aws\.amazon\.com/](https://console.aws.amazon.com/)\.

1. Go to the **Account Settings** page at [https://console\.aws\.amazon\.com/billing/home?\#/account](https://console.aws.amazon.com/billing/home?#/account)\.
**Warning**  
The following two steps will permanently delete **all** of the data you've stored in all AWS services across all AWS Regions\.

1. Under **Close Account**, read the disclaimer that describes the consequences of closing your AWS account\. If you agree to the terms, select the check box, and then choose **Close Account**\.

1. On the confirmation dialog box, choose **Close Account**\.