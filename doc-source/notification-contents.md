# Amazon SNS Notification Contents for Amazon SES<a name="notification-contents"></a>

Bounce, complaint, and delivery notifications are published to [Amazon Simple Notification Service \(Amazon SNS\)](https://aws.amazon.com/sns) topics in JavaScript Object Notation \(JSON\) format\. The top\-level JSON object contains a `notificationType` string, a `mail` object, and either a `bounce` object, a `complaint` object, or a `delivery` object\.

See the following sections for descriptions of the different types of objects:
+ [Top\-level JSON object](#top-level-json-object)
+ [`mail` object](#mail-object)
+ [`bounce` object](#bounce-object)
+ [`complaint` object](#complaint-object)
+ [`delivery` object](#delivery-object)

The following are some important notes about the contents of Amazon SNS notifications for Amazon SES:
+ For a given notification type, you might receive one Amazon SNS notification for multiple recipients, or you might receive a single Amazon SNS notification per recipient\. Your code should be able to parse the Amazon SNS notification and handle both cases; Amazon SES does not make ordering or batching guarantees for notifications sent through Amazon SNS\. However, different Amazon SNS notification types \(for example, bounces and complaints\) are never combined into a single notification\.
+ You might receive multiple types of Amazon SNS notifications for one recipient\. For example, the receiving mail server might accept the email \(triggering a delivery notification\), but after processing the email, the receiving mail server might determine that the email actually results in a bounce \(triggering a bounce notification\)\. However, these are always separate notifications because they are different notification types\.
+ Amazon SES reserves the right to add additional fields to the notifications\. As such, applications that parse these notifications must be flexible enough to handle unknown fields\.
+ Amazon SES overwrites the headers of the message when it sends the email\. You can retrieve the headers of the original message from the `headers` and `commonHeaders` fields of the `mail` object\.

## Top\-Level JSON Object<a name="top-level-json-object"></a>

The top\-level JSON object in an Amazon SES notification contains the following fields\.


| Field Name | Description | 
| --- | --- | 
|  `notificationType`  |  A string that holds the type of notification represented by the JSON object\. Possible values are `Bounce`, `Complaint`, or `Delivery`\.  | 
|  `mail`  |  A JSON object that contains information about the original mail to which the notification pertains\. For more information, see [Mail Object](#mail-object)\.  | 
|  `bounce`  |  This field is present only if the `notificationType` is `Bounce` and contains a JSON object that holds information about the bounce\. For more information, see [Bounce Object](#bounce-object)\.  | 
|  `complaint`  |  This field is present only if the `notificationType` is `Complaint` and contains a JSON object that holds information about the complaint\. For more information, see [Complaint Object](#complaint-object)\.  | 
|  `delivery`  |  This field is present only if the `notificationType` is `Delivery` and contains a JSON object that holds information about the delivery\. For more information, see [Delivery Object](#delivery-object)\.  | 

## Mail Object<a name="mail-object"></a>

Each bounce, complaint, or delivery notification contains information about the original email in the `mail` object\. The JSON object that contains information about a `mail` object has the following fields\.


| Field Name | Description | 
| --- | --- | 
|  `timestamp`  |  The time at which the original message was sent \(in ISO8601 format\)\.  | 
|  `messageId`  |  A unique ID that Amazon SES assigned to the message\. Amazon SES returned this value to you when you sent the message\.  This message ID was assigned by Amazon SES\. You can find the message ID of the original email in the `headers` and `commonHeaders` fields of the `mail` object\.   | 
|  `source`  |  The email address from which the original message was sent \(the envelope MAIL FROM address\)\.  | 
|  `sourceArn`  |  The Amazon Resource Name \(ARN\) of the identity that was used to send the email\. In the case of sending authorization, the `sourceArn` is the ARN of the identity that the identity owner authorized the delegate sender to use to send the email\. For more information about sending authorization, see [Using Sending Authorization](sending-authorization.md)\.  | 
|  `sourceIp`  |  The originating public IP address of the client that performed the email sending request to Amazon SES\.  | 
|  `sendingAccountId`  |  The AWS account ID of the account that was used to send the email\. In the case of sending authorization, the `sendingAccountId` is the delegate sender's account ID\.  | 
|  `destination`  |  A list of email addresses that were recipients of the original mail\.  | 
|  `headersTruncated`  |  \(Only present if the [notification settings](configure-sns-notifications.md) include the original email headers\.\) A string that specifies whether the headers are truncated in the notification, which occurs if the headers are larger than 10 KB\. Possible values are `true` and `false`\.  | 
|  `headers`  |  \(Only present if the notification settings include the original email headers\.\) A list of the email's original headers\. Each header in the list has a `name` field and a `value` field\.  Any message ID within the `headers` field is from the original message that you passed to Amazon SES\. The message ID that Amazon SES subsequently assigned to the message is in the `messageId` field of the `mail` object\.   | 
|  `commonHeaders`  |  \(Only present if the notification settings include the original email headers\.\) A list of the email's original, commonly used headers\. Each header in the list has a `name` field and a `value` field\.  Any message ID within the `commonHeaders` field is from the original message that you passed to Amazon SES\. The message ID that Amazon SES subsequently assigned to the message is in the `messageId` field of the `mail` object\.   | 

The following is an example of a `mail` object that includes the original email headers\. When this notification type is not configured to include the original email headers, the `mail` object does not include the `headersTruncated`, `headers`, and `commonHeaders` fields\. 

```
{
   "timestamp":"2016-01-27T14:05:45 +0000",
   "messageId":"000001378603177f-7a5433e7-8edb-42ae-af10-f0181f34d6ee-000000",
   "source":"john@example.com",
   "sourceArn": "arn:aws:ses:us-west-2:888888888888:identity/example.com",
   "sourceIp": "127.0.3.0",
   "sendingAccountId":"123456789012",
   "destination":[
      "jane@example.com"
   ],
   "headersTruncated":false,
   "headers":[ 
      { 
         "name":"From",
         "value":"\"John Doe\" <john@example.com>"
      },
      { 
         "name":"To",
         "value":"\"Jane Doe\" <jane@example.com>"
      },
      { 
         "name":"Message-ID",
         "value":"custom-message-ID"
      },
      { 
         "name":"Subject",
         "value":"Hello"
      },
      { 
         "name":"Content-Type",
         "value":"text/plain; charset=\"UTF-8\""
      },
      { 
         "name":"Content-Transfer-Encoding",
         "value":"base64"
      },
      { 
         "name":"Date",
         "value":"Wed, 27 Jan 2016 14:05:45 +0000"
      }
   ],
   "commonHeaders":{ 
      "from":[ 
         "John Doe <john@example.com>"
      ],
      "date":"Wed, 27 Jan 2016 14:05:45 +0000",
      "to":[ 
         "Jane Doe <jane@example.com>"
      ],
      "messageId":" custom-message-ID",
      "subject":"Hello"
   }
}
```

## Bounce Object<a name="bounce-object"></a>

The JSON object that contains information about bounces contains the following fields\.


| Field Name | Description | 
| --- | --- | 
|  `bounceType`  |  The type of bounce, as determined by Amazon SES\. For more information, see [Bounce Types](#bounce-types)\.  | 
|  `bounceSubType`  |  The subtype of the bounce, as determined by Amazon SES\. For more information, see [Bounce Types](#bounce-types)\.  | 
|  `bouncedRecipients`  |  A list that contains information about the recipients of the original mail that bounced\. For more information, see [Bounced Recipients](#bounced-recipients)\.  | 
|  `timestamp`  |  The date and time at which the bounce was sent \(in ISO8601 format\)\. Note that this is the time at which the notification was sent by the ISP, and not the time at which it was received by Amazon SES\.  | 
|  `feedbackId`  |  A unique ID for the bounce\.  | 

If Amazon SES was able to contact the remote Message Transfer Authority \(MTA\), the following field is also present\.


| Field Name | Description | 
| --- | --- | 
|  `remoteMtaIp`  |  The IP address of the MTA to which Amazon SES attempted to deliver the email\.  | 

If a delivery status notification \(DSN\) was attached to the bounce, the following field is also present\.


| Field Name | Description | 
| --- | --- | 
|  `reportingMTA`  |  The value of the `Reporting-MTA` field from the DSN\. This is the value of the MTA that attempted to perform the delivery, relay, or gateway operation described in the DSN\.  | 

The following is an example of a `bounce` object\.

```
{
   "bounceType":"Permanent",
   "bounceSubType": "General",
   "bouncedRecipients":[
      {
         "status":"5.0.0",
         "action":"failed",
         "diagnosticCode":"smtp; 550 user unknown",
         "emailAddress":"recipient1@example.com"
      },
      {
         "status":"4.0.0",
         "action":"delayed",
         "emailAddress":"recipient2@example.com"
      }
   ],
   "reportingMTA": "example.com",
   "timestamp":"2012-05-25T14:59:38.605Z",
   "feedbackId":"000001378603176d-5a4b5ad9-6f30-4198-a8c3-b1eb0c270a1d-000000",
   "remoteMtaIp":"127.0.2.0"
}
```

### Bounced Recipients<a name="bounced-recipients"></a>

A bounce notification may pertain to a single recipient or to multiple recipients\. The `bouncedRecipients` field holds a list of objects—one per recipient to whom the bounce notification pertains—and always contains the following field\.


| Field Name | Description | 
| --- | --- | 
|  `emailAddress`  |  The email address of the recipient\. If a DSN is available, this is the value of the `Final-Recipient` field from the DSN\.  | 

Optionally, if a DSN is attached to the bounce, the following fields may also be present\.


| Field Name | Description | 
| --- | --- | 
|  `action`  |  The value of the `Action` field from the DSN\. This indicates the action performed by the Reporting\-MTA as a result of its attempt to deliver the message to this recipient\.  | 
|  `status`  |  The value of the `Status` field from the DSN\. This is the per\-recipient transport\-independent status code that indicates the delivery status of the message\.  | 
|  `diagnosticCode`  |  The status code issued by the reporting MTA\. This is the value of the `Diagnostic-Code` field from the DSN\. This field may be absent in the DSN \(and therefore also absent in the JSON\)\.  | 

The following is an example of an object that might be in the `bouncedRecipients` list\.

```
{
    "emailAddress": "recipient@example.com",
    "action": "failed",
    "status": "5.0.0",
    "diagnosticCode": "X-Postfix; unknown user"
}
```

### Bounce Types<a name="bounce-types"></a>

The bounce object contains a bounce type of `Undetermined`, `Permanent`, or `Transient`\. The `Permanent` and `Transient` bounce types can also contain one of several bounce subtypes\. 

When you receive a bounce notification with a bounce type of `Transient`, you might be able to send email to that recipient in the future if the issue that caused the message to bounce is resolved\. 

When you receive a bounce notification with a bounce type of `Permanent`, it's unlikely that you'll be able to send email to that recipient in the future\. For this reason, you should immediately remove the recipient whose address produced the bounce from your mailing lists\. 

**Note**  
When a *soft bounce* \(a bounce related to a temporary issue, such as the recipient's inbox being full\) occurs, Amazon SES attempts to redeliver the email for a certain period of time\. At the end of that period of time, if Amazon SES still can't deliver the email, it stops trying\.  
Amazon SES provides notifications for hard bounces, as well as for soft bounces that it stopped trying to deliver\.


| bounceType | bounceSubType | Description | 
| --- | --- | --- | 
|  `Undetermined`  |  `Undetermined`  |  The recipient's email provider sent a bounce message\. The bounce message didn't contain enough information for Amazon SES to determine the reason for the bounce\. The bounce email, which was sent to the address in the Return\-Path header of the email that resulted in the bounce, might contain additional information about the issue that caused the email to bounce\.  | 
|  `Permanent`  |  `General`  |  The recipient's email provider sent a hard bounce message, but didn't specify the reason for the hard bounce\.   When you receive this type of bounce notification, you should immediately remove the recipient's email address from your mailing list\. Sending messages to addresses that produce hard bounces can have a negative impact on your reputation as a sender\. If you continue sending email to addresses that produce hard bounces, we might pause your ability to send additional email\.   | 
|  `Permanent`  |  `NoEmail`  |  The intended recipient's email provider sent a bounce message indicating that the email address doesn't exist\.  When you receive this type of bounce notification, you should immediately remove the recipient's email address from your mailing list\. Sending messages to addresses that don't exist can have a negative impact on your reputation as a sender\. If you continue sending email to addresses that don't exist, we might pause your ability to send additional email\.   | 
|  `Permanent`  |  `Suppressed`  |  The recipient's email address is on the Amazon SES suppression list because it has a recent history of producing hard bounces\. For information about removing an address from the Amazon SES suppression list, see [Removing an Email Address from the Amazon SES Suppression List](remove-from-suppression-list.md)\.  | 
|  `Transient`  |  `General`  |  The recipient's email provider sent a general bounce message\. You might be able to send a message to the same recipient in the future if the issue that caused the message to bounce is resolved\.  If you send an email to a recipient who has an active automatic response rule \(such as an "out of the office" message\), you might receive this type of notification\. Even though the response has a notification type of `Bounce`, Amazon SES doesn't count automatic responses when it calculates the bounce rate for your account\.   | 
|  `Transient`  |  `MailboxFull`  |  The recipient's email provider sent a bounce message because the recipient's inbox was full\. You might be able to send to the same recipient in the future when the mailbox is no longer full\.  | 
|  `Transient`  |  `MessageTooLarge`  |  The recipient's email provider sent a bounce message because message you sent was too large\. You might be able to send a message to the same recipient if you reduce the size of the message\.  | 
|  `Transient`  |  `ContentRejected`  |  The recipient's email provider sent a bounce message because the message you sent contains content that the provider doesn't allow\. You might be able to send a message to the same recipient if you change the content of the message\.  | 
|  `Transient`  |  `AttachmentRejected`  |  The recipient's email provider sent a bounce message because the message contained an unacceptable attachment\. For example, some email providers may reject messages with attachments of a certain file type, or messages with very large attachments\. You might be able to send a message to the same recipient if you remove or change the content of the attachment\.  | 

## Complaint Object<a name="complaint-object"></a>

The JSON object that contains information about complaints has the following fields\.


| Field Name | Description | 
| --- | --- | 
|  `complainedRecipients`  |  A list that contains information about recipients that may have been responsible for the complaint\. For more information, see [Complained Recipients](#complained-recipients)\.  | 
|  `timestamp`  |  The date and time when the ISP sent the complaint notification, in ISO 8601 format\. The date and time in this field might not be the same as the date and time when Amazon SES received the notification\.   | 
|  `feedbackId`  |  A unique ID associated with the complaint\.  | 

Further, if a feedback report is attached to the complaint, the following fields may be present\.


| Field Name | Description | 
| --- | --- | 
|  `userAgent`  |  The value of the `User-Agent` field from the feedback report\. This indicates the name and version of the system that generated the report\.  | 
|  `complaintFeedbackType`  |  The value of the `Feedback-Type` field from the feedback report received from the ISP\. This contains the type of feedback\.  | 
|  `arrivalDate`  |  The value of the `Arrival-Date` or `Received-Date` field from the feedback report \(in ISO8601 format\)\. This field may be absent in the report \(and therefore also absent in the JSON\)\.  | 

The following is an example of a `complaint` object\.

```
{
   "userAgent":"AnyCompany Feedback Loop (V0.01)",
   "complainedRecipients":[
      {
         "emailAddress":"recipient1@example.com"
      }
   ],
   "complaintFeedbackType":"abuse",
   "arrivalDate":"2009-12-03T04:24:21.000-05:00",
   "timestamp":"2012-05-25T14:59:38.623Z",
   "feedbackId":"000001378603177f-18c07c78-fa81-4a58-9dd1-fedc3cb8f49a-000000"
}
```

### Complained Recipients<a name="complained-recipients"></a>

The `complainedRecipients` field contains a list of recipients that may have submitted the complaint\. You should use this information to determine which recipient submitted the complaint, and then immediately remove that recipient your mailing lists\. 

**Important**  
Most ISPs remove the email address of the recipient who submitted the complaint from their complaint notification\. For this reason, this list contains information about recipients who might have sent the complaint, based on the recipients of the original message and the ISP from which we received the complaint\. Amazon SES performs a lookup against the original message to determine this recipient list\.

JSON objects in this list contain the following field\.


| Field Name | Description | 
| --- | --- | 
|  `emailAddress`  |  The email address of the recipient\.  | 

The following is an example of a Complained Recipient object\.

```
{ "emailAddress": "recipient1@example.com" }
```

**Note**  
Because of this behavior, you can be more certain that you know which email address complained about your message if you limit your sending to one message per recipient \(rather than sending one message with 30 different email addresses in the bcc line\)\.

#### Complaint Types<a name="complaint-types"></a>

You may see the following complaint types in the `complaintFeedbackType` field as assigned by the reporting ISP, according to the [Internet Assigned Numbers Authority website](http://www.iana.org/assignments/marf-parameters/marf-parameters.xml#marf-parameters-2):
+ `abuse`—Indicates unsolicited email or some other kind of email abuse\.
+ `auth-failure`—Email authentication failure report\.
+ `fraud`—Indicates some kind of fraud or phishing activity\.
+ `not-spam`—Indicates that the entity providing the report does not consider the message to be spam\. This may be used to correct a message that was incorrectly tagged or categorized as spam\.
+ `other`—Indicates any other feedback that does not fit into other registered types\.
+ `virus`—Reports that a virus is found in the originating message\. 

## Delivery Object<a name="delivery-object"></a>

The JSON object that contains information about deliveries always has the following fields\.


| Field Name | Description | 
| --- | --- | 
|  `timestamp`  |  The time Amazon SES delivered the email to the recipient's mail server \(in ISO8601 format\)\.  | 
|  `processingTimeMillis`  |  The time in milliseconds between when Amazon SES accepted the request from the sender to passing the message to the recipient's mail server\.  | 
|  `recipients`  |  A list of the intended recipients of the email to which the delivery notification applies\.  | 
|  `smtpResponse`  |  The SMTP response message of the remote ISP that accepted the email from Amazon SES\. This message varies by email, by receiving mail server, and by receiving ISP\.  | 
|  `reportingMTA`  |  The host name of the Amazon SES mail server that sent the mail\.  | 
|  `remoteMtaIp`  |  The IP address of the MTA to which Amazon SES delivered the email\.  | 

The following is an example of a `delivery` object\.

```
{
   "timestamp":"2014-05-28T22:41:01.184Z",
   "processingTimeMillis":546,
   "recipients":["success@simulator.amazonses.com"],
   "smtpResponse":"250 ok:  Message 64111812 accepted",
   "reportingMTA":"a8-70.smtp-out.amazonses.com",
   "remoteMtaIp":"127.0.2.0"
}
```