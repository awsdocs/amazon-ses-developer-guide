# Contents of notifications for Amazon SES email receiving<a name="receiving-email-notifications-contents"></a>

All notifications for email receiving are published to Amazon Simple Notification Service \(Amazon SNS\) topics in JavaScript Object Notation \(JSON\) format\.

## Top\-level JSON object<a name="receiving-email-notifications-contents-top-level-json-object"></a>

The top\-level JSON object contains the following fields\.


****  

| Field Name | Description | 
| --- | --- | 
|  `notificationType`  |  The notification type\. For this type of notification, the value is always `Received`\.  | 
|  [`receipt`](#receiving-email-notifications-contents-receipt-object)  |  Object that contains information about the email delivery\.   | 
|  [`mail`](#receiving-email-notifications-contents-mail-object)  |  Object that contains information about the email associated with the notification\.   | 
|  `content`  |  String that contains the raw, unmodified email, which is typically in Multipurpose Internet Mail Extensions \(MIME\) format\. For more information about MIME format, see [RFC 2045](https://tools.ietf.org/html/rfc2045)\.  This field is present only if the notification was triggered by an SNS action\. Notifications triggered by all other actions do not contain this field\.   | 

## receipt object<a name="receiving-email-notifications-contents-receipt-object"></a>

The `receipt` object has the following fields\.


****  

| Field Name | Description | 
| --- | --- | 
|  [`action`](#receiving-email-notifications-contents-action-object)  |  Object that encapsulates information about the action that was executed\. For a list of possible values, see [action object](#receiving-email-notifications-contents-action-object)\.  | 
|  [`dkimVerdict`](#receiving-email-notifications-contents-dkimverdict-object)  |  Object that indicates whether the DomainKeys Identified Mail \(DKIM\) check passed\. For a list of possible values, see [dkimVerdict object](#receiving-email-notifications-contents-dkimverdict-object)\.  | 
| dmarcPolicy | Indicates the Domain\-based Message Authentication, Reporting & Conformance \(DMARC\) settings for the sending domain\. This field only appears if the message fails DMARC authentication\. Possible values for this field are:[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/receiving-email-notifications-contents.html) | 
| [`dmarcVerdict`](#receiving-email-notifications-contents-dmarcverdict-object) | Object that indicates whether the Domain\-based Message Authentication, Reporting & Conformance \(DMARC\) check passed\. For a list of possible values, see [dmarcVerdict object](#receiving-email-notifications-contents-dmarcverdict-object)\. | 
|  `processingTimeMillis`  |  String that specifies the period, in milliseconds, from the time Amazon SES received the message to the time it triggered the action\.  | 
|  `recipients`  |  A list of recipients \(specifically, the envelope RCPT TO addresses\) that were matched by the active [receipt rule](receiving-email-receipt-rules.md)\. The addresses listed here may differ from those listed by the `destination` field in the [mail object](#receiving-email-notifications-contents-mail-object)\.  | 
|  [`spamVerdict`](#receiving-email-notifications-contents-spamverdict-object)  |  Object that indicates whether the message is spam\. For a list of possible values, see [spamVerdict object](#receiving-email-notifications-contents-spamverdict-object)\.  | 
|  [`spfVerdict`](#receiving-email-notifications-contents-spfverdict-object)  |  Object that indicates whether the Sender Policy Framework \(SPF\) check passed\. For a list of possible values, see [spfVerdict object](#receiving-email-notifications-contents-spfverdict-object)\.  | 
|  `timestamp`  |  String that specifies the date and time at which the action was triggered, in [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) format\.  | 
|  [virusVerdict](#receiving-email-notifications-contents-virusverdict-object)  |  Object that indicates whether the message contains a virus\. For a list of possible values, see [virusVerdict object](#receiving-email-notifications-contents-virusverdict-object)\.  | 

### action object<a name="receiving-email-notifications-contents-action-object"></a>

The `action` object has the following fields\.


****  

| Field Name | Description | 
| --- | --- | 
|  `type`  |  String that indicates the type of action that was executed\. Possible values are `S3`, `SNS`, `Bounce`, `Lambda`, `Stop`, and `WorkMail`\.  | 
|  `topicArn`  |  String that contains the Amazon Resource Name \(ARN\) of the Amazon SNS topic to which the notification was published\.  | 
|  `bucketName`  |  String that contains the name of the Amazon S3 bucket to which the message was published\. Present only for the S3 action type\.  | 
|  `objectKey`  |  String that contains a name that uniquely identifies the email in the Amazon S3 bucket\. This is the same as the `messageId` in the [mail object](#receiving-email-notifications-contents-mail-object)\. Present only for the S3 action type\.  | 
|  `smtpReplyCode`  |  String that contains the SMTP reply code, as defined by [RFC 5321](https://tools.ietf.org/html/rfc5321)\. Present only for the bounce action type\.  | 
|  `statusCode`  |  String that contains the SMTP enhanced status code, as defined by [RFC 3463](https://tools.ietf.org/html/rfc3463)\. Present only for the bounce action type\.  | 
|  `message`  |  String that contains the human\-readable text to include in the bounce message\. Present only for the bounce action type\.  | 
|  `sender`  |  String that contains the email address of the sender of the email that bounced\. This is the address from which the bounce message was sent\. Present only for the bounce action type\.  | 
|  `functionArn`  |  String that contains the ARN of the Lambda function that was triggered\. Present only for the Lambda action type\.  | 
|  `invocationType`  |  String that contains the invocation type of the Lambda function\. Possible values are `RequestResponse` and `Event`\. Present only for the Lambda action type\.  | 
|  `organizationArn`  |  String that contains the ARN of the Amazon WorkMail organization\. Present only for the WorkMail action type\.  | 

### dkimVerdict object<a name="receiving-email-notifications-contents-dkimverdict-object"></a>

The `dkimVerdict` object has the following fields\.


****  

| Field Name | Description | 
| --- | --- | 
|  `status`  |  String that contains the DKIM verdict\. Possible values are: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/receiving-email-notifications-contents.html)  | 

### dmarcVerdict object<a name="receiving-email-notifications-contents-dmarcverdict-object"></a>

The `dmarcVerdict` object has the following fields\.


****  

| Field Name | Description | 
| --- | --- | 
|  `status`  |  String that contains the DMARC verdict\. Possible values are: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/receiving-email-notifications-contents.html)  | 

### spamVerdict object<a name="receiving-email-notifications-contents-spamverdict-object"></a>

The `spamVerdict` object has the following fields\.


****  

| Field Name | Description | 
| --- | --- | 
|  `status`  |  String that contains the result of spam scanning\. Possible values are: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/receiving-email-notifications-contents.html)  | 

### spfVerdict object<a name="receiving-email-notifications-contents-spfverdict-object"></a>

The `spfVerdict` object has the following fields\.


****  

| Field Name | Description | 
| --- | --- | 
|  `status`  |  String that contains the SPF verdict\. Possible values are: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/receiving-email-notifications-contents.html)  | 

### virusVerdict object<a name="receiving-email-notifications-contents-virusverdict-object"></a>

The `virusVerdict` object has the following fields\.


****  

| Field Name | Description | 
| --- | --- | 
|  `status`  |  String that contains the result of virus scanning\. Possible values are: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/receiving-email-notifications-contents.html)  | 

## mail object<a name="receiving-email-notifications-contents-mail-object"></a>

The `mail` object has the following fields\.


****  

| Field Name | Description | 
| --- | --- | 
|   `destination`  |  A complete list of all recipient addresses \(including To: and CC: recipients\) from the MIME headers of the incoming email\.  | 
|  `messageId`  |  String that contains the unique ID assigned to the email by Amazon SES\. If the email was delivered to Amazon S3, the message ID is also the Amazon S3 object key that was used to write the message to your Amazon S3 bucket\.  | 
|  `source`  |  String that contains the email address \(specifically, the envelope MAIL FROM address\) that the email was sent from\.  | 
|  `timestamp`  |  String that contains the time at which the email was received, in ISO8601 format\.  | 
|  `headers`  |  A list of Amazon SES headers and your custom headers\. Each header in the list has a `name` field and a `value` field\.  | 
|  [`commonHeaders`](#receiving-email-notifications-contents-mail-object-commonHeaders)  |  A list of headers common to all emails\. Each header in the list is composed of a name and a value\.  | 
|  `headersTruncated`  |  Boolean that specifies whether the headers were truncated in the notification, which will happen if the headers are larger than 10 KB\. Possible values are true and false\.  | 

### commonHeaders object<a name="receiving-email-notifications-contents-mail-object-commonHeaders"></a>

The `commonHeaders` object can have the fields shown in the following table\. The fields present in this object vary depending on which fields were present in the incoming email\.


****  

| Field Name | Description | 
| --- | --- | 
|  `messageId`  | The ID of the original message\. | 
|  `date`  |  The date and time when Amazon SES received the message\.  | 
|  `to`  | The values in the To header of the email\. | 
|  `cc`  | The values in the CC header of the email\. | 
|  `bcc`  | The values in the BCC header of the email\. | 
|  `from`  | The values in the From header of the email\. | 
|  `sender`  | The values in the Sender header of the email\. | 
|  `returnPath`  | The value in the Return\-Path header of the email\. | 
|  `reply-to`  | The values in the Reply\-To header of the email\. | 
|  `subject`  | The value of the Subject header for the email\. | 
