# Bounce Action<a name="receiving-email-action-bounce"></a>

The **Bounce** action rejects the email by returning a bounce response to the sender and, optionally, notifies you through Amazon SNS\. This action has the following options\.
+ **SMTP Reply Code—**The SMTP reply code, as defined by [RFC 5321](https://tools.ietf.org/html/rfc5321)\.
+ **SMTP Status Code—**The SMTP enhanced status code, as defined by [RFC 3463](https://tools.ietf.org/html/rfc3463)\.
+ **Message—**Human\-readable text to include in the bounce email\.
+ **Reply Sender—**The email address of the sender of the bounced email\. This is the address from which the bounce email will be sent\. It must be verified with Amazon SES\.
+ **SNS Topic—**The name or ARN of the Amazon SNS topic to optionally notify when a bounce email is sent\. An example of an Amazon SNS topic ARN is *arn:aws:sns:us\-west\-2:123456789012:MyTopic*\. You can also create an Amazon SNS topic when you set up your action by choosing **Create SNS Topic**\. For more information about Amazon SNS topics, see the [Amazon Simple Notification Service Developer Guide](https://docs.aws.amazon.com/sns/latest/dg/CreateTopic.html)\. 
**Note**  
The Amazon SNS topic you choose must be in the same AWS region as the Amazon SES endpoint you use to receive email\. 

You can type in your own values for these fields, or you can choose a template that fills in the SMTP Reply Code, SMTP Status Code, and Message fields with values based on the bounce reason\. The following templates are available:
+ **Mailbox Does Not Exist—** SMTP Reply Code = 550, SMTP Status Code = 5\.1\.1
+ **Message Too Large—** SMTP Reply Code = 552, SMTP Status Code = 5\.3\.4
+ **Message Full—** SMTP Reply Code = 552, SMTP Status Code = 5\.2\.2
+ **Message Content Rejected—** SMTP Reply Code = 500, SMTP Status Code = 5\.6\.1
+ **Unknown Failure—** SMTP Reply Code = 554, SMTP Status Code = 5\.0\.0
+ **Temporary Failure—** SMTP Reply Code = 450, SMTP Status Code = 4\.0\.0

For additional bounce codes that you might use by typing custom values in the fields, see [RFC 3463](https://tools.ietf.org/html/rfc3463)\.