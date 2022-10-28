# Using the Amazon SES account\-level suppression list<a name="sending-email-suppression-list"></a>

The Amazon SES account\-level suppression list was introduced so that customers can create and control their own suppression lists and reputation, thus, your account\-level suppression list applies to your account only\. The account\-level suppression list interface in the SES console provides an easy way to manage addresses in your account\-level suppression list, including bulk actions to add or remove addresses\.  If an address is on the global suppression list, but not on your account level suppression list *\(which means you want to send to it\)*, and you do send to it, SES will still attempt delivery, but if it bounces, the bounce will affect your own reputation, but no one else will get bounces because they can’t send to that email address if they aren’t using their own account level suppression list; therefore, your account\-level suppression list overrides the global suppression list for your account only\.

Your SES *account\-level suppression list* applies to your AWS account in the current AWS Region\. You can add or remove, individually or in bulk, addresses from your account\-level suppression list by using the SES API v2 or console\.

**Note**  
To bulk add or remove addresses, you must have production access\. To learn more about the sandbox, see [Moving out of the Amazon SES sandbox](request-production-access.md)\.

## Amazon SES Account\-level suppression list considerations<a name="sending-email-suppression-list-considerations"></a>

You should consider the following factors when you use your account\-level suppression list:
+ If you started using Amazon SES after November 25, 2019, your account uses the account\-level suppression list by default for both bounces and complaints\. If you started using SES before this date, then you have to enable this feature by using the `PutAccountSuppressionAttributes` operation in the SES API\.
+ If you attempt to send a message to an address that's on your account\-level suppression list, SES accepts the message, but doesn't send it\.
+ SES doesn't count the messages that you send to addresses on your account\-level suppression list toward the bounce or complaint rates for your account\.
+ SES counts the messages that you send to addresses that aren't on your account\-level suppression list, but are on the global suppression list, toward the bounce or complaint rates for your account\.
+ SES counts the messages that you send to addresses on your account\-level suppression list toward your daily sending quota\.
+ Email addresses on your account\-level suppression list remain there until you remove them\. 
+ If your account's ability to send email is paused, SES automatically deletes the addresses in your account\-level suppression list after 90 days\. If your account's ability to send email is restored before this 90\-day period ends, then the addresses in the list aren't deleted\.
+ Gmail doesn't provide complaint data to SES\. If a recipient uses the **Spam** button in the Gmail web client to report a message that they receive from you as spam, they aren't added to your account\-level suppression list\.
+ You can enable your account\-level suppression list if your account is in the SES sandbox\. However, you can't use the [PutSuppressedDestination](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_PutSuppressedDestination) or [CreateImportJob](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_CreateImportJob.html) operation until your account is removed from the sandbox\. To learn more about the sandbox, see [Moving out of the Amazon SES sandbox](request-production-access.md)\.
+ When you use your account\-level suppression list, SES adds addresses that result in hard bounces or complaints to the global suppression list as well\.

## Enabling the Amazon SES account\-level suppression list<a name="sending-email-suppression-list-enabling"></a>

You can use the [PutAccountSuppressionAttributes](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_PutAccountSuppressionAttributes.html) operation in the Amazon SES API v2 to enable and set up your account\-level suppression list\. You can quickly and easily configure this setting by using the AWS CLI\. For more information about installing and configuring the AWS CLI, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\.

**To configure your account\-level suppression list using the AWS CLI**
+ At the command line, enter the following command:

------
#### [ Linux, macOS, or Unix ]

  ```
  aws sesv2 put-account-suppression-attributes \
  --suppressed-reasons BOUNCE COMPLAINT
  ```

------
#### [ Windows ]

  ```
  aws sesv2 put-account-suppression-attributes `
  --suppressed-reasons BOUNCE COMPLAINT
  ```

------

  To enable your account\-level suppression list, you have to specify at least one reason for the `suppressed-reasons` parameter\. You can specify either `BOUNCE` or `COMPLAINT`, or you can specify both, as shown in the preceding example\.

**To configure your account\-level suppression list using the SES console:**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane, under **Configuration**, choose **Suppression list**\.

1. In the **Account\-level settings** pane, choose **Edit**\.

1. In **Suppression list**, check the **Enabled** box\.

1. In **Suppression reasons**, select one of the reasons for which recipient email addresses should be automatically added to your account\-level suppression list\.

1. Choose **Save changes**\.

## Enabling the Amazon SES account\-level suppression list for a configuration set<a name="sending-email-suppression-list-enabling-configuration-set"></a>

You can also configure your Amazon SES account\-level suppression so that it only applies to specific [configuration sets](using-configuration-sets.md)\. When you do, addresses are only added to the suppression list if you specified the configuration set when you sent the email that caused the bounce or complaint event\.

**Note**  
The following procedure assumes that you've already installed the AWS CLI\. For more information about installing and configuring the AWS CLI, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\.

**To configure your account\-level suppression list for a configuration set using the AWS CLI**
+ At the command line, enter the following command:

------
#### [ Linux, macOS, or Unix ]

  ```
  aws sesv2 put-configuration-set-suppression-options \
  --configuration-set-name configSet \
  --suppressed-reasons BOUNCE COMPLAINT
  ```

------
#### [ Windows ]

  ```
  aws sesv2 put-configuration-set-suppression-options `
  --configuration-set-name configSet `
  --suppressed-reasons BOUNCE COMPLAINT
  ```

------

  In the preceding example, replace *configSet* with the name of the configuration set that should use your account\-level suppression list\.

**To configure your account\-level suppression list for a configuration set using the SES console:**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane, under **Configuration**, choose **Configuration sets**\.

1. In **Configuration sets**, choose the name of the configuration set you want to configure with customized suppression\. 

1. In the **Suppression list options** pane, choose **Edit**\.

1. <a name="suppression-list-config-set-level"></a>The **Suppression list options** section provides a decision set to define customized suppression starting with the option to use this configuration set to override your account\-level suppression\. The [configuration set\-level suppression logic map](sending-email-suppression-list-config-level.md) will help you understand the effects of the override combinations\. These multitiered selections of overrides can be combined to implement three different levels of suppression:

   1. **Use account\-level suppression:** Do not override your account\-level suppression and do not implement any configuration set\-level suppression \- basically, any email sent using this configuration set will just use your account\-level suppression\. To do this:

      1. In **Suppression list settings**, uncheck the **Override account level settings** box\.

   1. **Do not use any suppression:** Override your account\-level suppression without enabling any configuration set\-level suppression \- this means any email sent using this configuration set will not use any of your account\-level suppression; in other words, all suppression is cancelled\. To do this:

      1. In **Suppression list settings**, check the **Override account level settings** box\.

      1. In **Suppression list**, uncheck the **Enabled** box\.

   1. **Use configuration set\-level suppression:** Override your account\-level suppression with custom suppression list settings defined in this configuration set \- this means any email sent using this configuration set will only use its own suppression settings and ignore any account\-level suppression settings\. To do this:

      1. In **Suppression list settings**, check the **Override account level settings** box\. 

      1. In **Suppression list**, check **Enabled**\. 

      1. In **Specify the reason\(s\)\.\.\.**, select one of the suppression reasons for this configuration set to use\.

1. Choose **Save changes**\.

## Adding individual email addresses to the Amazon SES account\-level suppression list<a name="sending-email-suppression-list-manual-add"></a>

You can add individual addresses to your Amazon SES account\-level suppression list by using the [PutSuppressedDestination](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_PutSuppressedDestination.html) operation in the SES API v2\. There's no limit to the number of addresses that you can add to your account\-level suppression list\.

**Note**  
The following procedure assumes that you've already installed the AWS CLI\. For more information about installing and configuring the AWS CLI, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\.

**To add individual addresses to your account\-level suppression list using the AWS CLI**
+ At the command line, enter the following command:

------
#### [ Linux, macOS, or Unix ]

  ```
  aws sesv2 put-suppressed-destination \
  --email-address recipient@example.com \
  --reason BOUNCE
  ```

------
#### [ Windows ]

  ```
  aws sesv2 put-suppressed-destination `
  --email-address recipient@example.com `
  --reason BOUNCE
  ```

------

  In the preceding example, replace *recipient@example\.com* with the email address that you want to add to your account\-level suppression list, and *BOUNCE* with the reason that you're adding the address to the suppression list \(acceptable values are `BOUNCE` and `COMPLAINT`\)\.

**To add individual addresses to your account\-level suppression list using the SES console:**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane, under **Configuration**, choose **Suppression list**\.

1. In the **Suppression list** pane, choose **Add email address**\.

1. Type an email address in the **Email address** field followed by selecting a reason in **Suppression reason** \- if you need to enter more addresses, choose **Enter another address** and repeat for each additional one\.

1. When done entering addresses, review your entries for accuracy\. If you decide any of your entries shouldn't be part of this submission, choose its **Remove** button\.

1. Choose **Save changes** to add the entered email addresses to your account\-level suppression list\.

## Adding email addresses in bulk to your Amazon SES account\-level suppression list<a name="sending-email-suppression-list-manual-add-bulk"></a>

You can add addresses in bulk by first uploading your contact list into an Amazon S3 object followed by using the [CreateImportJob](#CIJ-add-bulk-API) operation in the Amazon SES API v2\.

**Note**  
There's no limit to the number of addresses that you can add to your account\-level suppression list, but there is a bulk add limit of 100,000 addresses in an Amazon S3 object per API call\.

To add email addresses in bulk to your account\-level suppression list, complete the following steps\.
+ Upload your address list into an Amazon S3 object in either CSV or JSON format\. 

  CSV format example for adding addresses:

  `recipient1@example.com,BOUNCE`

  `recipient2@example.com,COMPLAINT`

  Only newline\-delimited JSON files are supported\. In this format, each line is a complete JSON object that contains an individual address definition\.

  JSON format example for adding addresses:

  `{"emailAddress":"recipient1@example.com","reason":"BOUNCE"}`

  `{"emailAddress":"recipient2@example.com","reason":"COMPLAINT"}`

  In the preceding examples, replace *recipient1@example\.com* and *recipient2@example\.com* with the email addresses that you want to add to your account\-level suppression list\. The acceptable reasons that you're adding the addresses to the suppression list are `BOUNCE` and `COMPLAINT`\.
+ Give SES permission to read the Amazon S3 object\.

  When applied to an Amazon S3 bucket, the following policy gives SES permission to read that bucket\. For more information about attaching policies to Amazon S3 buckets, see [Using Bucket Policies and User Policies](https://docs.aws.amazon.com/AmazonS3/latest/dev/using-iam-policies.html) in the *Amazon Simple Storage Service User Guide*\.

  ```
  {
      "Version": "2012-10-17",
      "Statement": [
          {
              "Sid": "AllowSESGet",
              "Effect": "Allow",
              "Principal": {
                  "Service": "ses.amazonaws.com"
              },
              "Action": "s3:GetObject",
              "Resource": "arn:aws:s3:::BUCKET-NAME/OBJECT-NAME",
              "Condition": {
                  "StringEquals": {
                      "aws:Referer": "AWSACCOUNTID"
                  }
              }
          }
      ]
  }
  ```
+ Give SES permission to use your AWS KMS key\.

  If the Amazon S3 object is encrypted with an AWS KMS key, you need to give Amazon SES permission to use the AWS KMS key\. SES can only attain permission from a customer managed key, not a default KMS key\. You need to give SES permission to use the customer managed key by adding a statement to the key's policy\.

  Paste the following policy statement into the key policy to permit SES to use your customer managed key\.

  ```
  {
     "Sid": "AllowSESToDecrypt", 
     "Effect": "Allow",
     "Principal": {
         "Service":"ses.amazonaws.com"
     },
     "Action": [
         "kms:Decrypt", 
     ],
     "Resource": "*"
  }
  ```
+ Use the [CreateImportJob](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_CreateImportJob.html) operation in the SES API v2\.

**Note**  
The following example assumes that you've already installed the AWS CLI\. For more information about installing and configuring the AWS CLI, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\.

At the command line, enter the following command\. Replace *s3bucket* with the name of an Amazon S3 bucket and *s3object* with the name of an Amazon S3 object\.

```
aws sesv2 create-import-job --import-destination SuppressionListDestination={SuppressionListImportAction=PUT} --import-data-source S3Url=s3://s3bucket/s3object,DataFormat=CSV
```

**To add email addresses in bulk to your account\-level suppression list using the SES console:**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane, under **Configuration**, choose **Suppression list**\.

1. In the **Suppression list** table, expand the **Bulk actions** button and select **Add email addresses in bulk**\.

1. In **Bulk action specifications**, select either \(a\)**Choose file from S3 bucket** or \(b\)**Import from file** \- procedures are given for each import method: 

   1. **Choose file from S3 bucket** \- *if your source file is already stored in an Amazon S3 bucket*:

      1. If you know the URI of the Amazon S3 bucket you want to use, enter it in the **Amazon S3 URI** field; otherwise, choose **Browse S3**:

         1. In **Buckets**, select the name of the S3 bucket\.

         1. In **Objects**, select the name of the file then select **Choose** \- you'll be returned to **Bulk action specifications**\.

         1. \(Optional\) If you want to be taken to the Amazon S3 console to view details about your S3 object choose **View**\.

      1. In **File format**, select the format of the file you've chosen to import from you Amazon S3 bucket\.

      1. Choose **Add email addresses** to kick off the import of addresses from your file \- a table under the **Bulk actions** tab is displayed\.

   1. **Import from file** \- *if you have a local source file to upload to a new or existing Amazon S3 bucket*:

      1. In **Import source file**, select **Choose file**\.

      1. Select the JSON or CSV file in the file browser and choose **Open** \- you'll see the name, size, and date of your file displayed under the **Choose file** button\.

      1. Expand **Amazon S3 bucket** and select the S3 bucket\.

         1. To upload your file to a new bucket, choose **Create S3 bucket**, enter a name in the **Bucket name** field, and choose **Create bucket**\.

      1. Choose **Add email addresses** to kick off the import of addresses from your file \- a table under the **Bulk actions** tab is displayed\.

1. Regardless of the import method you used, your job ID will be listed in **Bulk actions** along with import type, status, and date \- to view job details, select the job ID\.

1. Select the **Suppression list** tab and all the successfully imported email addresses are displayed with their suppression reason and date added \- the following options are available:

   1. Select an email address, or select its corresponding checkbox and choose **View report** to view its details\. \(If it's an address that was automatically added to your suppression list because of a bounce or complaint, information will be displayed about the feedback event that caused it to be added, including details about the email message that produced the triggering event\.\)

   1. Select the corresponding checkbox of one or more email addresses you want to remove from your account suppression list and choose **Remove**\.

## Viewing a list of addresses that are on your Amazon SES account\-level suppression list<a name="sending-email-suppression-list-view-entries"></a>

You can view a list of all of the email addresses that are on your account\-level suppression list for your account by using the [ListSuppressedDestinations](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_ListSuppressedDestinations.html) operation in the SES API v2\.

**Note**  
The following procedure assumes that you've already installed the AWS CLI\. For more information about installing and configuring the AWS CLI, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\.

**To view a list of all of the email addresses that are on your account\-level suppression list**
+ At the command line, enter the following command:

  ```
  aws sesv2 list-suppressed-destinations
  ```

The preceding command returns all of the email addresses that are in your account\-level suppression list for your account\. The output resembles the following example:

```
{
    "SuppressedDestinationSummaries": [
        {
            "EmailAddress": "recipient2@example.com",
            "Reason": "COMPLAINT",
            "LastUpdateTime": "2020-04-10T21:03:05Z"
        },
        {
            "EmailAddress": "recipient0@example.com",
            "Reason": "COMPLAINT",
            "LastUpdateTime": "2020-04-10T21:04:26Z"
        },
        {
            "EmailAddress": "recipient1@example.com",
            "Reason": "BOUNCE",
            "LastUpdateTime": "2020-04-10T22:07:59Z"
        }
    ]
}
```
+ **Note** – If your output includes a "NextToken" field with a string value, this indicates there are additional email addresses on the suppression list for your account\. To view additional suppressed addresses, issue another request to `ListSuppressedDestinations`, and pass the returned string value in the `--next-token` parameter like so:

  ```
  aws sesv2 list-suppressed-destinations --next-token string
  ```

  In the preceding command, replace *string* with the returned NextToken value\.

You can use the `StartDate` option to only show email addresses that were added to the list *after* a certain date\.

**To view a list of addresses that were added to your account\-level suppression list after a specific date**
+ At the command line, enter the following command:

  ```
  aws sesv2 list-suppressed-destinations --start-date 1604394130
  ```

  In the preceding command, replace *1604394130* with the Unix timestamp of the start date\.

You can also use the `EndDate` option to only show email addresses that were added to the list *before* a certain date\.

**To view a list of addresses that were added to your account\-level suppression list before a specific date**
+ At the command line, enter the following command:

  ```
  aws sesv2 list-suppressed-destinations --end-date 1611126000
  ```

  In the preceding command, replace *1611126000* with the Unix timestamp of the end date\.

On the Linux, macOS, or Unix command line, you can also use the built\-in `grep` utility to search for specific addresses or domains\.

**To search your account\-level suppression list for a specific address**
+ At the command line, enter the following command:

  ```
  aws sesv2 list-suppressed-destinations | grep -A2 'example.com'
  ```

  In the preceding command, replace *example\.com* with the string of text \(such as the address or domain\) that you want to search for\.

**To view a list of all of the email addresses that are on your account\-level suppression list using the SES console:**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane, under **Configuration**, choose **Suppression list**\.

1. In the **Suppression list** pane, all the email addresses on your account\-level suppression list are displayed with their suppression reason and date added \- the following options are available:

   1. Select an email address, or select its corresponding checkbox and choose **View report** to view its details\. \(If it's an address that was automatically added to your suppression list because of a bounce or complaint, information will be displayed about the feedback event that caused it to be added, including details about the email message that produced the triggering event\.\)

   1. You can customize the suppression list table by choosing the gear icon \- a modal will be presented where you can customize page size, line wrap, and columns to view \- after making your selections, choose **Confirm**\. The suppression list table will reflect your viewing choices\.

## Removing individual email addresses from your Amazon SES account\-level suppression list<a name="sending-email-suppression-list-manual-delete"></a>

If an address is on the suppression list for your account, but you know that the address shouldn't be on the list, you can remove it by using [DeleteSuppressedDestination](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_DeleteSuppressedDestination.html) operation in the SES API v2\.

**Note**  
The following procedure assumes that you've already installed the AWS CLI\. For more information about installing and configuring the AWS CLI, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\.

**To remove individual addresses from your account\-level suppression list using the AWS CLI**
+ At the command line, enter the following command:

------
#### [ Linux, macOS, or Unix ]

  ```
  aws sesv2 delete-suppressed-destination \
  --email-address recipient@example.com
  ```

------
#### [ Windows ]

  ```
  aws sesv2 delete-suppressed-destination `
  --email-address recipient@example.com
  ```

------

  In the preceding example, replace *recipient@example\.com* with the email address that you want to remove from your account\-level suppression list\.

**To remove individual addresses from your account\-level suppression list using the SES console:**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane, under **Configuration**, choose **Suppression list**\.

1. Remove individual email addresses either by *\(a\)* table selection or *\(b\)* typed entry:

   1. *Select from table*: In the **Suppression list** table, select the corresponding checkbox of one or more email addresses and choose **Remove**\.

   1. *Type in field*:

      1. In the **Suppression list** table, choose **Remove email address**\.

      1. Type an email address in the **Email address** field \- if you need to enter more addresses, choose **Enter another address** and repeat for each additional one\.

      1. When done entering addresses, review your entries for accuracy\. If you decide any of your entries shouldn't be part of this submission, choose its **Remove** button\.

      1. Choose **Save changes** to remove the entered email addresses from your account\-level suppression list\.

## Removing email addresses in bulk from your Amazon SES account\-level suppression list<a name="sending-email-suppression-list-manual-delete-bulk"></a>

You can remove addresses in bulk by first uploading your contact list into an Amazon S3 object followed by using the [CreateImportJob](#CIJ-remove-bulk-API) operation in the SES API v2\.

**Note**  
There's no limit to the number of addresses that you can remove from the account\-level suppression list, but there is a bulk delete limit of 10,000 addresses in an Amazon S3 object per API call\.

To remove email addresses in bulk from your account\-level suppression list, complete the following steps\.
+ Upload your address list into an Amazon S3 object in either CSV or JSON format\. 

  CSV format example for removing addresses:

  `recipient3@example.com`

  Only newline\-delimited JSON files are supported\. In this format, each line is a complete JSON object that contains an individual address definition\.

  JSON format example for adding addresses:

  `{"emailAddress":"recipient3@example.com"}`

  In the preceding examples, replace *recipient3@example\.com* with the email addresses that you want to remove from your account\-level suppression list\.
+ Give SES permission to read the Amazon S3 object\.

  When applied to an Amazon S3 bucket, the following policy gives SES permission to read that bucket\. For more information about attaching policies to Amazon S3 buckets, see [Using Bucket Policies and User Policies](https://docs.aws.amazon.com/AmazonS3/latest/dev/using-iam-policies.html) in the *Amazon Simple Storage Service User Guide*\.

  ```
  {
      "Version": "2012-10-17",
      "Statement": [
          {
              "Sid": "AllowSESGet",
              "Effect": "Allow",
              "Principal": {
                  "Service": "ses.amazonaws.com"
              },
              "Action": "s3:GetObject",
              "Resource": "arn:aws:s3:::BUCKET-NAME/OBJECT-NAME",
              "Condition": {
                  "StringEquals": {
                      "aws:Referer": "AWSACCOUNTID"
                  }
              }
          }
      ]
  }
  ```
+ Give SES permission to use your AWS KMS key\.

  If the Amazon S3 object is encrypted with an AWS KMS key, you need to give Amazon SES permission to use the AWS KMS key\. SES can only attain permission from a customer managed key, not a default KMS key\. You need to give SES permission to use the customer managed key by adding a statement to the key's policy\.

  Paste the following policy statement into the key policy to permit SES to use your customer managed key\.

  ```
  {
     "Sid": "AllowSESToDecrypt", 
     "Effect": "Allow",
     "Principal": {
         "Service":"ses.amazonaws.com"
     },
     "Action": [
         "kms:Decrypt", 
     ],
     "Resource": "*"
  }
  ```
+ Use the [CreateImportJob](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_CreateImportJob.html) operation in the SES API v2\.

**Note**  
The following example assumes that you've already installed the AWS CLI\. For more information about installing and configuring the AWS CLI, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\.

At the command line, enter the following command\. Replace *s3bucket* with the name of the Amazon S3 bucket and *s3object* with the name of the Amazon S3 object\.

```
aws sesv2 create-import-job --import-destination SuppressionListDestination={SuppressionListImportAction=DELETE} --import-data-source S3Url="s3://s3bucket/s3object",DataFormat=CSV
```

**To remove email addresses in bulk from your account\-level suppression list using the SES console:**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane, under **Configuration**, choose **Suppression list**\.

1. In the **Suppression list** table, expand the **Bulk actions** button and select **Remove email addresses in bulk**\.

1. In **Bulk action specifications**, select either \(a\) **Choose file from S3 bucket** or \(b\) **Import from file** \- procedures are given for each import method: 

   1. **Choose file from S3 bucket** \- *if your source file is already stored in an Amazon S3 bucket*:

      1. If you know the URI of the Amazon S3 bucket you want to use, enter it in the **Amazon S3 URI** field; otherwise, choose **Browse S3**:

         1. In **Buckets**, select the name of the S3 bucket\.

         1. In **Objects**, select the name of the file then select **Choose** \- you'll be returned to **Bulk action specifications**\.

         1. \(Optional\) If you want to be taken to the Amazon S3 console to view details about your S3 object choose **View**\.

      1. In **File format**, select the format of the file you've chosen to import from your Amazon S3 bucket\.

      1. Choose **Remove email addresses** to kick off the import of addresses from your file \- a table under the **Bulk actions** tab is displayed\.

   1. **Import from file** \- *if you have a local source file to upload to a new or existing Amazon S3 bucket*:

      1. In **Import source file**, select **Choose file**\.

      1. Select the JSON or CSV file in the file browser and choose **Open** \- you'll see the name, size, and date of your file displayed under the **Choose file** button\.

      1. Expand **Amazon S3 bucket** and select the S3 bucket\.

         1. To upload your file to a new bucket, choose **Create S3 bucket**, enter a name in the **Bucket name** field, and choose **Create bucket**\.

      1. Choose **Remove email addresses** to kick off the import of addresses from your file \- a table under the **Bulk actions** tab is displayed\.

1. Regardless of the import method you used, your job ID will be listed in **Bulk actions** along with import type, status, and date \- to view job details, select the job ID\.

1. Select the **Suppression list** tab and all the successfully imported email addresses that were removed from your suppression list will no longer be displayed\.

## Viewing a list of import jobs for the account<a name="sending-email-suppression-list-view-import-jobs"></a>

You can view a list of all of the email addresses that are on your account\-level suppression list for your account by using the [ListImportJobs](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_ListImportJobs.html) operation in the Amazon SES API v2\.

**Note**  
The following procedure assumes that you've already installed the AWS CLI\. For more information about installing and configuring the AWS CLI, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\.

**To view a list of all of the import jobs for the account**
+ At the command line, enter the following command:

  ```
  aws sesv2 list-import-jobs
  ```

The preceding command returns all of the import jobs for the account\. The output resembles the following example:

```
{
    "ImportJobs": [
        {
            "CreatedTimestamp": 2020-07-31T06:06:55Z",
            "ImportDestination": {
                "SuppressionListDestination": {
                    "SuppressionListImportAction": "PUT"
                }
            },
            "JobStatus": "COMPLETED",
            "JobId": "755380d7-fbdb-4ed2-a9a3-06866220f5b5"
        },
        {
            "CreatedTimestamp": "2020-07-30T18:45:32Z",
            "ImportDestination": {
                "SuppressionListDestination": {
                    "SuppressionListImportAction": "DELETE"
                }
            },
            "JobStatus": "COMPLETED",
            "JobId": "076683bd-a7ee-4a40-9754-4ad1161ba8b6"
        },
        {
            "CreatedTimestamp": "2020-08-05T16:45:18Z",
            "ImportDestination": {
                "SuppressionListDestination": {
                    "SuppressionListImportAction": "PUT"
                }
            },
            "JobStatus": "COMPLETED",
            "JobId": "6e261869-bd30-4b33-b1f2-9e035a83a395"
        }
    ]
}
```

**To view a list of all of the import jobs for the account using the SES console:**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane, under **Configuration**, choose **Suppression list**\.

1. In the **Suppression list** pane, select the **Bulk actions** tab\.

1. All the import jobs will be listed in the **Bulk actions** table along with import type, status, and date\.

1. To view job details, select the job ID and the following panes are displayed:

   1. **Bulk action status**: shows the jobs overall status, the time and date it completed, how many records where imported, and the count of any records that failed to import successfully\.

   1. **Bulk action details**: shows the job ID, whether it was used to add or remove addresses, whether the file format was JSON or CSV, the URI of the Amazon S3 bucket where the bulk file was stored, and the time and date the bulk action was created\.

## Getting information about an import job for the account<a name="sending-email-suppression-list-get-import-jobs"></a>

You can get information about an import job for the account by using the [GetImportJob](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_GetImportJob.html) operation in the Amazon SES API v2\.

**Note**  
The following procedure assumes that you've already installed the AWS CLI\. For more information about installing and configuring the AWS CLI, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\.

**To get information about an import job for the account**
+ At the command line, enter the following command:

  ```
  aws sesv2 get-import-job --job-id JobId
  ```

The preceding command returns information about an import job for the account\. The output resembles the following example:

```
{
    "ImportDataSource": {
        "S3Url": "s3://bucket/object",
        "DataFormat": "CSV"
    },
    "ProcessedRecordsCount": 2,
    "FailureInfo": {
        "FailedRecordsS3Url": "s3presignedurl"
    },
    "JobStatus": "COMPLETED",
    "JobId": "jobid",
    "CreatedTimestamp": "2020-08-12T17:05:15Z",
    "FailedRecordsCount": 1,
    "ImportDestination": {
        "SuppressionListDestination": {
            "SuppressionListImportAction": "PUT"
        }
    },
    "CompletedTimestamp": "2020-08-12T17:06:42Z"
}
```

**To get information about an import job for the account using the SES console:**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane, under **Configuration**, choose **Suppression list**\.

1. In the **Suppression list** pane, select the **Bulk actions** tab\.

1. All the import jobs will be listed in the **Bulk actions** table along with import type, status, and date\.

1. To view job details, select the job ID and the following panes are displayed:

   1. **Bulk action status**: shows the jobs overall status, the time and date it completed, how many records where imported, and the count of any records that failed to import successfully\.

   1. **Bulk action details**: shows the job ID, whether it was used to add or remove addresses, whether the file format was JSON or CSV, the URI of the Amazon S3 bucket where the bulk file was stored, and the time and date the bulk action was created\.

## Disabling the Amazon SES account\-level suppression list<a name="sending-email-suppression-list-disabling"></a>

You can use the [PutAccountSuppressionAttributes](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_PutAccountSuppressionAttributes.html) operation in the SES API v2 to effectively disable your account\-level suppression list by removing the values from the `suppressed-reasons` attribute\.

**Note**  
The following procedure assumes that you've already installed the AWS CLI\. For more information about installing and configuring the AWS CLI, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\.

**To disable your account\-level suppression list using the AWS CLI**
+ At the command line, enter the following command:

  ```
  aws sesv2 put-account-suppression-attributes --suppressed-reasons
  ```

**To disable your account\-level suppression list using the SES console:**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane, under **Configuration**, choose **Suppression list**\.

1. In the **Account\-level settings** pane, choose **Edit**\.

1. In **Suppression list**, uncheck the **Enabled** box\.

1. Choose **Save changes**\.