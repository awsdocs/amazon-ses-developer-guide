# Giving permissions to Amazon SES for email receiving<a name="receiving-email-permissions"></a>

Some of the tasks that you can perform when you receive email in Amazon SES, such as sending email to an Amazon S3 bucket or calling a Lambda function, require special permissions\. This section includes example policies for several common use cases\.

**Topics**
+ [Give Amazon SES permission to write to an Amazon S3 bucket](#receiving-email-permissions-s3)
+ [Give Amazon SES permission to use your AWS KMS master key](#receiving-email-permissions-kms)
+ [Give Amazon SES permission to invoke a Lambda function](#receiving-email-permissions-lambda)
+ [Give Amazon SES permission to publish to an Amazon SNS topic that belongs to a different AWS account](#receiving-email-permissions-sns)

## Give Amazon SES permission to write to an Amazon S3 bucket<a name="receiving-email-permissions-s3"></a>

When you apply the following policy to an Amazon S3 bucket, it gives Amazon SES permission to write to that bucket\. For more information about creating receipt rules that transfer incoming email to Amazon S3, see [S3 action](receiving-email-action-s3.md)\.

For more information about attaching policies to Amazon S3 buckets, see [Using Bucket Policies and User Policies](https://docs.aws.amazon.com/AmazonS3/latest/dev/using-iam-policies.html) in the *Amazon Simple Storage Service Developer Guide*\.

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
14.           "aws:Referer":"111122223333"
15.         }
16.       }
17.     }
18.   ]
19. }
```

Make the following changes to the preceding policy example:
+ Replace *myBucket* with the name of the Amazon S3 bucket that you want to write to\.
+ Replace *111122223333* with your AWS account ID\.

## Give Amazon SES permission to use your AWS KMS master key<a name="receiving-email-permissions-kms"></a>

In order for Amazon SES to encrypt your emails, it must have permission to use the AWS KMS key that you specified when you set up your receipt rule\. You can either use the default master key \(**aws/ses**\) in your account, or use a custom master key that you create\. If you use the default master key, you don't need to perform any additional steps to give Amazon SES permission to use it\. If you use a custom master key, you need to give Amazon SES permission to use it by adding a statement to the key's policy\.

Use the following policy statement as the key policy to allow Amazon SES to use your custom master key when it receives email on your domain\.

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

## Give Amazon SES permission to invoke a Lambda function<a name="receiving-email-permissions-lambda"></a>

To enable Amazon SES to call a Lambda function, you can choose the function when you create a receipt rule in the Amazon SES console\. When you do, Amazon SES automatically adds the necessary permissions to the function\.

Alternatively, you can use the `AddPermission` operation in the AWS Lambda API to attach a policy to a function\. The following call to the `AddPermission` API gives Amazon SES permission to invoke your Lambda function\. In the following example, replace *111122223333* with your AWS account ID\. For more information about attaching policies to Lambda functions, see [AWS Lambda Permissions](https://docs.aws.amazon.com/lambda/latest/dg/intro-permission-model.html) in the *AWS Lambda Developer Guide*\.

```
{
  "Action": "lambda:InvokeFunction",
  "Principal": "ses.amazonaws.com",
  "SourceAccount": "111122223333",
  "StatementId": "GiveSESPermissionToInvokeFunction"
}
```

## Give Amazon SES permission to publish to an Amazon SNS topic that belongs to a different AWS account<a name="receiving-email-permissions-sns"></a>

If the Amazon SNS topic you want to use is owned by the same AWS account that you use for Amazon SES, then Amazon SES can publish to that topic without any extra setup steps\. If you want to publish notifications to a topic in a separate AWS account, then you have to attach a policy to the Amazon SNS topic\.

The following policy gives Amazon SES permission to publish to an Amazon SNS topic in a separate AWS account\.

```
{
  "Version":"2008-10-17",
  "Statement":[
    {
      "Effect":"Allow",
      "Principal":{
        "Service":"ses.amazonaws.com"
      },
      "Action":"SNS:Publish",
      "Resource":"arn:aws:sns:us-west-2:SNS-TOPIC-ACCOUNT-ID:myTopic",
      "Condition":{
        "StringEquals":{
          "AWS:SourceOwner":"SES-RECEIVING-ACCOUNT-ID"
        }
      }
    }
  ]
}
```

Make the following changes to the preceding policy example:
+ Replace *us\-west\-2* with the AWS Region that the Amazon SNS topic is located in\.
+ Replace *SNS\-TOPIC\-ACCOUNT\-ID* with the ID of the AWS account that the Amazon SNS topic is located in\.
+ Replace *myTopic* with the name of the Amazon SNS topic that you want to publish notifications to\.
+ Replace *SES\-RECEIVING\-ACCOUNT\-ID* with the ID of the AWS account that is configured to receive email\.