# Using the account\-level suppression list<a name="sending-email-suppression-list"></a>

Amazon SES includes an *account\-level suppression list* that applies to your AWS account in the current AWS Region\. This suppression list prevents you from sending email to addresses that previously produced a bounce or complaint event\. When you configure the account\-level suppression list, you specify whether addresses should be added to the list when they result in hard bounces, when they result in complaints, or both\. You can manually add or remove individual or bulk addresses from the account\-level suppression list by using the Amazon SES API v2\.

**Note**  
To bulk add or remove addresses, you must have production access\. To learn more about the sandbox, see [Moving out of the Amazon SES sandbox](request-production-access.md)\.

Amazon SES also includes a global suppression list\. For more information, see [Using the Amazon SES global suppression list](sending-email-global-suppression-list.md)\.

## Account\-level suppression list considerations<a name="sending-email-suppression-list-considerations"></a>

You should consider the following factors when you use the account\-level suppression list:
+ If you started using Amazon SES after November 25, 2019, your account uses the account\-level suppression list by default for both bounces and complaints\. If you started using Amazon SES before this date, then you have to enable this feature by using the `PutAccountSuppressionAttributes` operation in the Amazon SES API\.
+ If you attempt to send a message to an address that's on the account\-level suppression list, Amazon SES accepts the message, but doesn't send it\.
+ Amazon SES doesn't count the messages that you send to addresses on the account\-level suppression list toward the bounce rate for your account\.
+ Amazon SES counts the messages that you send to addresses on the account\-level suppression list toward your daily sending quota\.
+ Email addresses on the account\-level suppression list remain there until you remove them by using the [DeleteSuppressedDestination](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_DeleteSuppressedDestination.html) operation in the Amazon SES API v2\.
+ If your account's ability to send email is paused, Amazon SES automatically deletes the addresses in the account\-level suppression list after 90 days\. If your account's ability to send email is restored before this 90\-day period ends, then the addresses in the account\-level suppression list aren't deleted\.
+ Gmail doesn't provide complaint data to Amazon SES\. If a recipient uses the **Spam** button in the Gmail web client to report a message that they receive from you as spam, they aren't added to the account\-level suppression list\.
+ You can enable the account\-level suppression list if your account is in the Amazon SES sandbox\. However, you can't use the [PutSuppressedDestination](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_PutSuppressedDestination) or [CreateImportJob](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_CreateImportJob.html) operation until your account is removed from the sandbox\. To learn more about the sandbox, see [Moving out of the Amazon SES sandbox](request-production-access.md)\.
+ When you use the account\-level suppression list, Amazon SES also adds addresses that result in hard bounces to the global suppression list\.

## Enabling the account\-level suppression list<a name="sending-email-suppression-list-enabling"></a>

You can use the [PutAccountSuppressionAttributes](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_PutAccountSuppressionAttributes.html) operation in the Amazon SES API v2 to enable and set up the account\-level suppression list\. You can quickly and easily configure this setting by using the AWS CLI\. For more information about installing and configuring the AWS CLI, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\.

**To configure the account\-level suppression list by using the AWS CLI**
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

  To enable the account\-level suppression list, you have to specify at least one reason for the `suppressed-reasons` parameter\. You can specify either `BOUNCE` or `COMPLAINT`, or you can specify both, as shown in the preceding example\.

## Enabling the account\-level suppression list for a configuration set<a name="sending-email-suppression-list-enabling-configuration-set"></a>

You can also configure the account\-level suppression so that it only applies to specific [configuration sets](using-configuration-sets.md)\. When you do, addresses are only added to the suppression list if you specified the configuration set when you sent the email that caused the bounce or complaint event\.

**Note**  
The following procedure assumes that you've already installed the AWS CLI\. For more information about installing and configuring the AWS CLI, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\.

**To configure the account\-level suppression list for a configuration set by using the AWS CLI**
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

  In the preceding example, replace *configSet* with the name of the configuration set that should use the account\-level suppression list\.

## Manually adding individual email addresses to the account\-level suppression list<a name="sending-email-suppression-list-manual-add"></a>

You can manually add individual addresses to the account\-level suppression list by using the [PutSuppressedDestination](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_PutSuppressedDestination.html) operation in the Amazon SES API v2\. There's no limit to the number of addresses that you can add to the account\-level suppression list\.

**Note**  
The following procedure assumes that you've already installed the AWS CLI\. For more information about installing and configuring the AWS CLI, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\.

**To manually add an address to the account\-level suppression list by using the AWS CLI**
+ At the command line, enter the following command:

------
#### [ Linux, macOS, or Unix ]

  ```
  aws sesv2 put-suppressed-destination \
  --email-address="recipient@example.com" \
  --reason BOUNCE
  ```

------
#### [ Windows ]

  ```
  aws sesv2 put-suppressed-destination `
  --email-address="recipient@example.com" `
  --reason BOUNCE
  ```

------

  In the preceding example, replace *recipient@example\.com* with the email address that you want to add to the account\-level suppression list, and *BOUNCE* with the reason that you're adding the address to the suppression list \(acceptable values are `BOUNCE` and `COMPLAINT`\)\.

## Adding email addresses in bulk to the account\-level suppression list<a name="sending-email-suppression-list-manual-add-bulk"></a>

You can manually add addresses in bulk by first uploading your contact list into an Amazon S3 object followed by using the [CreateImportJob](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_CreateImportJob.html) operation in the Amazon SES API v2\.

**Note**  
There's no limit to the number of addresses that you can add to the account\-level suppression list, but there is a bulk add limit of 100,000 addresses in an Amazon S3 object per API call\.

To add email addresses in bulk to your account\-level suppression list, complete the following steps\.
+ Upload your address list into an Amazon S3 object in either CSV or JSON format\. 

  CSV format example for adding addresses:

  `recipient1@example.com,BOUNCE`

  `recipient2@example.com,COMPLAINT`

  Only newline\-delimited JSON files are supported\. In this format, each line is a complete JSON object that contains an individual address definition\.

  JSON format example for adding addresses:

  `{"emailAddress":"recipient1@example.com","reason":"BOUNCE"}`

  `{"emailAddress":"recipient2@example.com","reason":"COMPLAINT"}`

  In the preceding examples, replace *recipient1@example\.com* and *recipient2@example\.com* with the email addresses that you want to add to the account\-level suppression list\. The acceptable reasons that you're adding the addresses to the suppression list are `BOUNCE` and `COMPLAINT`\.
+ Give Amazon SES permission to read the Amazon S3 object\.

  When applied to an Amazon S3 bucket, the following policy gives Amazon SES permission to read that bucket\. For more information about attaching policies to Amazon S3 buckets, see [Using Bucket Policies and User Policies](https://docs.aws.amazon.com/AmazonS3/latest/dev/using-iam-policies.html) in the *Amazon Simple Storage Service Developer Guide*\.

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
+ Give Amazon SES permission to use your AWS KMS master key\.

  If the Amazon S3 object is encrypted with an AWS KMS key, you need to give Amazon SES permission to use the AWS KMS key\. Amazon SES can only attain permission from a custom master key, not a default master key\. You need to give Amazon SES permission to use the custom master key by adding a statement to the key's policy\.

  Paste the following policy statement into the key policy to permit Amazon SES to use your custom master key\.

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
+ Use the [CreateImportJob](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_CreateImportJob.html) operation in the Amazon SES API v2\.

**Note**  
The following procedure assumes that you've already installed the AWS CLI\. For more information about installing and configuring the AWS CLI, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\.

**To manually add bulk addresses to the account\-level suppression list by using the AWS CLI**
+ At the command line, enter the following command:

------
#### [ Linux, macOS, or Unix ]

  ```
  aws sesv2 create-import-job \
  --import-destination "{\"SuppressionListDestination\": {\"SuppressionListImportAction\":\"PUT\"}}" \
  --import-data-source "{\"S3Url\": \"s3://s3bucket/s3object\",\"DataFormat\": \"CSV\"}"
  ```

------
#### [ Windows ]

  ```
  aws sesv2 create-import-job `
  --import-destination "{\"SuppressionListDestination\": {\"SuppressionListImportAction\":\"PUT\"}}" `
  --import-data-source "{\"S3Url\": \"s3://s3bucket/s3object\",\"DataFormat\": \"CSV\"}"
  ```

------

  In the preceding examples, replace *s3bucket* and *s3object* with the Amazon S3 bucket name and Amazon S3 object name\.

## Viewing a list of addresses that are on the account\-level suppression list<a name="sending-email-suppression-list-view-entries"></a>

You can view a list of all of the email addresses that are on the account\-level suppression list for your account by using the [ListSuppressedDestinations](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_ListSuppressedDestinations.html) operation in the Amazon SES API v2\.

**Note**  
The following procedure assumes that you've already installed the AWS CLI\. For more information about installing and configuring the AWS CLI, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\.

**To view a list of all of the email addresses that are on the account\-level suppression list**
+ At the command line, enter the following command:

  ```
  aws sesv2 list-suppressed-destinations
  ```

The preceding command returns all of the email addresses that are in the account\-level suppression list for your account\. The output resembles the following example:

```
{
    "SuppressedDestinationSummaries": [
        {
            "EmailAddress": "recipient2@example.com",
            "Reason": "COMPLAINT",
            "LastUpdateTime": 1586552585.077
        },
        {
            "EmailAddress": "recipient0@example.com",
            "Reason": "COMPLAINT",
            "LastUpdateTime": 1586552666.613
        },
        {
            "EmailAddress": "recipient1@example.com",
            "Reason": "BOUNCE",
            "LastUpdateTime": 1586556479.141
        }
    ]
}
```

You can use the `StartDate` option to only show email addresses that were added to the list *after* a certain date\.

**To view a list of addresses that were added to the account\-level suppression list after a specific date**
+ At the command line, enter the following command:

  ```
  aws sesv2 list-suppressed-destinations --start-date 1604394130
  ```

  In the preceding command, replace *1604394130* with the Unix timestamp of the start date\.

You can also use the `EndDate` option to only show email addresses that were added to the list *before* a certain date\.

**To view a list of addresses that were added to the account\-level suppression list before a specific date**
+ At the command line, enter the following command:

  ```
  aws sesv2 list-suppressed-destinations --end-date 1611126000
  ```

  In the preceding command, replace *1611126000* with the Unix timestamp of the end date\.

On the Linux, macOS, or Unix command line, you can also use the built\-in `grep` utility to search for specific addresses or domains\.

**To search the account\-level suppression list for a specific address**
+ At the command line, enter the following command:

  ```
  aws sesv2 list-suppressed-destinations | grep -A2 'example.com'
  ```

  In the preceding command, replace *example\.com* with the string of text \(such as the address or domain\) that you want to search for\.

## Removing an email address from the account\-level suppression list<a name="sending-email-suppression-list-manual-delete"></a>

If an address is on the suppression list for your account, but you know that the address shouldn't be on the list, you can manually remove it by using [DeleteSuppressedDestination](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_DeleteSuppressedDestination.html) operation in the Amazon SES API v2\.

**Note**  
The following procedure assumes that you've already installed the AWS CLI\. For more information about installing and configuring the AWS CLI, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\.

**To remove an address from the account\-level suppression list by using the AWS CLI**
+ At the command line, enter the following command:

------
#### [ Linux, macOS, or Unix ]

  ```
  aws sesv2 delete-suppressed-destination \
  --email-address="recipient@example.com"
  ```

------
#### [ Windows ]

  ```
  aws sesv2 delete-suppressed-destination `
  --email-address="recipient@example.com"
  ```

------

  In the preceding example, replace *recipient@example\.com* with the email address that you want to remove from the account\-level suppression list\.

## Removing email addresses in bulk from the account\-level suppression list<a name="sending-email-suppression-list-manual-delete-bulk"></a>

You can manually remove addresses in bulk by first uploading your contact list into an Amazon S3 object followed by using the [CreateImportJob](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_CreateImportJob.html) operation in the Amazon SES API v2\.

**Note**  
There's no limit to the number of addresses that you can remove from the account\-level suppression list, but there is a bulk delete limit of 10,000 addresses in an Amazon S3 object per API call\.

To remove email addresses in bulk from your account\-level suppression list, complete the following steps\.
+ Upload your address list into an Amazon S3 object in either CSV or JSON format\. 

  CSV format example for removing addresses:

  `recipient3@example.com`

  Only newline\-delimited JSON files are supported\. In this format, each line is a complete JSON object that contains an individual address definition\.

  JSON format example for adding addresses:

  `{"emailAddress":"recipient3@example.com"}`

  In the preceding examples, replace *recipient3@example\.com* with the email addresses that you want to remove from the account\-level suppression list\.
+ Give Amazon SES permission to read the Amazon S3 object\.

  When applied to an Amazon S3 bucket, the following policy gives Amazon SES permission to read that bucket\. For more information about attaching policies to Amazon S3 buckets, see [Using Bucket Policies and User Policies](https://docs.aws.amazon.com/AmazonS3/latest/dev/using-iam-policies.html) in the *Amazon Simple Storage Service Developer Guide*\.

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
+ Give Amazon SES permission to use your AWS KMS master key\.

  If the Amazon S3 object is encrypted with an AWS KMS key, you need to give Amazon SES permission to use the AWS KMS key\. Amazon SES can only attain permission from a custom master key, not a default master key\. You need to give Amazon SES permission to use the custom master key by adding a statement to the key's policy\.

  Paste the following policy statement into the key policy to permit Amazon SES to use your custom master key\.

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
+ Use the [CreateImportJob](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_CreateImportJob.html) operation in the Amazon SES API v2\.

**Note**  
The following procedure assumes that you've already installed the AWS CLI\. For more information about installing and configuring the AWS CLI, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\.

**To manually remove bulk addresses from the account\-level suppression list by using the AWS CLI**
+ At the command line, enter the following command:

------
#### [ Linux, macOS, or Unix ]

  ```
  aws sesv2 create-import-job \
  --import-destination "{\"SuppressionListDestination\": {\"SuppressionListImportAction\":\"DELETE\"}}" \
  --import-data-source "{\"S3Url\": \"s3://s3bucket/s3object\",\"DataFormat\": \"CSV\"}"
  ```

------
#### [ Windows ]

  ```
  aws sesv2 create-import-job `
  --import-destination "{\"SuppressionListDestination\": {\"SuppressionListImportAction\":\"DELETE\"}}" `
  --import-data-source "{\"S3Url\": \"s3://s3bucket/s3object\",\"DataFormat\": \"CSV\"}"
  ```

------

  In the preceding examples, replace *s3bucket* and *s3object* with the Amazon S3 bucket name and Amazon S3 object name\.

## Viewing a list of import jobs for the account<a name="sending-email-suppression-list-view-import-jobs"></a>

You can view a list of all of the email addresses that are on the account\-level suppression list for your account by using the [ListImportJobs](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_ListImportJobs.html) operation in the Amazon SES API v2\.

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
            "CreatedTimestamp": 1596175615.804,
            "ImportDestination": {
                "SuppressionListDestination": {
                    "SuppressionListImportAction": "PUT"
                }
            },
            "JobStatus": "COMPLETED",
            "JobId": "755380d7-fbdb-4ed2-a9a3-06866220f5b5"
        },
        {
            "CreatedTimestamp": 1596134732.398,
            "ImportDestination": {
                "SuppressionListDestination": {
                    "SuppressionListImportAction": "DELETE"
                }
            },
            "JobStatus": "COMPLETED",
            "JobId": "076683bd-a7ee-4a40-9754-4ad1161ba8b6"
        },
        {
            "CreatedTimestamp": 1596645918.134,
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
    "CreatedTimestamp": 1597251915.243,
    "FailedRecordsCount": 1,
    "ImportDestination": {
        "SuppressionListDestination": {
            "SuppressionListImportAction": "PUT"
        }
    },
    "CompletedTimestamp": 1597252002.583
}
```

## Disabling the account\-level suppression list<a name="sending-email-suppression-list-disabling"></a>

You can use the [PutAccountSuppressionAttributes](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_PutAccountSuppressionAttributes.html) operation in the Amazon SES API v2 to effectively disable the account\-level suppression list by removing the values from the `suppressed-reasons` attribute\.

**Note**  
The following procedure assumes that you've already installed the AWS CLI\. For more information about installing and configuring the AWS CLI, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\.

**To disable the account\-level suppression list by using the AWS CLI**
+ At the command line, enter the following command:

  ```
  aws sesv2 put-account-suppression-attributes --suppressed-reasons
  ```
