# Policy anatomy<a name="policy-anatomy"></a>

Policies adhere to a specific structure, contain elements, and must meet certain requirements\.

## Policy structure<a name="sending-authorization-policy-structure"></a>

Each sending authorization policy is a JSON document that is attached to an identity\. Each policy includes the following sections:
+ Policy\-wide information at the top of the document\.
+ One or more individual statements, each of which describes a set of permissions\.

The following example policy grants AWS account ID *123456789012* permission to send from the verified domain *example\.com*\.

```
 1. {
 2.   "Id":"ExampleAuthorizationPolicy",
 3.   "Version":"2012-10-17",
 4.   "Statement":[
 5.     {
 6.       "Sid":"AuthorizeAccount",
 7.       "Effect":"Allow",
 8.       "Resource":"arn:aws:ses:us-east-1:123456789012:identity/example.com",
 9.       "Principal":{
10.         "AWS":[
11.           "123456789012"
12.         ]
13.       },
14.       "Action":[
15.         "ses:SendEmail",
16.         "ses:SendTemplatedEmail",
17.         "ses:SendRawEmail",
18.         "ses:SendBulkTemplatedEmail"
19.       ]
20.     }
21.   ]
22. }
```

You can find more sending authorization policy examples at [Policy examples](sending-authorization-policy-examples.md)\.

## Policy elements<a name="sending-authorization-policy-elements"></a>

This section describes the elements contained in sending authorization policies\. First we describe policy\-wide elements, and then we describe elements that apply only to the statement in which they are included\. We follow with a discussion of how to add conditions to your statements\.

For specific information about the syntax of the elements, see [Grammar of the IAM Policy Language](https://docs.aws.amazon.com/IAM/latest/UserGuide/policies-grammar.html) in the *IAM User Guide*\.

### Policy\-wide information<a name="sending-authorization-policy-policy-wide"></a>

There are two policy\-wide elements: `Id` and `Version`\. The following table provides information about these elements\.


****  

|  Name  |  Description  |  Required  |  Valid values  | 
| --- | --- | --- | --- | 
|   `Id`   |  Uniquely identifies the policy\.  |  No  |  Any string  | 
|   `Version`   |  Specifies the policy access language version\.  |  No  |  Either "2012\-10\-17"\ or "2008\-10\-17"\. As a best practice, we recommend that you include this field with a value of "2012\-10\-17"\.  For more information, see [IAM JSON policy elements: Version](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_version.html)| 

### Statements specific to the policy<a name="sending-authorization-policy-statements"></a>

Sending authorization policies require at least one statement\. Each statement can include the elements described in the following table\.


****  

|  Name  |  Description  |  Required  |  Valid values  | 
| --- | --- | --- | --- | 
|   `Sid`   |  Uniquely identifies the statement\.  |  No  |  Any string\.  | 
|   `Effect`   |  Specifies the result that you want the policy statement to return at evaluation time\.  |  Yes  |  "Allow" or "Deny"\.  | 
|   `Resource`   |  Specifies the identity to which the policy applies\. This is the email address or domain that the identity owner is authorizing the delegate sender to use\.  |  Yes  |  The Amazon Resource Name \(ARN\) of the email identity\.  | 
|   `Principal`   |  Specifies the AWS account, IAM user, or AWS service that receives the permission in the statement\.  |  Yes  |  A valid AWS account ID, IAM user ARN, or AWS service\. AWS account IDs and IAM user ARNs are specified using `"AWS"` \(for example, `"AWS": ["123456789012"]` or `"AWS": ["arn:aws:iam::123456789012:root"]`\)\. AWS service names are specified using `"Service"` \(for example, `"Service": ["cognito-idp.amazonaws.com"]`\)\.  For examples of the format of IAM user ARNs, see the [AWS General Reference](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html#arn-syntax-iam.html)\.  | 
|   `Action`   |  Specifies the email sending action that the statement applies to\.  |  Yes  |  "ses:SendEmail", "ses:SendRawEmail", "ses:SendTemplatedEmail", "ses:SendBulkTemplatedEmail" You can specify one or more of these operations\. You can also specify "ses:Send\*" to encompass all of these operations\. If the delegate sender plans to send email by using the SMTP interface, you have to specify "ses:SendRawEmail", or use "ses:Send\*"\.  | 
|   `Condition`   |  Specifies any restrictions or details about the permission\.  |  No  |  See the information about conditions following this table\.  | 

### Conditions<a name="sending-authorization-policy-conditions"></a>

A *condition* is any restriction about the permission in the statement\. The part of the statement that specifies the conditions can be the most detailed of all the parts\. A *key* is the specific characteristic that's the basis for access restriction, such as the date and time of the request\.

You use both conditions and keys together to express the restriction\. For example, if you want to restrict the delegate sender from making requests to Amazon SES on your behalf after July 30, 2019, you use the condition called `DateLessThan`\. You use the key called `aws:CurrentTime` and set it to the value `2019-07-30T00:00:00Z`\. 

You can use any of the AWS\-wide keys listed at [Available Keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/AccessPolicyLanguage_ElementDescriptions.html#AvailableKeys) in the *IAM User Guide*, or you can use one of the following keys specific to Amazon SES:


****  

|  Condition key  |  Description  | 
| --- | --- | 
|   `ses:Recipients`   |  Restricts the recipient addresses, which include the To:, "CC", and "BCC" addresses\.  | 
|   `ses:FromAddress`   |  Restricts the "From" address\.  | 
|   `ses:FromDisplayName`   |  Restricts the contents of the string that is used as the "From" display name \(sometimes called "friendly from"\)\. For example, the display name of "John Doe <johndoe@example\.com>" is John Doe\.  | 
|   `ses:FeedbackAddress`   |  Restricts the "Return Path" address, which is the address where bounce and complaints can be sent to you by email feedback forwarding\. For information about email feedback forwarding, see [Receiving Amazon SES notifications through email](monitor-sending-activity-using-notifications-email.md)\.  | 

You can use the `StringEquals` and `StringLike` conditions with Amazon SES keys\. These conditions are for case\-sensitive string matching\. For `StringLike`, the values can include a multi\-character match wildcard \(\*\) or a single\-character match wildcard \(?\) anywhere in the string\. For example, the following condition specifies that the delegate sender can only send from a "From" address that starts with *invoicing* and ends with *@example\.com*:

```
1. "Condition": {
2.     "StringLike": {
3.       "ses:FromAddress": "invoicing*@example.com"
4.     }
5. }
```

You can also use the `StringNotLike` condition to prevent delegate senders from sending email from certain email addresses\. For example, you can disallow sending from *admin@example\.com*, and also similar addresses such as *"admin"@example\.com*, *admin\+1@example\.com*, or *sender@admin\.example\.com*, by including the following condition in your policy statement:

```
1. "Condition": {
2.     "StringNotLike": {
3.       "ses:FromAddress": "*admin*example.com"
4.     }
5.  }
```

For more information about how to specify conditions, see [IAM JSON Policy Elements: Condition](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html) in the *IAM User Guide*\.

## Policy requirements<a name="sending-authorization-policy-restrictions"></a>

Policies must meet all of the following requirements:
+ Each policy has to include at least one statement\.
+ Each policy has to include at least one valid principal\.
+ Each policy has to specify one resource, and that resource has to be the ARN of the identity that the policy is attached to\.
+ Identity owners can associate up to 20 policies with each unique identity\.
+ Policies can't exceed 4 kilobytes \(KB\) in size\.
+ Policy names can't exceed 64 characters\. Additionally, they can only include alphanumeric characters, dashes, and underscores\.
