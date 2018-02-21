# Logging Amazon SES API Calls By Using AWS CloudTrail<a name="logging-using-cloudtrail"></a>

Amazon SES is integrated with CloudTrail, a service that captures API calls made by or on behalf of Amazon SES in your AWS account and delivers the log files to an Amazon S3 bucket that you specify\. CloudTrail captures API calls made from the Amazon SES console or from the Amazon SES API\. Using the information collected by CloudTrail, you can determine what request was made to Amazon SES, the source IP address from which the request was made, who made the request, when it was made, and so on\. To learn more about CloudTrail, including how to configure and enable it, see the [AWS CloudTrail User Guide](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/)\.

## Amazon SES Information in CloudTrail<a name="service-name-info-in-cloudtrail"></a>

When CloudTrail logging is enabled in your AWS account, API calls made to a subset of Amazon SES actions are tracked in log files\. Amazon SES records are written together with other AWS service records in a log file\. CloudTrail determines when to create and write to a new file based on a time period and file size\.

The following actions are supported:

+ [CloneReceiptRuleSet](http://docs.aws.amazon.com/ses/latest/APIReference/API_CloneReceiptRuleSet.html)

+ [CreateReceiptFilter](http://docs.aws.amazon.com/ses/latest/APIReference/API_CreateReceiptFilter.html)

+ [CreateReceiptRule](http://docs.aws.amazon.com/ses/latest/APIReference/API_CreateReceiptRule.html)

+ [CreateReceiptRuleSet](http://docs.aws.amazon.com/ses/latest/APIReference/API_CreateReceiptRuleSet.html)

+ [DeleteIdentity](http://docs.aws.amazon.com/ses/latest/APIReference/API_DeleteIdentity.html)

+ [DeleteIdentityPolicy](http://docs.aws.amazon.com/ses/latest/APIReference/API_DeleteIdentityPolicy.html)

+ [DeleteReceiptFilter](http://docs.aws.amazon.com/ses/latest/APIReference/API_DeleteReceiptFilter.html)

+ [DeleteReceiptRule](http://docs.aws.amazon.com/ses/latest/APIReference/API_DeleteReceiptRule.html)

+ [DeleteReceiptRuleSet](http://docs.aws.amazon.com/ses/latest/APIReference/API_DeleteReceiptRuleSet.html)

+ [DeleteVerifiedEmailAddress](http://docs.aws.amazon.com/ses/latest/APIReference/API_DeleteVerifiedEmailAddress.html)

+ [DescribeActiveReceiptRuleSet](http://docs.aws.amazon.com/ses/latest/APIReference/API_DescribeActiveReceiptRuleSet.html)

+ [DescribeReceiptRule](http://docs.aws.amazon.com/ses/latest/APIReference/API_DescribeReceiptRule.html)

+ [DescribeReceiptRuleSet](http://docs.aws.amazon.com/ses/latest/APIReference/API_DescribeReceiptRuleSet.html)

+ [GetIdentityDkimAttributes](http://docs.aws.amazon.com/ses/latest/APIReference/API_GetIdentityDkimAttributes.html)

+ [GetIdentityNotificationAttributes](http://docs.aws.amazon.com/ses/latest/APIReference/API_GetIdentityNotificationAttributes.html)

+ [GetIdentityPolicies](http://docs.aws.amazon.com/ses/latest/APIReference/API_GetIdentityPolicies.html)

+ [GetIdentityVerificationAttributes](http://docs.aws.amazon.com/ses/latest/APIReference/API_GetIdentityVerificationAttributes.html)

+ [GetSendQuota](http://docs.aws.amazon.com/ses/latest/APIReference/API_GetSendQuota.html)

+ [GetSendStatistics](http://docs.aws.amazon.com/ses/latest/APIReference/API_GetSendStatistics.html)

+ [ListIdentities](http://docs.aws.amazon.com/ses/latest/APIReference/API_ListIdentities.html)

+ [ListIdentityPolicies](http://docs.aws.amazon.com/ses/latest/APIReference/API_ListIdentityPolicies.html)

+ [ListReceiptFilters](http://docs.aws.amazon.com/ses/latest/APIReference/API_ListReceiptFilters.html)

+ [ListReceiptRuleSets](http://docs.aws.amazon.com/ses/latest/APIReference/API_ListReceiptRuleSets.html)

+ [ListVerifiedEmailAddresses](http://docs.aws.amazon.com/ses/latest/APIReference/API_ListVerifiedEmailAddresses.html)

+ [PutIdentityPolicy](http://docs.aws.amazon.com/ses/latest/APIReference/API_PutIdentityPolicy.html)

+ [ReorderReceiptRuleSet](http://docs.aws.amazon.com/ses/latest/APIReference/API_ReorderReceiptRuleSet.html)

+ [SetActiveReceiptRuleSet](http://docs.aws.amazon.com/ses/latest/APIReference/API_SetActiveReceiptRuleSet.html)

+ [SetReceiptRulePosition](http://docs.aws.amazon.com/ses/latest/APIReference/API_SetReceiptRulePosition.html)

+ [SetIdentityDkimEnabled](http://docs.aws.amazon.com/ses/latest/APIReference/API_SetIdentityDkimEnabled.html)

+ [SetIdentityFeedbackForwardingEnabled](http://docs.aws.amazon.com/ses/latest/APIReference/API_SetIdentityFeedbackForwardingEnabled.html)

+ [SetIdentityHeadersInNotificationsEnabled](http://docs.aws.amazon.com/ses/latest/APIReference/API_SetIdentityHeadersInNotificationsEnabled.html)

+ [SetIdentityNotificationTopic](http://docs.aws.amazon.com/ses/latest/APIReference/API_SetIdentityNotificationTopic.html)

+ [UpdateReceiptRule](http://docs.aws.amazon.com/ses/latest/APIReference/API_UpdateReceiptRule.html)

+ [VerifyDomainDkim](http://docs.aws.amazon.com/ses/latest/APIReference/API_VerifyDomainDkim.html)

+ [VerifyDomainIdentity](http://docs.aws.amazon.com/ses/latest/APIReference/API_VerifyDomainIdentity.html)

+ [VerifyEmailAddress](http://docs.aws.amazon.com/ses/latest/APIReference/API_VerifyEmailAddress.html)

+ [VerifyEmailIdentity](http://docs.aws.amazon.com/ses/latest/APIReference/API_VerifyEmailIdentity.html)

Every log entry contains information about who generated the request\. The user identity information in the log helps you determine whether the request was made with root or IAM user credentials, with temporary security credentials for a role or federated user, or by another AWS service\. For more information, see the **userIdentity** field in the [CloudTrail Event Reference](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/event_reference_top_level.html)\.

You can store your log files in your bucket for as long as you want, but you can also define Amazon S3 lifecycle rules to archive or delete log files automatically\. By default, your log files are encrypted by using Amazon S3 server\-side encryption \(SSE\)\.

You can choose to have CloudTrail publish Amazon SNS notifications when new log files are delivered if you want to take quick action upon log file delivery\. For more information, see [Configuring Amazon SNS Notifications](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/getting_notifications_top_level.html)\.

You can also aggregate Amazon SES log files from multiple AWS regions and multiple AWS accounts into a single Amazon S3 bucket\. For more information, see [Aggregating CloudTrail Log Files to a Single Amazon S3 Bucket](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/aggregating_logs_top_level.html)\.

## Understanding Amazon SES Log File Entries<a name="understanding-service-name-entries"></a>

CloudTrail log files contain one or more log entries where each entry is made up of multiple JSON\-formatted events\. A log entry represents a single request from any source and includes information about the requested action, any parameters, the date and time of the action, and so on\. The log entries are not guaranteed to be in any particular order\. That is, they are not an ordered stack trace of the public API calls\.

The following example shows a CloudTrail log\.

```
{
   "Records": [
      {
        "awsRegion": "us-west-2", 
        "eventID": "0ffa308d-1467-4259-8be3-c749753be325", 
        "eventName": "DeleteIdentity", 
        "eventSource": "ses.amazonaws.com", 
        "eventTime": "2015-02-02T21:34:50Z", 
        "eventType": "AwsApiCall", 
        "eventVersion": "1.02", 
        "recipientAccountId": "111122223333", 
        "requestID": "50b87bfe-ab23-11e4-9106-5b36376f9d12", 
        "requestParameters": {
            "identity": "amazon.com"
        }, 
        "responseElements": null, 
        "sourceIPAddress": "192.0.2.0", 
        "userAgent": "aws-sdk-java/unknown-version", 
        "userIdentity": {
            "accessKeyId": "AKIAIOSFODNN7EXAMPLE", 
            "accountId": "111122223333", 
            "arn": "arn:aws:iam::111122223333:root", 
            "principalId": "111122223333", 
            "type": "Root"
        }
      },             
      {
        "awsRegion": "us-west-2", 
        "eventID": "17bb827a-dc8c-4156-90b1-c214e1d135c9", 
        "eventName": "DeleteVerifiedEmailAddress", 
        "eventSource": "ses.amazonaws.com", 
        "eventTime": "2015-02-04T00:57:15Z", 
        "eventType": "AwsApiCall", 
        "eventVersion": "1.02", 
        "recipientAccountId": "111122223333", 
        "requestID": "c29fb5c1-ac08-11e4-8ff5-a56a3119e253", 
        "requestParameters": {
            "emailAddress": "user@example.com"
        }, 
        "responseElements": null, 
        "sourceIPAddress": "192.0.2.0", 
        "userAgent": "aws-sdk-java/unknown-version", 
        "userIdentity": {
            "accessKeyId": "AKIAIOSFODNN7EXAMPLE", 
            "accountId": "111122223333", 
            "arn": "arn:aws:iam::111122223333:root", 
            "principalId": "111122223333", 
            "type": "Root"
        }
      }, 
      {
        "awsRegion": "us-west-2", 
        "eventID": "0b311e38-b5c6-43b3-9a39-5fbf0c2d0d99", 
        "eventName": "GetIdentityDkimAttributes", 
        "eventSource": "ses.amazonaws.com", 
        "eventTime": "2015-02-02T21:34:50Z", 
        "eventType": "AwsApiCall", 
        "eventVersion": "1.02", 
        "recipientAccountId": "111122223333", 
        "requestID": "50f92e80-ab23-11e4-9106-5b36376f9d12", 
        "requestParameters": {
            "identities": [
            "example.com"
            ]
        }, 
        "responseElements": null, 
        "sourceIPAddress": "192.0.2.0", 
        "userAgent": "aws-sdk-java/unknown-version", 
        "userIdentity": {
            "accessKeyId": "AKIAIOSFODNN7EXAMPLE", 
            "accountId": "111122223333", 
            "arn": "arn:aws:iam::111122223333:root", 
            "principalId": "111122223333", 
            "type": "Root"
        }
      },     
      {
        "awsRegion": "us-west-2", 
        "eventID": "bf695be8-1c67-45b0-8f10-fd56afee09dd", 
        "eventName": "GetIdentityNotificationAttributes", 
        "eventSource": "ses.amazonaws.com", 
        "eventTime": "2015-02-02T21:34:50Z", 
        "eventType": "AwsApiCall", 
        "eventVersion": "1.02", 
        "recipientAccountId": "111122223333", 
        "requestID": "5133ed92-ab23-11e4-9106-5b36376f9d12", 
        "requestParameters": {
            "identities": [
            "example.com"
            ]
        }, 
        "responseElements": null, 
        "sourceIPAddress": "192.0.2.0", 
        "userAgent": "aws-sdk-java/unknown-version", 
        "userIdentity": {
            "accessKeyId": "AKIAIOSFODNN7EXAMPLE", 
            "accountId": "111122223333", 
            "arn": "arn:aws:iam::111122223333:root", 
            "principalId": "111122223333", 
            "type": "Root"
        }
      },         
      {
        "awsRegion": "us-west-2", 
        "eventID": "8f9aed63-b03a-4d30-a880-33ae0c6b7786", 
        "eventName": "GetIdentityVerificationAttributes", 
        "eventSource": "ses.amazonaws.com", 
        "eventTime": "2015-02-04T00:57:16Z", 
        "eventType": "AwsApiCall", 
        "eventVersion": "1.02", 
        "recipientAccountId": "111122223333", 
        "requestID": "c2d23773-ac08-11e4-8ff5-a56a3119e253", 
        "requestParameters": {
            "identities": [
                "example.com"
            ]
        }, 
        "responseElements": null, 
        "sourceIPAddress": "192.0.2.0", 
        "userAgent": "aws-sdk-java/unknown-version", 
        "userIdentity": {
            "accessKeyId": "AKIAIOSFODNN7EXAMPLE", 
            "accountId": "111122223333", 
            "arn": "arn:aws:iam::111122223333:root", 
            "principalId": "111122223333", 
            "type": "Root"
        }
      }, 
      {
        "awsRegion": "us-west-2", 
        "eventID": "60ef4f01-9826-4fb4-828e-8c36dda81f40", 
        "eventName": "GetSendQuota", 
        "eventSource": "ses.amazonaws.com", 
        "eventTime": "2015-02-04T01:03:27Z", 
        "eventType": "AwsApiCall", 
        "eventVersion": "1.02", 
        "recipientAccountId": "111122223333", 
        "requestID": "a0760648-ac09-11e4-8ff5-a56a3119e253", 
        "requestParameters": null, 
        "responseElements": null, 
        "sourceIPAddress": "192.0.2.0", 
        "userAgent": "aws-sdk-java/unknown-version", 
        "userIdentity": {
            "accessKeyId": "AKIAIOSFODNN7EXAMPLE", 
            "accountId": "111122223333", 
            "arn": "arn:aws:iam::111122223333:root", 
            "principalId": "111122223333", 
            "type": "Root"
        }
      },                      
      {
        "awsRegion": "us-west-2", 
        "eventID": "0fe5eef3-0c28-4480-808e-307b21404a78", 
        "eventName": "GetSendStatistics", 
        "eventSource": "ses.amazonaws.com", 
        "eventTime": "2015-02-02T21:34:51Z", 
        "eventType": "AwsApiCall", 
        "eventVersion": "1.02", 
        "recipientAccountId": "111122223333", 
        "requestID": "51644c64-ab23-11e4-9106-5b36376f9d12", 
        "requestParameters": null, 
        "responseElements": null, 
        "sourceIPAddress": "192.0.2.0", 
        "userAgent": "aws-sdk-java/unknown-version", 
        "userIdentity": {
            "accessKeyId": "AKIAIOSFODNN7EXAMPLE", 
            "accountId": "111122223333", 
            "arn": "arn:aws:iam::111122223333:root", 
            "principalId": "111122223333", 
            "type": "Root"
        }
      },                                   
      {
        "awsRegion": "us-west-2", 
        "eventID": "6eb8178e-69c3-4a93-8af0-2a5a0f5f209e", 
        "eventName": "ListIdentities", 
        "eventSource": "ses.amazonaws.com", 
        "eventTime": "2015-02-04T01:03:27Z", 
        "eventType": "AwsApiCall", 
        "eventVersion": "1.02", 
        "recipientAccountId": "111122223333", 
        "requestID": "a0a4de7a-ac09-11e4-8ff5-a56a3119e253", 
        "requestParameters": {
            "identityType": "Domain", 
            "maxItems": 10
        }, 
        "responseElements": null, 
        "sourceIPAddress": "192.0.2.0", 
        "userAgent": "aws-sdk-java/unknown-version", 
        "userIdentity": {
            "accessKeyId": "AKIAIOSFODNN7EXAMPLE", 
            "accountId": "111122223333", 
            "arn": "arn:aws:iam::111122223333:root", 
            "principalId": "111122223333", 
            "type": "Root"
        }
      },         
      {
        "awsRegion": "us-west-2", 
        "eventID": "a18a9745-d06a-43e9-aad0-8eee4de50f48", 
        "eventName": "ListVerifiedEmailAddresses", 
        "eventSource": "ses.amazonaws.com", 
        "eventTime": "2015-02-02T21:34:51Z", 
        "eventType": "AwsApiCall", 
        "eventVersion": "1.02", 
        "recipientAccountId": "111122223333", 
        "requestID": "51ad8a66-ab23-11e4-9106-5b36376f9d12", 
        "requestParameters": null, 
        "responseElements": null, 
        "sourceIPAddress": "192.0.2.0", 
        "userAgent": "aws-sdk-java/unknown-version", 
        "userIdentity": {
            "accessKeyId": "AKIAIOSFODNN7EXAMPLE", 
            "accountId": "111122223333", 
            "arn": "arn:aws:iam::111122223333:root", 
            "principalId": "111122223333", 
            "type": "Root"
        }
      },                       
      {
        "awsRegion": "us-west-2", 
        "eventID": "da975f45-e68b-4499-8e3f-31a89140e0c9", 
        "eventName": "SetIdentityDkimEnabled", 
        "eventSource": "ses.amazonaws.com", 
        "eventTime": "2015-02-04T01:01:24Z", 
        "eventType": "AwsApiCall", 
        "eventVersion": "1.02", 
        "recipientAccountId": "111122223333", 
        "requestID": "5731c4ab-ac09-11e4-8ff5-a56a3119e253", 
        "requestParameters": {
            "dkimEnabled": true, 
            "identity": "example.com"
        }, 
        "responseElements": null, 
        "sourceIPAddress": "192.0.2.0", 
        "userAgent": "aws-sdk-java/unknown-version", 
        "userIdentity": {
            "accessKeyId": "AKIAIOSFODNN7EXAMPLE", 
            "accountId": "111122223333", 
            "arn": "arn:aws:iam::111122223333:root", 
            "principalId": "111122223333", 
            "type": "Root"
        }
      },           
      {
        "awsRegion": "us-west-2", 
        "eventID": "5d817126-dadb-436f-b480-f9843289f487", 
        "eventName": "SetIdentityFeedbackForwardingEnabled", 
        "eventSource": "ses.amazonaws.com", 
        "eventTime": "2015-02-02T21:34:51Z", 
        "eventType": "AwsApiCall", 
        "eventVersion": "1.02", 
        "recipientAccountId": "111122223333", 
        "requestID": "51dd4cf8-ab23-11e4-9106-5b36376f9d12", 
        "requestParameters": {
            "forwardingEnabled": true, 
            "identity": "example.com"
        }, 
        "responseElements": null, 
        "sourceIPAddress": "192.0.2.0", 
        "userAgent": "aws-sdk-java/unknown-version", 
        "userIdentity": {
            "accessKeyId": "AKIAIOSFODNN7EXAMPLE", 
            "accountId": "111122223333", 
            "arn": "arn:aws:iam::111122223333:root", 
            "principalId": "111122223333", 
            "type": "Root"
        }
      },             
      {
        "awsRegion": "us-west-2", 
        "eventID": "5d817126-dadb-436f-b480-f9843289f487", 
        "eventName": "SetIdentityHeadersInNotificationsEnabled", 
        "eventSource": "ses.amazonaws.com", 
        "eventTime": "2015-02-02T21:34:51Z", 
        "eventType": "AwsApiCall", 
        "eventVersion": "1.02", 
        "recipientAccountId": "111122223333", 
        "requestID": "51dd4cf8-ab23-11e4-9106-5b36376f9d12", 
        "requestParameters": {
            "enabled": true, 
            "identity": "example.com",
            "notificationType": "Bounce"
        }, 
        "responseElements": null, 
        "sourceIPAddress": "192.0.2.0", 
        "userAgent": "aws-sdk-java/unknown-version", 
        "userIdentity": {
            "accessKeyId": "AKIAIOSFODNN7EXAMPLE", 
            "accountId": "111122223333", 
            "arn": "arn:aws:iam::111122223333:root", 
            "principalId": "111122223333", 
            "type": "Root"
        }
      },   
      {
        "awsRegion": "us-west-2", 
        "eventID": "1a31fd43-55ba-4ce7-b3fe-55659e8144c0", 
        "eventName": "SetIdentityNotificationTopic", 
        "eventSource": "ses.amazonaws.com", 
        "eventTime": "2015-02-04T00:59:21Z", 
        "eventType": "AwsApiCall", 
        "eventVersion": "1.02", 
        "recipientAccountId": "111122223333", 
        "requestID": "0d553aac-ac09-11e4-8ff5-a56a3119e253", 
        "requestParameters": {
            "identity": "example.com", 
            "notificationType": "Bounce", 
            "snsTopic": "arn:aws:sns:us-west-2:123456789100:MyTopic"
        }, 
        "responseElements": null, 
        "sourceIPAddress": "192.0.2.0", 
        "userAgent": "aws-sdk-java/unknown-version", 
        "userIdentity": {
            "accessKeyId": "AKIAIOSFODNN7EXAMPLE", 
            "accountId": "111122223333", 
            "arn": "arn:aws:iam::111122223333:root", 
            "principalId": "111122223333", 
            "type": "Root"
        }
      },                                    
      {
        "awsRegion": "us-west-2", 
        "eventID": "aec73edb-6dac-4503-81bb-cca1102f959e", 
        "eventName": "VerifyDomainDkim", 
        "eventSource": "ses.amazonaws.com", 
        "eventTime": "2015-02-02T21:34:52Z", 
        "eventType": "AwsApiCall", 
        "eventVersion": "1.02", 
        "recipientAccountId": "111122223333", 
        "requestID": "52215ada-ab23-11e4-9106-5b36376f9d12", 
        "requestParameters": {
            "domain": "example.com"
        }, 
        "responseElements": {
            "dkimTokens": [
            "3r2ultrqtelopya3v2apjulcvz7z5n5o", 
            "yexya47xmy5f3j3e7vgm6pcrcmayu6nu", 
            "wtlduqduorhmb2vdt2m53yqlcj2m6tpw"
            ]
        }, 
        "sourceIPAddress": "192.0.2.0", 
        "userAgent": "aws-sdk-java/unknown-version", 
        "userIdentity": {
            "accessKeyId": "AKIAIOSFODNN7EXAMPLE", 
            "accountId": "111122223333", 
            "arn": "arn:aws:iam::111122223333:root", 
            "principalId": "111122223333", 
            "type": "Root"
        }
      },             
      {
        "awsRegion": "us-west-2", 
        "eventID": "33b3e2eb-7ba3-460b-a127-a5f4cedb4469", 
        "eventName": "VerifyDomainIdentity", 
        "eventSource": "ses.amazonaws.com", 
        "eventTime": "2015-02-04T00:59:21Z", 
        "eventType": "AwsApiCall", 
        "eventVersion": "1.02", 
        "recipientAccountId": "111122223333", 
        "requestID": "0d9c2ebe-ac09-11e4-8ff5-a56a3119e253", 
        "requestParameters": {
            "disableEmailNotifications": false, 
            "domain": "example.com"
        }, 
        "responseElements": {
            "verificationToken": "pmBGN/7MjnfhTKUZ06Enqq1PeGUaOkw8lGhcfwefcHU="
        }, 
        "sourceIPAddress": "192.0.2.0", 
        "userAgent": "aws-sdk-java/unknown-version", 
        "userIdentity": {
            "accessKeyId": "AKIAIOSFODNN7EXAMPLE", 
            "accountId": "111122223333", 
            "arn": "arn:aws:iam::111122223333:root", 
            "principalId": "111122223333", 
            "type": "Root"
        }
      },
      {
        "awsRegion": "us-west-2", 
        "eventID": "eb2e1616-2b7b-4cd2-b6dc-29f83fc1789f", 
        "eventName": "VerifyEmailAddress", 
        "eventSource": "ses.amazonaws.com", 
        "eventTime": "2015-02-02T21:34:53Z", 
        "eventType": "AwsApiCall", 
        "eventVersion": "1.02", 
        "recipientAccountId": "111122223333", 
        "requestID": "5265ddec-ab23-11e4-9106-5b36376f9d12", 
        "requestParameters": {
            "emailAddress": "user@example.com"
        }, 
        "responseElements": null, 
        "sourceIPAddress": "192.0.2.0", 
        "userAgent": "aws-sdk-java/unknown-version", 
        "userIdentity": {
            "accessKeyId": "AKIAIOSFODNN7EXAMPLE", 
            "accountId": "111122223333", 
            "arn": "arn:aws:iam::111122223333:root", 
            "principalId": "111122223333", 
            "type": "Root"
        }
      },                      
      {
        "awsRegion": "us-west-2", 
        "eventID": "5613b0ff-d6c6-4526-9b53-a603a9231725", 
        "eventName": "VerifyEmailIdentity", 
        "eventSource": "ses.amazonaws.com", 
        "eventTime": "2015-02-04T01:05:33Z", 
        "eventType": "AwsApiCall", 
        "eventVersion": "1.02", 
        "recipientAccountId": "111122223333", 
        "requestID": "eb2ff803-ac09-11e4-8ff5-a56a3119e253", 
        "requestParameters": {
            "emailAddress": "user@example.com"
        }, 
        "responseElements": null, 
        "sourceIPAddress": "192.0.2.0", 
        "userAgent": "aws-sdk-java/unknown-version", 
        "userIdentity": {
            "accessKeyId": "AKIAIOSFODNN7EXAMPLE", 
            "accountId": "111122223333", 
            "arn": "arn:aws:iam::111122223333:root", 
            "principalId": "111122223333", 
            "type": "Root"
      }
     }
   ]
}
```