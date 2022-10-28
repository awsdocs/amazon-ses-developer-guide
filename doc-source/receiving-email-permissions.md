# Giving permissions to Amazon SES for email receiving<a name="receiving-email-permissions"></a>

Some of the tasks that you can perform when you receive email in Amazon SES, such as sending email to an Amazon Simple Storage Service \(Amazon S3\) bucket or calling a AWS Lambda function, require special permissions\. This section includes example policies for several common use cases\.

**Topics**
+ [Give Amazon SES permission to write to an S3 bucket](#receiving-email-permissions-s3)
+ [Give Amazon SES permission to use your AWS KMS key](#receiving-email-permissions-kms)
+ [Give Amazon SES permission to invoke a AWS Lambda function](#receiving-email-permissions-lambda)
+ [Give Amazon SES permission to publish to an Amazon SNS topic that belongs to a different AWS account](#receiving-email-permissions-sns)

## Give Amazon SES permission to write to an S3 bucket<a name="receiving-email-permissions-s3"></a>

When you apply the following policy to an S3 bucket, it gives Amazon SES permission to write to that bucket\. For more information about creating receipt rules that transfer incoming email to Amazon S3, see [Deliver to S3 bucket action](receiving-email-action-s3.md)\.

For more information about attaching policies to S3 buckets, see [Using Bucket Policies and User Policies](https://docs.aws.amazon.com/AmazonS3/latest/dev/using-iam-policies.html) in the *Amazon Simple Storage Service User Guide*\.

```
 1. {
 2.   "Version":"2012-10-17",
 3.   "Statement":[
 4.     {
 5.       "Sid":"AllowSESPuts",
 6.       "Effect":"Allow",
 7.       "Principal":{
 8.         "Service":"ses.amazonaws.com"
 9.       },
10.       "Action":"s3:PutObject",
11.       "Resource":"arn:aws:s3:::myBucket/*",
12.       "Condition":{
13.         "StringEquals":{
14.           "AWS:SourceAccount":"111122223333",
15.           "AWS:SourceArn": "arn:aws:ses:region:111122223333:receipt-rule-set/rule_set_name:receipt-rule/receipt_rule_name"
16.         }
17.       }
18.     }
19.   ]
20. }
```

Make the following changes to the preceding policy example:
+ Replace *myBucket* with the name of the S3 bucket that you want to write to\.
+ Replace *region* with the AWS Region where you created the receipt rule\.
+ Replace *111122223333* with your AWS account ID\.
+ Replace *rule\_set\_name* with the name of the rule set that contains the receipt rule that contains the deliver to Amazon S3 bucket action\.
+ Replace *receipt\_rule\_name* with the name of the receipt rule that contains the deliver to Amazon S3 bucket action\.

## Give Amazon SES permission to use your AWS KMS key<a name="receiving-email-permissions-kms"></a>

In order for Amazon SES to encrypt your emails, it must have permission to use the AWS KMS key that you specified when you set up your receipt rule\. You can either use the default KMS key \(**aws/ses**\) in your account, or use a customer managed key that you create\. If you use the default KMS key, you don't need to perform any additional steps to give Amazon SES permission to use it\. If you use a customer managed key, you need to give Amazon SES permission to use it by adding a statement to the key's policy\.

Use the following policy statement as the key policy to allow Amazon SES to use your customer managed key when it receives email on your domain\.

```
{
  "Sid": "AllowSESToEncryptMessagesBelongingToThisAccount", 
  "Effect": "Allow",
  "Principal": {
    "Service":"ses.amazonaws.com"
  },
  "Action": [
    "kms:GenerateDataKey*"
  ],
  "Resource": "*",
  "Condition":{
        "StringEquals":{
          "AWS:SourceAccount":"111122223333",
          "AWS:SourceArn": "arn:aws:ses:region:111122223333:receipt-rule-set/rule_set_name:receipt-rule/receipt_rule_name"
        }
      }
}
```

Make the following changes to the preceding policy example:
+ Replace *region* with the AWS Region where you created the receipt rule\.
+ Replace *111122223333* with your AWS account ID\.
+ Replace *rule\_set\_name* with the name of the rule set that contains the receipt rule that you've associated with email receiving\.
+ Replace *receipt\_rule\_name* with the name of the receipt rule that you've associated with email receiving\.

If you're using AWS KMS to send encrypted messages to an S3 bucket with server\-side encryption enabled, then you need to add the policy action, `"kms:Decrypt"`\. Using the preceding example, adding this action to your policy would appear as follows:

```
{
  "Sid": "AllowSESToEncryptMessagesBelongingToThisAccount", 
  "Effect": "Allow",
  "Principal": {
    "Service":"ses.amazonaws.com"
  },
  "Action": [
    "kms:Decrypt",
    "kms:GenerateDataKey*"
  ],
  "Resource": "*",
  "Condition":{
        "StringEquals":{
          "AWS:SourceAccount":"111122223333",
          "AWS:SourceArn": "arn:aws:ses:region:111122223333:receipt-rule-set/rule_set_name:receipt-rule/receipt_rule_name"
        }
      }
}
```

For more information about attaching policies to AWS KMS keys, see [Using Key Policies in AWS KMS](https://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html) in the *AWS Key Management Service Developer Guide*\.

## Give Amazon SES permission to invoke a AWS Lambda function<a name="receiving-email-permissions-lambda"></a>

To enable Amazon SES to call a AWS Lambda function, you can choose the function when you create a receipt rule in the Amazon SES console\. When you do, Amazon SES automatically adds the necessary permissions to the function\.

Alternatively, you can use the `AddPermission` operation in the AWS Lambda API to attach a policy to a function\. The following call to the `AddPermission` API gives Amazon SES permission to invoke your Lambda function\.  For more information about attaching policies to Lambda functions, see [AWS Lambda Permissions](https://docs.aws.amazon.com/lambda/latest/dg/intro-permission-model.html) in the *AWS Lambda Developer Guide*\.

```
{
  "Action": "lambda:InvokeFunction",
  "Principal": "ses.amazonaws.com",
  "SourceAccount": "111122223333",
  "SourceArn": "arn:aws:ses:region:111122223333:receipt-rule-set/rule_set_name:receipt-rule/receipt_rule_name"
  "StatementId": "GiveSESPermissionToInvokeFunction"
}
```

Make the following changes to the preceding policy example:
+ Replace *region* with the AWS Region where you created the receipt rule\.
+ Replace *111122223333* with your AWS account ID\.
+ Replace *rule\_set\_name* with the name of the rule set that contains the receipt rule where you created your Lambda function\.
+ Replace *receipt\_rule\_name* with the name of the receipt rule containing your Lambda function\.

## Give Amazon SES permission to publish to an Amazon SNS topic that belongs to a different AWS account<a name="receiving-email-permissions-sns"></a>

To publish notifications to a topic in a separate AWS account, you must attach a policy to the Amazon SNS topic\. The SNS topic must be in the same Region as the domain and receipt rule set\.

The following policy gives Amazon SES permission to publish to an Amazon SNS topic in a separate AWS account\.

```
{
  "Version":"2012-10-17",
  "Statement":[
    {
      "Effect":"Allow",
      "Principal":{
        "Service":"ses.amazonaws.com"
      },
      "Action":"SNS:Publish",
      "Resource":"arn:aws:sns:topic_region:sns_topic_account_id:topic_name",
      "Condition":{
        "StringEquals":{
          "AWS:SourceAccount":"aws_account_id",
          "AWS:SourceArn": "arn:aws:ses:receipt_region:aws_account_id:receipt-rule-set/rule_set_name:receipt-rule/receipt_rule_name"
        }
      }
    }
  ]
}
```

Make the following changes to the preceding policy example:
+ Replace *topic\_region* with the AWS Region that the Amazon SNS topic was created in\.
+ Replace *sns\_topic\_account\_id* with the ID of the AWS account that owns the Amazon SNS topic\.
+ Replace *topic\_name* with the name of the Amazon SNS topic that you want to publish notifications to\.
+ Replace *aws\_account\_id* with the ID of the AWS account that is configured to receive email\.
+ Replace *receipt\_region* with the AWS Region where you created the receipt rule\.
+ Replace *rule\_set\_name* with the name of the rule set that contains the receipt rule where you created your publish to Amazon SNS topic action\.
+ Replace *receipt\_rule\_name* with the name of the receipt rule containing the publish to Amazon SNS topic action\.

If your Amazon SNS topic uses AWS KMS for server\-side encryption, you have to add permissions to the AWS KMS key policy\. You can add permissions by attaching the following policy to the AWS KMS key policy:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowSESToUseKMSKey",
            "Effect": "Allow",
            "Principal": {
                "Service": "ses.amazonaws.com"
            },
            "Action": [
                "kms:GenerateDataKey",
                "kms:Decrypt"
            ],
            "Resource": "*"
        }
    ]
}
```