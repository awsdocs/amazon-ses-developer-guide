# Identity and access management in Amazon SES<a name="control-user-access"></a>

You can use AWS Identity and Access Management \(IAM\) with Amazon Simple Email Service \(Amazon SES\) to specify which SES API actions an IAM user, group, or role can perform\. \(In this topic we refer to these entities collectively as *user*\.\) You can also control which email addresses the user can use for the "From", recipient, and "Return\-Path" addresses of emails\.

For example, you can create an IAM policy that allows users in your organization to send email, but not perform administrative actions such as checking sending statistics\. As another example, you can write a policy that allows a user to send emails through SES from your account, but only if they use a specific "From" address\.

To use IAM, you define an IAM policy, which is a document that explicitly defines permissions, and attach the policy to a user\. To learn how to create IAM policies, see the [IAM User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/policies_overview.html)\. Other than applying the restrictions you set in your policy, there are no changes to how users interact with SES or in how SES carries out requests\.

**Note**  
If your account is in the SES sandbox, its restrictions well prevent the implementation of some of these polices \- see [Moving out of the sandbox](request-production-access.md)\.
You can also control access to SES by using sending authorization policies\. Whereas IAM policies constrain what individual IAM users can do, sending authorization policies constrain how individual verified identities can be used\. Further, only sending authorization policies can grant cross\-account access\. For more information about sending authorization, see [Using sending authorization with Amazon SES](sending-authorization.md)\.

If you are looking for information about how to generate SES SMTP credentials for an existing IAM user, see [Obtaining Amazon SES SMTP credentials](smtp-credentials.md)\.

## Creating IAM Policies for Access to SES<a name="iam-and-ses"></a>

This section explains how you can use IAM policies specifically with SES\. To learn how to create IAM policies in general, see the [IAM User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/AccessPolicyLanguage_ElementDescriptions.html)\.

There are three reasons you might use IAM with SES:
+ To restrict the email\-sending action\.
+ To restrict the "From", recipient, and "Return\-Path" addresses of the emails that the user sends\.
+ To control general aspects of API usage such as the time period during which a user is permitted to call the APIs that they are authorized to use\.

### Restricting the Action<a name="iam-and-ses-restrict-action"></a>

To control which SES actions a user can perform, you use the `Action` element of an IAM policy\. You can set the `Action` element to any SES API action by prefixing the API name with the lowercase string `ses:`\. For example, you can set the `Action` to `ses:SendEmail`, `ses:GetSendStatistics`, or `ses:*` \(for all actions\)\.

Then, depending on the `Action`, specify the `Resource` element as follows:

**If the `Action` element only permits access to email\-sending APIs \(that is, `ses:SendEmail` and/or `ses:SendRawEmail`\):**
+ To allow the user to send from any identity in your AWS account, set `Resource` to \*
+ To restrict the identities that a user is allowed to send from, set `Resource` to the ARNs of the identities that you are permitting the user to use\.

**If the `Action` element permits access to all APIs:**
+ If you don't want to restrict the identities that the user can send from, set `Resource` to \*
+ If you want to restrict the identities that a user is allowed to send from, you need to create two policies \(or two statements within one policy\):
  + One with `Action` set to an explicit list of the permitted non\-email\-sending APIs and `Resource` set to \*
  + One with `Action` set to one of the email\-sending APIs \(`ses:SendEmail` and/or `ses:SendRawEmail`\), and `Resource` set to the ARN\(s\) of the identities you are permitting the user to use\.

For a list of available SES actions, see the [Amazon Simple Email Service API Reference](https://docs.aws.amazon.com/ses/latest/APIReference/)\. If the IAM user will be using the SMTP interface, you must allow access to `ses:SendRawEmail` at a minimum\.

### Restricting Email Addresses<a name="iam-and-ses-restrict-addresses"></a>

If you want to restrict the user to specific email addresses, you can use a `Condition` block\. In the `Condition` block, you specify conditions by using condition keys as described in the [IAM User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/AccessPolicyLanguage_ElementDescriptions.html#Condition)\. By using condition keys, you can control the following email addresses:

**Note**  
These email address condition keys apply only to the APIs noted in the following table\.


****  

| Condition Key | Description | API | 
| --- | --- | --- | 
| `ses:Recipients` | Restricts the recipient addresses, which include the To:, "CC", and "BCC" addresses\. | `SendEmail`, `SendRawEmail` | 
| `ses:FromAddress` | Restricts the "From" address\. | `SendEmail`, `SendRawEmail`, `SendBounce` | 
| `ses:FromDisplayName` | Restricts the "From" address that is used as the display name\.  | `SendEmail`, `SendRawEmail` | 
| `ses:FeedbackAddress` | Restricts the "Return\-Path" address, which is the address where bounces and complaints can be sent to you by email feedback forwarding\. For information about email feedback forwarding, see [Receiving Amazon SES notifications through email](monitor-sending-activity-using-notifications-email.md)\. | `SendEmail`, `SendRawEmail` | 

### Restricting by SES API version<a name="iam-and-ses-restrict-api-version"></a>

By using the `ses:ApiVersion` key in conditions, you can restrict access to SES based on the version of the SES API\.

**Note**  
The SES SMTP interface uses SES API version 2 of `ses:SendRawEmail`\.

### Restricting General API Usage<a name="iam-and-ses-restrict-API-usage"></a>

By using AWS\-wide keys in conditions, you can restrict access to SES based on aspects such as the date and time that user is permitted access to APIs\. SES implements only the following AWS\-wide policy keys:
+ `aws:CurrentTime`
+ `aws:EpochTime`
+ `aws:SecureTransport`
+ `aws:SourceIp`
+ `aws:UserAgent`

For more information about these keys, see the [IAM User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/AccessPolicyLanguage_ElementDescriptions.html#Condition)\.

## Example IAM Policies for SES<a name="iam-and-ses-examples"></a>

This topic provides examples of policies that permit a user access to SES, but only under certain conditions\.

**Topics**
+ [Allowing Full Access to All SES Actions](#iam-and-ses-examples-full-access)
+ [Allowing Access to only SES API version 2](#iam-and-ses-examples-access-specific-ses-api-version)
+ [Allowing Access to Email\-Sending Actions Only](#iam-and-ses-examples-email-sending-actions)
+ [Restricting the Time Period of Sending](#iam-and-ses-examples-time-period)
+ [Restricting the Recipient Addresses](#iam-and-ses-examples-recipients)
+ [Restricting the "From" Address](#iam-and-ses-examples-from-address)
+ [Restricting the Display Name of the Email Sender](#iam-and-ses-examples-display-name)
+ [Restricting the Destination of Bounce and Complaint Feedback](#iam-and-ses-examples-feedback)

### Allowing Full Access to All SES Actions<a name="iam-and-ses-examples-full-access"></a>

The following policy allows a user to call any SES action\.

```
 1. {
 2.   "Version":"2012-10-17",
 3.   "Statement":[
 4.     {
 5.       "Effect":"Allow",
 6.       "Action":[
 7.         "ses:*"
 8.       ],
 9.       "Resource":"*"
10.     }
11.   ]
12. }
```

### Allowing Access to only SES API version 2<a name="iam-and-ses-examples-access-specific-ses-api-version"></a>

The following policy allows a user to call only the SES actions of API version 2\.

```
 1. {
 2.   		                 "Version":"2012-10-17",
 3.   		                 "Statement":[
 4.   		                     {
 5.   		                         "Effect":"Allow",
 6.   		                         "Action":[
 7.   		                         "ses:*"
 8.   		                         ],
 9.   		                         "Resource":"*",
10.   		                         "Condition": {
11.   		                             "StringEquals" : {
12.   		                             "ses:ApiVersion" : "2"
13.   		                             }
14.   		                         }
15.   		                     }
16.   		                 ]
17.   		             }
```

### Allowing Access to Email\-Sending Actions Only<a name="iam-and-ses-examples-email-sending-actions"></a>

The following policy permits a user to send email using SES, but does not permit the user to perform administrative actions such as accessing SES sending statistics\.

```
 1. {
 2.   "Version":"2012-10-17",
 3.   "Statement":[
 4.     {
 5.       "Effect":"Allow",
 6.       "Action":[
 7.         "ses:SendEmail",
 8.         "ses:SendRawEmail"
 9.       ],
10.       "Resource":"*"
11.     }
12.   ]
13. }
```

### Restricting the Time Period of Sending<a name="iam-and-ses-examples-time-period"></a>

The following policy permits a user to call SES email\-sending APIs only during the month of September 2018\.

```
 1. {
 2.   "Version":"2012-10-17",
 3.   "Statement":[
 4.     {
 5.       "Effect":"Allow",
 6.       "Action":[
 7.         "ses:SendEmail",
 8.         "ses:SendRawEmail"
 9.       ],
10.       "Resource":"*",
11.       "Condition":{
12.         "DateGreaterThan":{
13.           "aws:CurrentTime":"2018-08-31T12:00Z"
14.         },
15.         "DateLessThan":{
16.           "aws:CurrentTime":"2018-10-01T12:00Z"
17.         }
18.       }
19.     }
20.   ]
21. }
```

### Restricting the Recipient Addresses<a name="iam-and-ses-examples-recipients"></a>

The following policy permits a user to call the SES email\-sending APIs, but only to recipient addresses in domain *example\.com* \(`StringLike` *is case sensitive*\)\.

```
 1. {
 2.   "Version":"2012-10-17",
 3.   "Statement":[
 4.     {
 5.       "Effect":"Allow",
 6.       "Action":[
 7.         "ses:SendEmail",
 8.         "ses:SendRawEmail"
 9.       ],
10.       "Resource":"*",
11.       "Condition":{
12.         "ForAllValues:StringLike":{
13.           "ses:Recipients":[
14.             "*@example.com"
15.           ]
16.         }
17.       }
18.     }
19.   ]
20. }
```

### Restricting the "From" Address<a name="iam-and-ses-examples-from-address"></a>

The following policy permits a user to call the SES email\-sending APIs, but only if the "From" address is *marketing@example\.com*\. 

```
 1. {
 2.   "Version":"2012-10-17",
 3.   "Statement":[
 4.     {
 5.       "Effect":"Allow",
 6.       "Action":[
 7.         "ses:SendEmail",
 8.         "ses:SendRawEmail"
 9.       ],
10.       "Resource":"*",
11.       "Condition":{
12.         "StringEquals":{
13.           "ses:FromAddress":"marketing@example.com"
14.         }
15.       }
16.     }
17.   ]
18. }
```

The following policy permits a user to call the [SendBounce](https://docs.aws.amazon.com/ses/latest/APIReference/API_SendBounce.html) API, but only if the "From" address is *bounce@example\.com*\. 

```
 1. {
 2.   "Version":"2012-10-17",
 3.   "Statement":[
 4.     {
 5.       "Effect":"Allow",
 6.       "Action":[
 7.         "ses:SendBounce"
 8.       ],
 9.       "Resource":"*",
10.       "Condition":{
11.         "StringEquals":{
12.           "ses:FromAddress":"bounce@example.com"
13.         }
14.       }
15.     }
16.   ]
17. }
```

### Restricting the Display Name of the Email Sender<a name="iam-and-ses-examples-display-name"></a>

The following policy permits a user to call the SES email\-sending APIs, but only if the display name of the "From" address includes *Marketing* \(`StringLike` *is case sensitive*\)\. 

```
 1. {
 2.   "Version":"2012-10-17",
 3.   "Statement":[
 4.     {
 5.       "Effect":"Allow",
 6.       "Action":[
 7.         "ses:SendEmail",
 8.         "ses:SendRawEmail"
 9.       ],
10.       "Resource":"*",
11.       "Condition":{
12.         "StringLike":{
13.           "ses:FromDisplayName":"Marketing"
14.         }
15.       }
16.     }
17.   ]
18. }
```

### Restricting the Destination of Bounce and Complaint Feedback<a name="iam-and-ses-examples-feedback"></a>

The following policy permits a user to call the SES email\-sending APIs, but only if the "Return\-Path" of the email is set to *feedback@example\.com*\.

```
 1. {
 2.   "Version":"2012-10-17",
 3.   "Statement":[
 4.     {
 5.       "Effect":"Allow",
 6.       "Action":[
 7.         "ses:SendEmail",
 8.         "ses:SendRawEmail"
 9.       ],
10.       "Resource":"*",
11.       "Condition":{
12.         "StringEquals":{
13.           "ses:FeedbackAddress":"feedback@example.com"
14.         }
15.       }
16.     }
17.   ]
18. }
```