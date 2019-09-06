# Amazon SES Sending Authorization Policies<a name="sending-authorization-policies"></a>

To enable another AWS account, Identity Access and Management \(IAM\) user, or AWS service to send email through Amazon SES on your behalf, you create a *sending authorization policy*, which is a JSON document that you attach to an identity that you own\. The policy explicitly lists who you're allowing to send for that identity, and under which conditions\. All senders except you and the entities that you explicitly grant permissions to in the policies are denied\. An identity can have no policy, one policy, or multiple policies attached to it\. You can also have one policy with multiple statements to achieve the effect of multiple policies\.

Policies can be simple, or can be configured to provide fine\-grained control\. For example, if you owned *example\.com*, you could write a simple policy to grant AWS ID 123456789012 permission to send from that domain\. A more detailed policy could specify that AWS ID 123456789012 can send email only from *user@example\.com* and only within a specified date range\.

Amazon SES sending authorization policies apply to email sending APIs \(`SendEmail`, `SendRawEmail`, `SendTemplatedEmail`, and `SendBulkTemplatedEmail`\) only\. They don't enable a user to access your AWS account in any other way\.

## Policy Structure<a name="sending-authorization-policy-structure"></a>

Each sending authorization policy is a JSON document that is attached to an identity\. Each policy includes the following sections:
+ Policy\-wide information at the top of the document\.
+ One or more individual statements, each of which describes a set of permissions\.

Each statement includes the core information about a single permission\. If a policy includes multiple statements, Amazon SES applies a logical OR across the statements at evaluation time\. Similarly, if an identity has multiple policies attached to it, Amazon SES applies a logical OR across the policies at evaluation time\.

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
15.         "SES:SendEmail",
16.         "SES:SendRawEmail"
17.       ]
18.     }
19.   ]
20. }
```

You can find more sending authorization policy examples at [Sending Authorization Policy Examples](sending-authorization-policy-examples.md)\.

## Policy Elements<a name="sending-authorization-policy-elements"></a>

This section describes the elements contained in sending authorization policies\. First we describe policy\-wide elements, and then we describe elements that apply only to the statement in which they are included\. We follow with a discussion of how to add conditions to your statements\.

For specific information about the syntax of the elements, see [Grammar of the IAM Policy Language](https://docs.aws.amazon.com/IAM/latest/UserGuide/policies-grammar.html) in the *IAM User Guide*\.

### Policy\-Wide Information<a name="sending-authorization-policy-policy-wide"></a>

There are two policy\-wide elements: `Id` and `Version`\. The following table provides information about these elements\.


****  

|  Name  |  Description  |  Required  |  Valid Values  | 
| --- | --- | --- | --- | 
|   `Id`   |  Uniquely identifies the policy\.  |  No\.  |  Any string  | 
|   `Version`   |  Specifies the policy access language version\.  |  No, but as a best practice, we recommend that you include this field with a value of "2012\-10\-17"\.  |  Any string  | 

### Statements Specific to the Policy<a name="sending-authorization-policy-statements"></a>

Sending authorization policies require at least one statement\. Each statement can include the elements described in the following table\.


****  

|  Name  |  Description  |  Required  |  Valid Values  | 
| --- | --- | --- | --- | 
|   `Sid`   |  Uniquely identifies the statement\.  |  No\.  |  Any string\.  | 
|   `Effect`   |  Specifies the result that you want the policy statement to return at evaluation time\.  |  No, although a statement without an effect is useless\.  |  "Allow" or "Deny"\.  | 
|   `Resource`   |  Specifies the identity to which the policy applies\. This is the email address or domain that the identity owner is authorizing the delegate sender to use\.  |  Yes\.  |  An identity's ARN, as specified in the Amazon SES console\.  | 
|   `Principal`   |  Specifies the AWS account, IAM user, or AWS service that receives the permission in the statement\.  |  Yes\.  |  A valid AWS account ID, IAM user ARN, or AWS service\. AWS account IDs and IAM user ARNs are specified using `"AWS"` \(for example, `"AWS": ["123456789012"]` or `"AWS": ["arn:aws:iam::123456789012:root"]`\)\. AWS service names are specified using `"Service"` \(for example, `"Service": ["cognito-idp.amazonaws.com"]`\)\.  For examples of the format of IAM user ARNs, see the [AWS General Reference](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html#arn-syntax-iam.html)\.  | 
|   `Action`   |  Specifies the email sending action that the statement applies to\.  |  Yes\.  |  "ses:SendEmail", "ses:SendRawEmail" \(one or both\)\.  If you use the custom policy editor, you can also set the action to "ses:\*" to encompass both APIs\. If your sender will access Amazon SES through the SMTP interface, you must at least specify "ses:SendRawEmail", or use "ses:\*"\.  | 
|   `Condition`   |  Specifies any restrictions or details about the permission\.  |  No\.  |  See the information about conditions following this table\.  | 

### Conditions<a name="sending-authorization-policy-conditions"></a>

A *condition* is any restriction about the permission in the statement\. The part of the statement that specifies the conditions can be the most detailed of all the parts\. A *key* is the specific characteristic that is the basis for access restriction, such as the date and time of the request\.

You use both conditions and keys together to express the restriction\. For example, if you want to restrict the delegate sender from making requests to Amazon SES on your behalf after July 30, 2019, you use the condition called `DateLessThan`\. You use the key called `aws:CurrentTime` and set it to the value `2019-07-30T00:00:00Z`\. 

You can use any of the AWS\-wide keys listed at [Available Keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/AccessPolicyLanguage_ElementDescriptions.html#AvailableKeys) in the *IAM User Guide*, or you can use one of the following keys specific to Amazon SES:


****  

|  Condition Key  |  Description  | 
| --- | --- | 
|   `ses:Recipients`   |  Restricts the recipient addresses, which include the To:, "CC", and "BCC" addresses\.  | 
|   `ses:FromAddress`   |  Restricts the "From" address\.  | 
|   `ses:FromDisplayName`   |  Restricts the contents of the string that is used as the "From" display name \(sometimes called "friendly from"\)\. For example, the display name of "John Doe <johndoe@example\.com>" is John Doe\.  | 
|   `ses:FeedbackAddress`   |  Restricts the "Return Path" address, which is the address where bounce and complaints can be sent to you by email feedback forwarding\. For information about email feedback forwarding, see [Amazon SES Notifications Through Email](notifications-via-email.md)\.  | 

It is common to use the `StringEquals` and `StringLike` conditions with the Amazon SES keys\. These conditions are for case\-sensitive string matching\. For `StringLike`, the values can include a multi\-character match wildcard \(\*\) or a single\-character match wildcard \(?\) anywhere in the string\. For example, the following condition specifies that the delegate sender can only send from a "From" address that starts with *invoicing* and ends with *example\.com*:

```
1. "Condition": {
2.     "StringLike": {
3.       "ses:FromAddress": "invoicing+.*@example.com"
4.     }
5. }
```

**Note**  
When you want to disallow access to an email address, you can use wildcards to ensure that you're completely preventing access to all variations of that address\. For example, to disallow sending from *admin@example\.com*, you can prevent access to alternatives such as *"admin"@example\.com* and *admin\+1@example\.com* by specifying the following condition:  

```
1. "Condition": {
2.     "StringNotLike": {
3.       "ses:FromAddress": "*admin*.example.com"
4.     }
5.  }
```

For more information about how to specify conditions, see [Condition](https://docs.aws.amazon.com/IAM/latest/UserGuide/AccessPolicyLanguage_ElementDescriptions.html#Condition) in the *IAM User Guide*\.

## Policy Requirements<a name="sending-authorization-policy-restrictions"></a>

Each policy must adhere to the following requirements:
+ Each policy has to include at least one statement\.
+ Each policy has to include at least one valid principal\.
+ Each policy has to specify one resource, and that resource has to be the ARN of the identity that the policy is attached to\.
+ Identity owners can associate up to 20 policies with each unique identity\.
+ Policies can't exceed 4 kilobytes \(KB\) in size\.
+ Policy names can't exceed 64 characters\. Additionally, they can only include alphanumeric characters, dashes, and underscores\.