# Giving permissions to Amazon SES for email receiving<a name="receiving-email-permissions"></a>

To enable Amazon SES to write emails to your Amazon S3 bucket, use an AWS KMS key to encrypt your emails, call your Lambda function, or publish to an Amazon SNS topic of another account, Amazon SES must have permission to access those resources\. You give permission by attaching a policy to the resource\. This topic provides example policies\.

## Give Amazon SES permission to write to your Amazon S3 bucket<a name="receiving-email-permissions-s3"></a>

When applied to an Amazon S3 bucket, the following policy gives Amazon SES permission to write to that bucket\. For more information about creating receipt rules that transfer incoming email to Amazon S3, see [S3 action](receiving-email-action-s3.md)\. For more information about attaching policies to Amazon S3 buckets, see [Using Bucket Policies and User Policies](https://docs.aws.amazon.com/AmazonS3/latest/dev/using-iam-policies.html) in the *Amazon Simple Storage Service Developer Guide*\.

```
 1. {
 2.     "Version": "2012-10-17",
 3.     "Statement": [
 4.         {
 5.             "Sid": "AllowSESPuts",
 6.             "Effect": "Allow",
 7.             "Principal": {
 8.                 "Service": "ses.amazonaws.com"
 9.             },
10.             "Action": "s3:PutObject",
11.             "Resource": "arn:aws:s3:::BUCKET-NAME/*",
12.             "Condition": {
13.                 "StringEquals": {
14.                     "aws:Referer": "AWSACCOUNTID"
15.                 }
16.             }
17.         }
18.     ]
19. }
```

## Give Amazon SES permission to use your AWS KMS master key<a name="receiving-email-permissions-kms"></a>

For Amazon SES to encrypt your emails, it must have permission to use the AWS KMS key that you specified when you set up your receipt rule\. You can either use the default master key \(**aws/ses**\) in your account or a custom master key that you create\. If you use the default master key, you don't need to perform any steps to give Amazon SES permission to use it\. If you use a custom master key, you need to give Amazon SES permission to use it by adding a statement to the key's policy\.

Paste the following policy statement into the key policy to permit Amazon SES to use your custom master key when Amazon SES receives email on behalf of your AWS account\.

```
{
   "Sid": "AllowSESToEncryptMessagesBelongingToThisAccount", 
   "Effect": "Allow",
   "Principal": {
       "Service":"ses.amazonaws.com"
   },
   "Action": [
       "kms:Encrypt", 
       "kms:GenerateDataKey*"
   ],
   "Resource": "*"
}
```

**Note**  
Amazon SES uses the Amazon S3 [multipart upload API](https://docs.aws.amazon.com/AmazonS3/latest/dev/mpuoverview.html) to send large messages \(5 MB or larger\) to Amazon S3 buckets\. If you're using AWS KMS to send encrypted messages to an Amazon S3 bucket, and you plan to receive messages that are larger than 5 MB, then you should use the following policy statement instead of the statement in the preceding example:  

```
{
   "Sid": "AllowSESToEncryptMessagesBelongingToThisAccount", 
   "Effect": "Allow",
   "Principal": {
       "Service":"ses.amazonaws.com"
   },
   "Action": [
        "kms:Encrypt", 
        "kms:Decrypt", 
        "kms:ReEncrypt*",
        "kms:GenerateDataKey*",
        "kms:DescribeKey"
   ],
   "Resource": "*"
}
```

For more information about multipart uploads in Amazon S3, see [Multipart Upload API and Permissions](https://docs.aws.amazon.com/AmazonS3/latest/dev/mpuAndPermissions.html) in the *Amazon Simple Storage Service Developer Guide*\. For more information about attaching policies to AWS KMS keys, see [Using Key Policies in AWS KMS](https://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html) in the *AWS Key Management Service Developer Guide*\.

## Give Amazon SES permission to invoke your Lambda function<a name="receiving-email-permissions-lambda"></a>

To enable Amazon SES to call your Lambda function, you can either configure the Lambda function using the Amazon SES console during receipt\-rule setup \(in which case Amazon SES automatically adds the necessary permissions to the function\) or you can use the AWS Lambda `AddPermission` API to attach a policy to the function\. The following `AddPermission` API call gives Amazon SES permission to invoke your Lambda function\. Replace *AWSACCOUNTID* with your 12\-digit AWS account ID\. For more information about attaching policies to Lambda functions, see [AWS Lambda Permissions](https://docs.aws.amazon.com/lambda/latest/dg/intro-permission-model.html) in the *AWS Lambda Developer Guide*\.

```
1. {
2.      "Action": "lambda:InvokeFunction",
3.      "Principal": "ses.amazonaws.com",
4.      "SourceAccount": "AWSACCOUNTID",
5.      "StatementId": "GiveSESPermissionToInvokeFunction"
6. }
```

## Give Amazon SES permission to publish to an Amazon SNS topic that belongs to a different AWS account<a name="receiving-email-permissions-sns"></a>

If the Amazon SNS topic you want to use is owned by the same AWS account you are using for Amazon SES, no setup is required to allow Amazon SES to publish to the topic\. However, if you want to publish notifications to a topic that you do not own, use the Amazon SNS console or API to attach a policy to the Amazon SNS topic\. The following policy gives Amazon SES permission to publish to an Amazon SNS topic\. Replace *AWSACCOUNTID* with your 12\-digit AWS account ID, and *TOPIC\-NAME* with the name of the Amazon SNS topic\. For more information about writing policies for Amazon SNS topics, see [Authentication and Access Control for Amazon SNS](https://docs.aws.amazon.com/sns/latest/dg/AccessPolicyLanguage.html) in the *Amazon Simple Notification Service Developer Guide*\.

```
 1. {
 2.     "Version": "2008-10-17",
 3.     "Statement": [
 4.      {
 5.           "Effect": "Allow",
 6.           "Principal": {
 7.               "Service": "ses.amazonaws.com"
 8.            },
 9.            "Action": "SNS:Publish",
10.            "Resource": "arn:aws:sns:us-east-1:AWSACCOUNTID:TOPIC-NAME"
11.       }
12.    ]
13. }
```