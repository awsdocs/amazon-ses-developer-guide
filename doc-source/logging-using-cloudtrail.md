# Logging Amazon SES API calls with AWS CloudTrail<a name="logging-using-cloudtrail"></a>

Amazon SES is integrated with AWS CloudTrail, a service that provides a record of actions taken by a user, role, or an AWS service in Amazon SES\. CloudTrail captures API calls for Amazon SES as events\. The calls captured include calls from the Amazon SES console and code calls to the Amazon SES API operations\. If you create a trail, you can enable continuous delivery of CloudTrail events to an Amazon S3 bucket, including events for Amazon SES\. If you don't configure a trail, you can still view the most recent events in the CloudTrail console in **Event history**\. Using the information collected by CloudTrail, you can determine the request that was made to Amazon SES, the IP address from which the request was made, who made the request, when it was made, and additional details\. 

To learn more about CloudTrail, including how to configure and enable it, see the [AWS CloudTrail User Guide](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/)\.

## Amazon SES Information in CloudTrail<a name="service-name-info-in-cloudtrail"></a>

CloudTrail is enabled on your AWS account when you create the account\. When supported event activity occurs in Amazon SES, that activity is recorded in a CloudTrail event along with other AWS service events in **Event history**\. You can view, search, and download recent events in your AWS account\. For more information, see [Viewing Events with CloudTrail Event History](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/view-cloudtrail-events.html)\. 

For an ongoing record of events in your AWS account, including events for Amazon SES, create a trail\. A *trail* enables CloudTrail to deliver log files to an Amazon S3 bucket\. By default, when you create a trail in the console, the trail applies to all AWS Regions\. The trail logs events from all Regions in the AWS partition and delivers the log files to the Amazon S3 bucket that you specify\. Additionally, you can configure other AWS services to further analyze and act upon the event data collected in CloudTrail logs\. For more information, see the following: 
+ [Overview for Creating a Trail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-create-and-update-a-trail.html)
+ [CloudTrail Supported Services and Integrations](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-aws-service-specific-topics.html#cloudtrail-aws-service-specific-topics-integrations)
+ [Configuring Amazon SNS Notifications for CloudTrail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/getting_notifications_top_level.html)
+ [Receiving CloudTrail Log Files from Multiple Regions](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/receive-cloudtrail-log-files-from-multiple-regions.html) and [Receiving CloudTrail Log Files from Multiple Accounts](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-receive-logs-from-multiple-accounts.html)

Amazon SES supports logging the following actions as events in CloudTrail log files:
+ [CloneReceiptRuleSet](https://docs.aws.amazon.com/ses/latest/APIReference/API_CloneReceiptRuleSet.html)
+ [CreateReceiptFilter](https://docs.aws.amazon.com/ses/latest/APIReference/API_CreateReceiptFilter.html)
+ [CreateReceiptRule](https://docs.aws.amazon.com/ses/latest/APIReference/API_CreateReceiptRule.html)
+ [CreateReceiptRuleSet](https://docs.aws.amazon.com/ses/latest/APIReference/API_CreateReceiptRuleSet.html)
+ [DeleteIdentity](https://docs.aws.amazon.com/ses/latest/APIReference/API_DeleteIdentity.html)
+ [DeleteIdentityPolicy](https://docs.aws.amazon.com/ses/latest/APIReference/API_DeleteIdentityPolicy.html)
+ [DeleteReceiptFilter](https://docs.aws.amazon.com/ses/latest/APIReference/API_DeleteReceiptFilter.html)
+ [DeleteReceiptRule](https://docs.aws.amazon.com/ses/latest/APIReference/API_DeleteReceiptRule.html)
+ [DeleteReceiptRuleSet](https://docs.aws.amazon.com/ses/latest/APIReference/API_DeleteReceiptRuleSet.html)
+ [DeleteVerifiedEmailAddress](https://docs.aws.amazon.com/ses/latest/APIReference/API_DeleteVerifiedEmailAddress.html)
+ [DescribeActiveReceiptRuleSet](https://docs.aws.amazon.com/ses/latest/APIReference/API_DescribeActiveReceiptRuleSet.html)
+ [DescribeReceiptRule](https://docs.aws.amazon.com/ses/latest/APIReference/API_DescribeReceiptRule.html)
+ [DescribeReceiptRuleSet](https://docs.aws.amazon.com/ses/latest/APIReference/API_DescribeReceiptRuleSet.html)
+ [GetIdentityDkimAttributes](https://docs.aws.amazon.com/ses/latest/APIReference/API_GetIdentityDkimAttributes.html)
+ [GetIdentityNotificationAttributes](https://docs.aws.amazon.com/ses/latest/APIReference/API_GetIdentityNotificationAttributes.html)
+ [GetIdentityPolicies](https://docs.aws.amazon.com/ses/latest/APIReference/API_GetIdentityPolicies.html)
+ [GetIdentityVerificationAttributes](https://docs.aws.amazon.com/ses/latest/APIReference/API_GetIdentityVerificationAttributes.html)
+ [GetSendQuota](https://docs.aws.amazon.com/ses/latest/APIReference/API_GetSendQuota.html)
+ [GetSendStatistics](https://docs.aws.amazon.com/ses/latest/APIReference/API_GetSendStatistics.html)
+ [ListIdentities](https://docs.aws.amazon.com/ses/latest/APIReference/API_ListIdentities.html)
+ [ListIdentityPolicies](https://docs.aws.amazon.com/ses/latest/APIReference/API_ListIdentityPolicies.html)
+ [ListReceiptFilters](https://docs.aws.amazon.com/ses/latest/APIReference/API_ListReceiptFilters.html)
+ [ListReceiptRuleSets](https://docs.aws.amazon.com/ses/latest/APIReference/API_ListReceiptRuleSets.html)
+ [ListVerifiedEmailAddresses](https://docs.aws.amazon.com/ses/latest/APIReference/API_ListVerifiedEmailAddresses.html)
+ [PutIdentityPolicy](https://docs.aws.amazon.com/ses/latest/APIReference/API_PutIdentityPolicy.html)
+ [ReorderReceiptRuleSet](https://docs.aws.amazon.com/ses/latest/APIReference/API_ReorderReceiptRuleSet.html)
+ [SetActiveReceiptRuleSet](https://docs.aws.amazon.com/ses/latest/APIReference/API_SetActiveReceiptRuleSet.html)
+ [SetReceiptRulePosition](https://docs.aws.amazon.com/ses/latest/APIReference/API_SetReceiptRulePosition.html)
+ [SetIdentityDkimEnabled](https://docs.aws.amazon.com/ses/latest/APIReference/API_SetIdentityDkimEnabled.html)
+ [SetIdentityFeedbackForwardingEnabled](https://docs.aws.amazon.com/ses/latest/APIReference/API_SetIdentityFeedbackForwardingEnabled.html)
+ [SetIdentityHeadersInNotificationsEnabled](https://docs.aws.amazon.com/ses/latest/APIReference/API_SetIdentityHeadersInNotificationsEnabled.html)
+ [SetIdentityNotificationTopic](https://docs.aws.amazon.com/ses/latest/APIReference/API_SetIdentityNotificationTopic.html)
+ [UpdateReceiptRule](https://docs.aws.amazon.com/ses/latest/APIReference/API_UpdateReceiptRule.html)
+ [VerifyDomainDkim](https://docs.aws.amazon.com/ses/latest/APIReference/API_VerifyDomainDkim.html)
+ [VerifyDomainIdentity](https://docs.aws.amazon.com/ses/latest/APIReference/API_VerifyDomainIdentity.html)
+ [VerifyEmailAddress](https://docs.aws.amazon.com/ses/latest/APIReference/API_VerifyEmailAddress.html)
+ [VerifyEmailIdentity](https://docs.aws.amazon.com/ses/latest/APIReference/API_VerifyEmailIdentity.html)

**Note**  
Amazon SES delivers *management events* to CloudTrail\. Management events include actions that are related to creating and managing resources within your AWS account\. In Amazon SES, management events include actions such as creating and deleting identities or receipt rules\.  
Management events are different from *data events*\. Data events are events that are related to accessing and interacting with data within your AWS account\. In Amazon SES, data events include actions such as sending emails\.  
Because Amazon SES only delivers management events to CloudTrail, the following events **aren't** recorded in CloudTrail:  
SendEmail
SendRawEmail
SendTemplatedEmail
SendBulkTemplatedEmail
SendCustomVerificationEmail
You can use event publishing to record events related to email sending\. For more information, see [Monitor email sending using Amazon SES event publishing](monitor-using-event-publishing.md)\.

Every event or log entry contains information about who generated the request\. The identity information helps you determine the following: 
+ Whether the request was made with root or AWS Identity and Access Management \(IAM\) user credentials\.
+ Whether the request was made with temporary security credentials for a role or federated user\.
+ Whether the request was made by another AWS service\.

For more information, see the [CloudTrail userIdentity Element](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-event-reference-user-identity.html)\.

## Example: Amazon SES Log File Entries<a name="understanding-service-name-entries"></a>

 A trail is a configuration that enables delivery of events as log files to an Amazon S3 bucket that you specify\. CloudTrail log files contain one or more log entries\. An event represents a single request from any source and includes information about the requested action, the date and time of the action, request parameters, and so on\. CloudTrail log files aren't an ordered stack trace of the public API calls, so they don't appear in any specific order\.

The following example shows a CloudTrail log entry that demonstrates the `DeleteIdentity` and `VerifyEmailIdentity` actions\.

```
{
  "Records":[
    {
      "awsRegion":"us-west-2",
      "eventID":"0ffa308d-1467-4259-8be3-c749753be325",
      "eventName":"DeleteIdentity",
      "eventSource":"ses.amazonaws.com",
      "eventTime":"2018-02-02T21:34:50Z",
      "eventType":"AwsApiCall",
      "eventVersion":"1.02",
      "recipientAccountId":"111122223333",
      "requestID":"50b87bfe-ab23-11e4-9106-5b36376f9d12",
      "requestParameters":{
        "identity":"amazon.com"
      },
      "responseElements":null,
      "sourceIPAddress":"192.0.2.0",
      "userAgent":"aws-sdk-java/unknown-version",
      "userIdentity":{
        "accessKeyId":"AKIAIOSFODNN7EXAMPLE",
        "accountId":"111122223333",
        "arn":"arn:aws:iam::111122223333:root",
        "principalId":"111122223333",
        "type":"Root"
      }
    },
    {
      "awsRegion":"us-west-2",
      "eventID":"5613b0ff-d6c6-4526-9b53-a603a9231725",
      "eventName":"VerifyEmailIdentity",
      "eventSource":"ses.amazonaws.com",
      "eventTime":"2018-02-04T01:05:33Z",
      "eventType":"AwsApiCall",
      "eventVersion":"1.02",
      "recipientAccountId":"111122223333",
      "requestID":"eb2ff803-ac09-11e4-8ff5-a56a3119e253",
      "requestParameters":{
        "emailAddress":"sender@example.com"
      },
      "responseElements":null,
      "sourceIPAddress":"192.0.2.0",
      "userAgent":"aws-sdk-java/unknown-version",
      "userIdentity":{
        "accessKeyId":"AKIAIOSFODNN7EXAMPLE",
        "accountId":"111122223333",
        "arn":"arn:aws:iam::111122223333:root",
        "principalId":"111122223333",
        "type":"Root"
      }
    }
  ]
}
```