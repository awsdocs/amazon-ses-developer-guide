# Email format and Amazon SES<a name="send-email-concepts-email-format"></a>

When a client makes a request to Amazon SES, Amazon SES constructs an email message compliant with the Internet Message Format specification \([RFC 5322](https://www.ietf.org/rfc/rfc5322.txt)\)\. An email consists of a *header*, a *body*, and an *envelope*, as described below\.
+ **Header—**Contains routing instructions and information about the message\. Examples are the sender's address, the recipient's address, the subject, and the date\. The header is analogous to the information at the top of a postal letter, though it can contain many other types of information, such as the format of the message\. 
+ **Body—**Contains the text of the message itself\.
+ **Envelope—**Contains the actual routing information that is communicated between the email client and the mail server during the SMTP session\. This email envelope information is analogous to the information on a postal envelope\. The routing information of the email envelope is usually the same as the routing information in the email header, but not always\. For example, when you send a blind carbon copy \(BCC\), the actual recipient address \(derived from the envelope\) is not the same as the "To" address that is displayed in the recipient's email client, which is derived from the header\.

The following is a simple example of an email\. The header is followed by a blank line and then the body of the email\. The envelope isn't shown because it is communicated between the client and the mail server during the SMTP session, rather than a part of the email itself\. 

```
 1. Received: from abc.smtp-out.amazonses.com (123.45.67.89) by in.example.com (87.65.43.210); Fri, 17 Dec 2010 14:26:22
 2. From: "Andrew" <andrew@example.com>;
 3. To: "Bob" <bob@example.com>
 4. Date: Fri, 17 Dec 2010 14:26:21 -0800
 5. Subject: Hello
 6. Message-ID: <61967230-7A45-4A9D-BEC9-87CBCF2211C9@example.com>
 7. Accept-Language: en-US
 8. Content-Language: en-US
 9. Content-Type: text/plain; charset="us-ascii"
10. Content-Transfer-Encoding: quoted-printable
11. MIME-Version: 1.0
12. 
13. Hello, I hope you are having a good day.
14. 
15. -Andrew
```

The following sections review email headers and bodies and identify the information that you need to provide when you use Amazon SES\.

## Email header<a name="send-email-concepts-email-format-header"></a>

There is one header per email message\. Each line of the header contains a field followed by a colon followed by a field body\. When you read an email in an email client, the email client typically displays the values of the following header fields:
+ **To—**The email addresses of the message's recipients\.
+ **CC—**The email addresses of the message's carbon copy recipients\.
+ **From—**The email address from which the email is sent\.
+ **Subject—**A summary of the message topic\.
+ **Date—**The time and date the email is sent\.

There are many additional header fields that provide routing information and describe the content of the message\. Email clients typically do not display these fields to the user\. For a full list of the header fields that Amazon SES accepts, see [Header fields](header-fields.md)\. When you use Amazon SES, you particularly need to understand the difference between "From," "Reply\-To," and "Return\-Path" header fields\. As noted previously, the "From" address is the email address of the message sender, whereas "Reply\-To" and "Return\-Path" are as follows:
+ **Reply\-To—**The email address to which replies will be sent\. By default, replies are sent to the original sender's email address\.
+ **Return\-Path—**The email address to which message bounces and complaints should be sent\. "Return\-Path" is sometimes called "envelope from," "envelope sender," or "MAIL FROM\."
**Note**  
When you use Amazon SES, we recommend that you always set the "Return\-Path" parameter so that you can be aware of bounces and take corrective action if they occur\.

To easily match a bounced message with its intended recipient, you can use Variable Envelope Return Path \(VERP\)\. With VERP, you set a different "Return\-Path" for each recipient, so that if the message bounces back, you automatically know which recipient it bounced from, rather than having to open the bounce message and parse it\.

## Email body<a name="send-email-concepts-email-format-body"></a>

The email body contains the text of the message\. The body can be sent in the following formats:
+ **HTML—**If the recipient's email client can interpret HTML, the body can include formatted text and hyperlinks
+ **Plain text—**If the recipient's email client is text\-based, the body must not contain any nonprintable characters\.
+ **Both HTML and plain text—**When you use both formats to send the same content in a single message, the recipient's email client decides which to display, based upon its capabilities\.

If you are sending an email message to a large number of recipients, then it makes sense to send it in both HTML and text\. Some recipients will have HTML\-enabled email clients, so that they can click embedded hyperlinks in the message\. Recipients using text\-based email clients will need you to include URLs that they can copy and open using a web browser\.

## Email information you need to provide to Amazon SES<a name="send-email-concepts-email-required-information"></a>

When you send an email with Amazon SES, the email information you need to provide depends on how you call Amazon SES\. You can provide a minimal amount of information and have Amazon SES take care of all of the formatting for you\. Or, if you want to do something more advanced like send an attachment, you can provide the raw message yourself\. The following sections review what you need to provide when you send an email by using the Amazon SES API, the Amazon SES SMTP interface, or the Amazon SES console\.

### Amazon SES API<a name="send-email-concepts-email-required-information-api"></a>

If you call the Amazon SES API directly, you call either the `SendEmail` or the `SendRawEmail` API\. The amount of information you need to provide depends on which API you call\.
+ The `SendEmail API` requires you to provide only a source address, destination address, message subject, and a message body\. You can optionally provide "Reply\-To" addresses\. When you call this API, Amazon SES automatically assembles a properly formatted multi\-part Multipurpose Internet Mail Extensions \(MIME\) email message optimized for display by email client software\. For more information, see [Sending formatted email using the Amazon SES API](send-email-formatted.md)\.
+ The `SendRawEmail` API provides you the flexibility to format and send your own raw email message by specifying headers, MIME parts, and content types\. `SendRawEmail` is typically used by advanced users\. You need to provide the body of the message and all header fields that are specified as required in the Internet Message Format specification \([RFC 5322](https://www.ietf.org/rfc/rfc5322.txt)\)\. For more information, see [Sending raw email using the Amazon SES API](send-email-raw.md)\.

If you use an AWS SDK to call the Amazon SES API, you provide the information listed above to the corresponding functions \(for example, `SendEmail` and `SendRawEmail` for Java\)\.

For more information about sending email using the Amazon SES API, see [Using the Amazon SES API to send email](send-email-api.md)\.

### Amazon SES SMTP interface<a name="send-email-concepts-email-required-information-smtp"></a>

When you access Amazon SES through the SMTP interface, your SMTP client application assembles the message, so the information you need to provide depends on the application you are using\. At a minimum, the SMTP exchange between a client and a server requires a source address, a destination address, and message data\. If you are using the SMTP interface and have feedback forwarding enabled, then your bounces, complaints, and delivery notifications are sent to the "MAIL FROM" address\. Any "Reply\-To" address that you specify is not used\.

For more information about sending email using the Amazon SES SMTP interface, see [Using the Amazon SES SMTP interface to send email](send-email-smtp.md)\.

### Amazon SES console<a name="send-email-concepts-email-required-information-console"></a>

When you send an email by using the Amazon SES console, the amount of information you need to provide depends on whether you choose to send a formatted or raw email\.
+ To send a formatted email, you need to provide a source address, a destination address, a message subject, and a message body\. Amazon SES automatically assembles a properly formatted multi\-part MIME email message optimized for display by email client software\. You can also specify a reply\-to and a return path field\.
+ To send a raw email, you provide the source address, a destination address, and the message content, which must contain the body of the message and all header fields that are specified as required in the Internet Message Format specification \([RFC 5322](https://www.ietf.org/rfc/rfc5322.txt)\)\.
