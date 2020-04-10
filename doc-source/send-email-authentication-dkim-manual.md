# Manual DKIM Signing in Amazon SES<a name="send-email-authentication-dkim-manual"></a>

As an alternative to using Easy DKIM, you can instead manually add DKIM signatures to your messages, and then send those messages using Amazon SES\. If you choose to manually sign your messages, you first have to create a DKIM signature\. After you create the message and the DKIM signature, you can use the [SendRawEmail](https://docs.aws.amazon.com/ses/latest/APIReference/API_SendRawEmail.html) API to send it\.

If you decide to manually sign your email, consider the following factors:
+ Every message that you send by using Amazon SES contains a DKIM header that references a signing domain of *amazonses\.com* \(that is, it contains the following string: `d=amazonses.com`\)\. If you manually sign your messages, your messages should include *two* DKIM headers: one for your domain, and one for *amazonses\.com*\.
+ Amazon SES doesn't validate DKIM signatures that you manually add to your messages\. If there are errors with the DKIM signature in a message, it might be rejected by email providers\.
+ When you sign your messages, you should use a bit length of at least 1024 bits\. 
+ Don't sign the following fields: Message\-ID, Date, Return\-Path, Bounces\-To\.
**Note**  
If you use an email client to send email using the Amazon SES SMTP interface, your client might automatically perform DKIM signing of your messages\. Some clients might sign some of these fields\. See the documentation for your email client to see which fields are signed by default\.