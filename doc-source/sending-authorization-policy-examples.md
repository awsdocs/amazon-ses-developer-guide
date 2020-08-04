# Amazon SES Sending Authorization Policy Examples<a name="sending-authorization-policy-examples"></a>

Sending authorization enables you to specify the fine\-grained conditions under which you allow delegate senders to send on your behalf\.

**Topics**
+ [Specifying the Delegate Sender](#sending-authorization-policy-example-sender)
+ [Restricting the "From" Address](#sending-authorization-policy-example-from)
+ [Restricting the Time at which the Delegate can Send Email](#sending-authorization-policy-example-time)
+ [Restricting the Email Sending Action](#sending-authorization-policy-example-action)
+ [Restricting the Display Name of the Email Sender](#sending-authorization-policy-example-display-name)
+ [Using Multiple Statements](#sending-authorization-policy-example-multiple-statements)

## Specifying the Delegate Sender<a name="sending-authorization-policy-example-sender"></a>

The *principal*, which is the entity to which you are granting permission, can be an AWS account, an AWS Identity and Access Management \(IAM\) user, or an AWS service\.

The following example shows a simple policy that allows AWS ID *123456789012* to send email from the verified identity *example\.com* \(which is owned by AWS account *888888888888*\)\. The `Condition` statement in this policy only allows the delegate \(that is, AWS ID *123456789012*\) to send email from the address *marketing\+\.\*@example\.com*, where *\.\** is any string that the sender wants to add after *marketing\+*\.

```
 1. {
 2.   "Id":"SampleAuthorizationPolicy",
 3.   "Version":"2012-10-17",
 4.   "Statement":[
 5.     {
 6.       "Sid":"AuthorizeMarketer",
 7.       "Effect":"Allow",
 8.       "Resource":"arn:aws:ses:us-east-1:888888888888:identity/example.com",
 9.       "Principal":{
10.         "AWS":[
11.           "123456789012"
12.         ]
13.       },
14.       "Action":[
15.         "SES:SendEmail",
16.         "SES:SendRawEmail"
17.       ],
18.       "Condition":{
19.         "StringLike":{
20.           "ses:FromAddress":"marketing+.*@example.com"
21.         }
22.       }
23.     }
24.   ]
25. }
```

The following example policy grants permission to two IAM users to send from identity *example\.com*\. IAM users are specified by their Amazon Resource Name \(ARN\)\.

```
 1. {
 2.   "Id":"ExampleAuthorizationPolicy",
 3.   "Version":"2012-10-17",
 4.   "Statement":[
 5.     {
 6.       "Sid":"AuthorizeIAMUser",
 7.       "Effect":"Allow",
 8.       "Resource":"arn:aws:ses:us-east-1:888888888888:identity/example.com",
 9.       "Principal":{
10.         "AWS":[
11.           "arn:aws:iam::111122223333:user/John",
12.           "arn:aws:iam::444455556666:user/Jane"
13.         ]
14.       },
15.       "Action":[
16.         "SES:SendEmail",
17.         "SES:SendRawEmail"
18.       ]
19.     }
20.   ]
21. }
```

The following example policy grants permission to Amazon Cognito to send from identity *example\.com*\.

```
 1. {
 2.   "Id":"ExampleAuthorizationPolicy",
 3.   "Version":"2012-10-17",
 4.   "Statement":[
 5.     {
 6.       "Sid":"AuthorizeService",
 7.       "Effect":"Allow",
 8.       "Resource":"arn:aws:ses:us-east-1:888888888888:identity/example.com",
 9.       "Principal":{
10.         "Service":[
11.           "cognito-idp.amazonaws.com"
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

The following example policy grants permission to all accounts within an AWS Organization to send from identity *example\.com*\. The AWS Organization is specified using the [PrincipalOrgID](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-principalorgid) global condition key.

```
 1. {
 2.   "Id":"ExampleAuthorizationPolicy",
 3.   "Version":"2012-10-17",
 4.   "Statement":[
 5.     {
 6.       "Sid":"AuthorizeOrg",
 7.       "Effect":"Allow",
 8.       "Resource":"arn:aws:ses:us-east-1:888888888888:identity/example.com",
 9.       "Principal":"*",
 10.      "Action":[
 11.        "SES:SendEmail",
 12.        "SES:SendRawEmail"
 13.      ],
 14.      "Condition":{
 15.        "StringEquals":{
 16.          "aws:PrincipalOrgID":"o-xxxxxxxxxxx"
 17.        }
 18.      }
 19.    }
 20.  ]
 21. }
```

## Restricting the "From" Address<a name="sending-authorization-policy-example-from"></a>

If you use a verified domain, you may want to create a policy that only allows the delegate sender to send from a specified email address\. To restrict the "From" address, you set a condition on the key called *ses:FromAddress*\. The following policy enables AWS account ID *123456789012* to send from the identity *example\.com*, but only from the email address *sender@example\.com*\.

```
 1. {
 2.   "Id":"ExamplePolicy",
 3.   "Version":"2012-10-17",
 4.   "Statement":[
 5.     {
 6.       "Sid":"AuthorizeFromAddress",
 7.       "Effect":"Allow",
 8.       "Resource":"arn:aws:ses:us-east-1:888888888888:identity/example.com",
 9.       "Principal":{
10.         "AWS":[
11.           "123456789012"
12.         ]
13.       },
14.       "Action":[
15.         "SES:SendEmail",
16.         "SES:SendRawEmail"
17.       ],
18.       "Condition":{
19.         "StringEquals":{
20.           "ses:FromAddress":"sender@example.com"
21.         }
22.       }
23.     }
24.   ]
25. }
```

## Restricting the Time at which the Delegate can Send Email<a name="sending-authorization-policy-example-time"></a>

You can also configure your sender authorization policy so that a delegate sender can only send email at a certain time of day, or within a certain date range\. For example, if you plan to send an email campaign during the month of September 2018, you can use the following policy to restrict the delegate's ability to send email to that month only\.

```
 1. {
 2.   "Id":"ExamplePolicy",
 3.   "Version":"2012-10-17",
 4.   "Statement":[
 5.     {
 6.       "Sid":"ControlTimePeriod",
 7.       "Effect":"Allow",
 8.       "Resource":"arn:aws:ses:us-east-1:888888888888:identity/example.com",
 9.       "Principal":{
10.         "AWS":[
11.           "123456789012"
12.         ]
13.       },
14.       "Action":[
15.         "SES:SendEmail",
16.         "SES:SendRawEmail"
17.       ],
18.       "Condition":{
19.         "DateGreaterThan":{
20.           "aws:CurrentTime":"2018-08-31T12:00Z"
21.         },
22.         "DateLessThan":{
23.           "aws:CurrentTime":"2018-10-01T12:00Z"
24.         }
25.       }
26.     }
27.   ]
28. }
```

## Restricting the Email Sending Action<a name="sending-authorization-policy-example-action"></a>

There are two actions that senders can use to send an email with Amazon SES: `SendEmail` and `SendRawEmail`, depending on how much control the sender wants over the format of the email\. Sending authorization policies enable you to restrict the delegate sender to one of those two actions\. However, many identity owners leave the details of the email sending calls up to the delegate sender by enabling both actions in their policies\.

**Note**  
If you want to enable the delegate sender to access Amazon SES through the SMTP interface, you must choose `SendRawEmail` at a minimum\.

If your use case is such that you want to restrict the action, you can do so by including only one of the actions in your sending authorization policy\. The following example shows you how to restrict the action to `SendRawEmail`\.

```
 1. {
 2.   "Id":"ExamplePolicy",
 3.   "Version":"2012-10-17",
 4.   "Statement":[
 5.     {
 6.       "Sid":"ControlAction",
 7.       "Effect":"Allow",
 8.       "Resource":"arn:aws:ses:us-east-1:888888888888:identity/example.com",
 9.       "Principal":{
10.         "AWS":[
11.           "123456789012"
12.         ]
13.       },
14.       "Action":[
15.         "SES:SendRawEmail"
16.       ]
17.     }
18.   ]
19. }
```

## Restricting the Display Name of the Email Sender<a name="sending-authorization-policy-example-display-name"></a>

Some email clients display the "friendly" name of the email sender \(if the email header provides it\), rather than the actual "From" address\. For example, the display name of "John Doe <johndoe@example\.com>" is John Doe\. For instance, you might send emails from *user@example\.com*, but you prefer that recipients see that the email is from *Marketing* rather than from *user@example\.com*\. The following policy enables AWS account ID 123456789012 to send from identity *example\.com*, but only if the display name of the "From" address includes *Marketing*\.

```
 1. {
 2.   "Id":"ExamplePolicy",
 3.   "Version":"2012-10-17",
 4.   "Statement":[
 5.     {
 6.       "Sid":"AuthorizeFromAddress",
 7.       "Effect":"Allow",
 8.       "Resource":"arn:aws:ses:us-east-1:888888888888:identity/example.com",
 9.       "Principal":{
10.         "AWS":[
11.           "123456789012"
12.         ]
13.       },
14.       "Action":[
15.         "SES:SendEmail",
16.         "SES:SendRawEmail"
17.       ],
18.       "Condition":{
19.         "StringLike":{
20.           "ses:FromDisplayName":"Marketing"
21.         }
22.       }
23.     }
24.   ]
25. }
```

## Using Multiple Statements<a name="sending-authorization-policy-example-multiple-statements"></a>

Your sending authorization policy can include multiple statements\. The following example policy has two statements\. The first statement authorizes two AWS accounts to send from *sender@example\.com* as long as the "From" address and the feedback address both use the domain *example\.com*\. The second statement authorizes an IAM user to send email from *sender@example\.com* as long as the recipient's email address is under the *example\.com* domain\.

```
 1. {
 2.   "Version":"2012-10-17",
 3.   "Statement":[
 4.     {
 5.       "Sid":"AuthorizeAWS",
 6.       "Effect":"Allow",
 7.       "Resource":"arn:aws:ses:us-east-1:999999999999:identity/sender@example.com",
 8.       "Principal":{
 9.         "AWS":[
10.           "111111111111",
11.           "222222222222"
12.         ]
13.       },
14.       "Action":[
15.         "SES:SendEmail",
16.         "SES:SendRawEmail"
17.       ],
18.       "Condition":{
19.         "StringLike":{
20.           "ses:FromAddress":"*@example.com",
21.           "ses:FeedbackAddress":"*@example.com"
22.         }
23.       }
24.     },
25.     {
26.       "Sid":"AuthorizeInternal",
27.       "Effect":"Allow",
28.       "Resource":"arn:aws:ses:us-east-1:999999999999:identity/sender@example.com",
29.       "Principal":{
30.         "AWS":"arn:aws:iam::333333333333:user/Jane"
31.       },
32.       "Action":[
33.         "SES:SendEmail",
34.         "SES:SendRawEmail"
35.       ],
36.       "Condition":{
37.         "ForAllValues:StringLike":{
38.           "ses:Recipients":"*@example.com"
39.         }
40.       }
41.     }
42.   ]
43. }
```
