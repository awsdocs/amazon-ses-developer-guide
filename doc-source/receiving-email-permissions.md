# Giving Permissions to Amazon SES for Email Receiving<a name="receiving-email-permissions"></a>

To enable Amazon SES to write emails to your Amazon S3 bucket, use an AWS KMS key to encrypt your emails, call your Lambda function, or publish to an Amazon SNS topic of another account, Amazon SES must have permission to access those resources\. You give permission by attaching a policy to the resource\. This topic provides example policies\.

## Give Amazon SES Permission to Write to Your Amazon S3 Bucket<a name="receiving-email-permissions-s3"></a>

When applied to an Amazon S3 bucket, the following policy gives Amazon SES permission to write to that bucket\. For more information about creating receipt rules that transfer incoming email to Amazon S3, see [S3 Action](receiving-email-action-s3.md)\. For more information about attaching policies to Amazon S3 buckets, see the *[Amazon Simple Storage Service Developer Guide](https://docs.aws.amazon.com/AmazonS3/latest/dev/using-iam-policies.html.html)*\.

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

## Give Amazon SES Permission to Use Your AWS KMS Master Key<a name="receiving-email-permissions-kms"></a>

For Amazon SES to encrypt your emails, it must have permission to use the AWS KMS key that you specified when you set up your receipt rule\. You can either use the default master key \(**aws/ses**\) in your account or a custom master key you create\. If you use the default master key, you don't need to perform any steps to give Amazon SES permission to use it\. If you use a custom master key, you need to give Amazon SES permission to use it by adding a statement to the key's policy\. The policy statement includes conditions that are designed to ensure that Amazon SES can only use your custom master key when certain values are present in the request to AWS KMS; specifically:
+ `aws:ses:source-account—`The AWS account ID on behalf of which Amazon SES received the email\.
+ `aws:ses:message-id—`The Amazon SES message ID of the received email\.
+ `aws:ses:rule-name—`The name of the receipt rule that was used to encrypt the email\.

Paste the following policy statement into the key policy to permit Amazon SES to use your custom master key when Amazon SES receives email on behalf of your AWS account\. Replace `AWSACCOUNTID` with your 12\-digit AWS account ID\.

```
 1. {
 2.    "Sid": "AllowSESToEncryptMessagesBelongingToThisAccount",
 3.    "Effect": "Allow",
 4.    "Principal": {"Service":"ses.amazonaws.com"},
 5.    "Action": ["kms:Encrypt", "kms:GenerateDataKey*"],
 6.    "Resource": "*",
 7.    "Condition": {
 8.        "Null": {
 9.             "kms:EncryptionContext:aws:ses:rule-name": "false",
10.             "kms:EncryptionContext:aws:ses:message-id": "false"
11.         },
12.         "StringEquals": {
13.               "kms:EncryptionContext:aws:ses:source-account": "AWSACCOUNTID"
14.         }
15.    }
16. }
```

For more information about attaching policies to AWS KMS keys, see the *[AWS Key Management Service Developer Guide](https://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html)*\.

## Give Amazon SES Permission to Invoke Your Lambda Function<a name="receiving-email-permissions-lambda"></a>

To enable Amazon SES to call your Lambda function, you can either configure the Lambda function using the Amazon SES console during receipt\-rule setup \(in which case Amazon SES automatically adds the necessary permissions to the function\) or you can use the AWS Lambda `AddPermission` API to attach a policy to the function\. The following `AddPermission` API call gives Amazon SES permission to invoke your Lambda function\. Replace `AWSACCOUNTID` with your 12\-digit AWS account ID\. For more information about attaching policies to Lambda functions, see the *[AWS Lambda Developer Guide](https://docs.aws.amazon.com/lambda/latest/dg/intro-permission-model.html)*\.

```
1. {
2.      "Action": "lambda:InvokeFunction",
3.      "Principal": "ses.amazonaws.com",
4.      "SourceAccount": "AWSACCOUNTID",
5.      "StatementId": "GiveSESPermissionToInvokeFunction"
6. }
```

## Give Amazon SES Permission to Publish to an Amazon SNS Topic of Another Account<a name="receiving-email-permissions-sns"></a>

If the Amazon SNS topic you want to use is owned by the same AWS account you are using for Amazon SES, no setup is required to allow Amazon SES to publish to the topic\. However, if you want to publish notifications to a topic that you do not own, use the Amazon SNS console or API to attach a policy to the Amazon SNS topic\. The following policy gives Amazon SES permission to publish to an Amazon SNS topic\. Replace `AWSACCOUNTID` with your 12\-digit AWS account ID, and `TOPIC-NAME` with the name of the Amazon SNS topic\. For more information about writing policies for Amazon SNS topics, see the *[Amazon Simple Notification Service Developer Guide](https://docs.aws.amazon.com/sns/latest/dg/AccessPolicyLanguage.html)*\.

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