# Sending email using AWS SDKs<a name="examples-send-using-sdk"></a>

The AWS SDKs contain built\-in methods for interacting with Amazon SES and several other AWS services\. If you plan to use Amazon SES along with other AWS services, we recommend that you use an SDK\. To learn more about the AWS SDKs, see [Tools for Amazon Web Services](https://aws.amazon.com/tools/?nc1=h_ls#sdk)

In this section, you will find code examples in several programming languages that demonstrate the process of sending email through Amazon SES using the AWS SDKs\.

------
#### [ C\# ]

The following code example is a complete solution for sending email through Amazon SES using the [AWS SDK for \.NET](https://aws.amazon.com/sdk-for-net/)\. This code example assumes that you have installed the AWS SDK for \.NET, and that you've created a shared credentials file\. For more information about creating a shared credentials file, see [Create a shared credentials file](create-shared-credentials-file.md)\.

**Important**  
You use a shared credentials file to pass your AWS access key ID and secret access key\. As an alternative to using a shared credentials file, you can specify your AWS access key ID and secret access key in the SDK Store\. For more information, see [Configuring AWS credentials](https://docs.aws.amazon.com/sdk-for-net/v3/developer-guide/net-dg-config-creds.html) in the AWS SDK for \.NET Developer Guide\. This example doesn't function unless you specify your credentials using one of these methods\.

```
 1. using Amazon;
 2. using System;
 3. using System.Collections.Generic;
 4. using Amazon.SimpleEmail;
 5. using Amazon.SimpleEmail.Model;
 6. 
 7. namespace AmazonSESSample 
 8. {
 9.     class Program
10.     {
11.         // Replace sender@example.com with your "From" address.
12.         // This address must be verified with Amazon SES.
13.         static readonly string senderAddress = "sender@example.com";
14. 
15.         // Replace recipient@example.com with a "To" address. If your account
16.         // is still in the sandbox, this address must be verified.
17.         static readonly string receiverAddress = "recipient@example.com";
18. 
19.         // The configuration set to use for this email. If you do not want to use a
20.         // configuration set, comment out the following property and the
21.         // ConfigurationSetName = configSet argument below. 
22.         static readonly string configSet = "ConfigSet";
23. 
24.         // The subject line for the email.
25.         static readonly string subject = "Amazon SES test (AWS SDK for .NET)";
26. 
27.         // The email body for recipients with non-HTML email clients.
28.         static readonly string textBody = "Amazon SES Test (.NET)\r\n" 
29.                                         + "This email was sent through Amazon SES "
30.                                         + "using the AWS SDK for .NET.";
31.         
32.         // The HTML body of the email.
33.         static readonly string htmlBody = @"<html>
34. <head></head>
35. <body>
36.   <h1>Amazon SES Test (AWS SDK for .NET)</h1>
37.   <p>This email was sent with
38.     <a href='https://aws.amazon.com/ses/'>Amazon SES</a> using the
39.     <a href='https://aws.amazon.com/sdk-for-net/'>
40.       AWS SDK for .NET</a>.</p>
41. </body>
42. </html>";
43. 
44.         static void Main(string[] args)
45.         {
46.             // Replace USWest2 with the AWS Region you're using for Amazon SES.
47.             // Acceptable values are EUWest1, USEast1, and USWest2.
48.             using (var client = new AmazonSimpleEmailServiceClient(RegionEndpoint.USWest2))
49.             {
50.                 var sendRequest = new SendEmailRequest
51.                 {
52.                     Source = senderAddress,
53.                     Destination = new Destination
54.                     {
55.                         ToAddresses =
56.                         new List<string> { receiverAddress }
57.                     },
58.                     Message = new Message
59.                     {
60.                         Subject = new Content(subject),
61.                         Body = new Body
62.                         {
63.                             Html = new Content
64.                             {
65.                                 Charset = "UTF-8",
66.                                 Data = htmlBody
67.                             },
68.                             Text = new Content
69.                             {
70.                                 Charset = "UTF-8",
71.                                 Data = textBody
72.                             }
73.                         }
74.                     },
75.                     // If you are not using a configuration set, comment
76.                     // or remove the following line 
77.                     ConfigurationSetName = configSet
78.                 };
79.                 try
80.                 {
81.                     Console.WriteLine("Sending email using Amazon SES...");
82.                     var response = client.SendEmail(sendRequest);
83.                     Console.WriteLine("The email was sent successfully.");
84.                 }
85.                 catch (Exception ex)
86.                 {
87.                     Console.WriteLine("The email was not sent.");
88.                     Console.WriteLine("Error message: " + ex.Message);
89. 
90.                 }
91.             }
92. 
93.             Console.Write("Press any key to continue...");
94.             Console.ReadKey();
95.         }
96.     }
97. }
```

------
#### [ Go ]

The following code example is a complete solution for sending email through Amazon SES using the [AWS SDK for Go](https://aws.amazon.com/sdk-for-go/)\. This code example assumes that you have installed the AWS SDK for Go, and that you have created a shared credentials file\. For more information about creating a shared credentials file, see [Create a shared credentials file](create-shared-credentials-file.md)\.

**Important**  
You use a shared credentials file to pass your AWS access key ID and secret access key\. As an alternative to using a shared credentials file, you can specify your AWS access key ID and secret access key by setting two environment variables \(`AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`, respectively\)\. This example doesn't function unless you specify your credentials using one of these methods\.

```
  1. package main
  2. 
  3. import (
  4.     "fmt"
  5.     
  6.     //go get -u github.com/aws/aws-sdk-go
  7.     "github.com/aws/aws-sdk-go/aws"
  8.     "github.com/aws/aws-sdk-go/aws/session"
  9.     "github.com/aws/aws-sdk-go/service/ses"
 10.     "github.com/aws/aws-sdk-go/aws/awserr"
 11. )
 12. 
 13. const (
 14.     // Replace sender@example.com with your "From" address. 
 15.     // This address must be verified with Amazon SES.
 16.     Sender = "sender@example.com"
 17.     
 18.     // Replace recipient@example.com with a "To" address. If your account 
 19.     // is still in the sandbox, this address must be verified.
 20.     Recipient = "recipient@example.com"
 21. 
 22.     // Specify a configuration set. If you do not want to use a configuration
 23.     // set, comment out the following constant and the 
 24.     // ConfigurationSetName: aws.String(ConfigurationSet) argument below
 25.     ConfigurationSet = "ConfigSet"
 26.     
 27.     // Replace us-west-2 with the AWS Region you're using for Amazon SES.
 28.     AwsRegion = "us-west-2"
 29.     
 30.     // The subject line for the email.
 31.     Subject = "Amazon SES Test (AWS SDK for Go)"
 32.     
 33.     // The HTML body for the email.
 34.     HtmlBody =  "<h1>Amazon SES Test Email (AWS SDK for Go)</h1><p>This email was sent with " +
 35.                 "<a href='https://aws.amazon.com/ses/'>Amazon SES</a> using the " +
 36.                 "<a href='https://aws.amazon.com/sdk-for-go/'>AWS SDK for Go</a>.</p>"
 37.     
 38.     //The email body for recipients with non-HTML email clients.
 39.     TextBody = "This email was sent with Amazon SES using the AWS SDK for Go."
 40.     
 41.     // The character encoding for the email.
 42.     CharSet = "UTF-8"
 43. )
 44. 
 45. func main() {
 46.     
 47.     // Create a new session and specify an AWS Region.
 48.     sess, err := session.NewSession(&aws.Config{
 49.         Region:aws.String(AwsRegion)},
 50.     )
 51.     
 52.     // Create an SES client in the session.
 53.     svc := ses.New(sess)
 54.     
 55.     // Assemble the email.
 56.     input := &ses.SendEmailInput{
 57.         Destination: &ses.Destination{
 58.             CcAddresses: []*string{
 59.             },
 60.             ToAddresses: []*string{
 61.                 aws.String(Recipient),
 62.             },
 63.         },
 64.         Message: &ses.Message{
 65.             Body: &ses.Body{
 66.                 Html: &ses.Content{
 67.                     Charset: aws.String(CharSet),
 68.                     Data:    aws.String(HtmlBody),
 69.                 },
 70.                 Text: &ses.Content{
 71.                     Charset: aws.String(CharSet),
 72.                     Data:    aws.String(TextBody),
 73.                 },
 74.             },
 75.             Subject: &ses.Content{
 76.                 Charset: aws.String(CharSet),
 77.                 Data:    aws.String(Subject),
 78.             },
 79.         },
 80.         Source: aws.String(Sender),
 81.             // Comment or remove the following line if you are not using a configuration set
 82.             ConfigurationSetName: aws.String(ConfigurationSet),
 83.     }
 84. 
 85.     // Attempt to send the email.
 86.     result, err := svc.SendEmail(input)
 87.     
 88.     // Display error messages if they occur.
 89.     if err != nil {
 90.         if aerr, ok := err.(awserr.Error); ok {
 91.             switch aerr.Code() {
 92.             case ses.ErrCodeMessageRejected:
 93.                 fmt.Println(ses.ErrCodeMessageRejected, aerr.Error())
 94.             case ses.ErrCodeMailFromDomainNotVerifiedException:
 95.                 fmt.Println(ses.ErrCodeMailFromDomainNotVerifiedException, aerr.Error())
 96.             case ses.ErrCodeConfigurationSetDoesNotExistException:
 97.                 fmt.Println(ses.ErrCodeConfigurationSetDoesNotExistException, aerr.Error())
 98.             default:
 99.                 fmt.Println(aerr.Error())
100.             }
101.         } else {
102.             // Print the error, cast err to awserr.Error to get the Code and
103.             // Message from an error.
104.             fmt.Println(err.Error())
105.         }
106.         return
107.     }
108.     
109.     fmt.Println("Email Sent!")
110.     fmt.Println(result)
111. }
```

------
#### [ Java SDK v1 ]

The following code example is a complete solution for sending email through Amazon SES using the [AWS SDK for Java](https://aws.amazon.com/sdk-for-java/)\. This code example assumes that you have installed the AWS SDK for Java, and that you have created a shared credentials file\. For more information about creating a shared credentials file, see [Create a shared credentials file](create-shared-credentials-file.md)\.

```
 1. package com.amazonaws.samples;
 2. 
 3. import java.io.IOException;
 4. 
 5. import com.amazonaws.regions.Regions;
 6. import com.amazonaws.services.simpleemail.AmazonSimpleEmailService;
 7. import com.amazonaws.services.simpleemail.AmazonSimpleEmailServiceClientBuilder;
 8. import com.amazonaws.services.simpleemail.model.Body;
 9. import com.amazonaws.services.simpleemail.model.Content;
10. import com.amazonaws.services.simpleemail.model.Destination;
11. import com.amazonaws.services.simpleemail.model.Message;
12. import com.amazonaws.services.simpleemail.model.SendEmailRequest; 
13. 
14. public class AmazonSESSample {
15. 
16.   // Replace sender@example.com with your "From" address.
17.   // This address must be verified with Amazon SES.
18.   static final String FROM = "sender@example.com";
19. 
20.   // Replace recipient@example.com with a "To" address. If your account
21.   // is still in the sandbox, this address must be verified.
22.   static final String TO = "recipient@example.com";
23. 
24.   // The configuration set to use for this email. If you do not want to use a
25.   // configuration set, comment the following variable and the 
26.   // .withConfigurationSetName(CONFIGSET); argument below.
27.   static final String CONFIGSET = "ConfigSet";
28. 
29.   // The subject line for the email.
30.   static final String SUBJECT = "Amazon SES test (AWS SDK for Java)";
31.   
32.   // The HTML body for the email.
33.   static final String HTMLBODY = "<h1>Amazon SES test (AWS SDK for Java)</h1>"
34.       + "<p>This email was sent with <a href='https://aws.amazon.com/ses/'>"
35.       + "Amazon SES</a> using the <a href='https://aws.amazon.com/sdk-for-java/'>" 
36.       + "AWS SDK for Java</a>";
37. 
38.   // The email body for recipients with non-HTML email clients.
39.   static final String TEXTBODY = "This email was sent through Amazon SES "
40.       + "using the AWS SDK for Java.";
41. 
42.   public static void main(String[] args) throws IOException {
43. 
44.     try {
45.       AmazonSimpleEmailService client = 
46.           AmazonSimpleEmailServiceClientBuilder.standard()
47.           // Replace US_WEST_2 with the AWS Region you're using for
48.           // Amazon SES.
49.             .withRegion(Regions.US_WEST_2).build();
50.       SendEmailRequest request = new SendEmailRequest()
51.           .withDestination(
52.               new Destination().withToAddresses(TO))
53.           .withMessage(new Message()
54.               .withBody(new Body()
55.                   .withHtml(new Content()
56.                       .withCharset("UTF-8").withData(HTMLBODY))
57.                   .withText(new Content()
58.                       .withCharset("UTF-8").withData(TEXTBODY)))
59.               .withSubject(new Content()
60.                   .withCharset("UTF-8").withData(SUBJECT)))
61.           .withSource(FROM)
62.           // Comment or remove the next line if you are not using a
63.           // configuration set
64.           .withConfigurationSetName(CONFIGSET);
65.       client.sendEmail(request);
66.       System.out.println("Email sent!");
67.     } catch (Exception ex) {
68.       System.out.println("The email was not sent. Error message: " 
69.           + ex.getMessage());
70.     }
71.   }
72. }
```

------
#### [ Java SDK v2 ]

The following code example is a complete solution for sending email through Amazon SES using the [AWS SDK for Java 2\.x](https://aws.amazon.com/sdk-for-java/) and the [JavaMail API](https://github.com/javaee/javamail/releases)\. This code example assumes that you have installed the SDK for Java 2\.x, and that you have created a shared credentials file\. For more information about creating a shared credentials file, see [Create a shared credentials file](create-shared-credentials-file.md)\.

```
package com.example.ses;

import software.amazon.awssdk.regions.Region;
import software.amazon.awssdk.services.ses.SesClient;
import javax.mail.Message;
import javax.mail.MessagingException;
import javax.mail.Session;
import javax.mail.internet.AddressException;
import javax.mail.internet.InternetAddress;
import javax.mail.internet.MimeMessage;
import javax.mail.internet.MimeMultipart;
import javax.mail.internet.MimeBodyPart;
import java.io.ByteArrayOutputStream;
import java.io.IOException;
import java.nio.ByteBuffer;
import java.util.Properties;
import software.amazon.awssdk.core.SdkBytes;
import software.amazon.awssdk.services.ses.model.SendRawEmailRequest;
import software.amazon.awssdk.services.ses.model.RawMessage;
import software.amazon.awssdk.services.ses.model.SesException;

public class SendMessage {

    public static void main(String[] args) throws IOException {

        final String USAGE = "\n" +
                "Usage:\n" +
                "    SendMessage <sender> <recipient> <subject> \n\n" +
                "Where:\n" +
                "    sender - an email address that represents the sender. \n"+
                "    recipient -  an email address that represents the recipient. \n"+
                "    subject - the  subject line. \n" ;

         if (args.length != 3) {
            System.out.println(USAGE);
            System.exit(1);
          }

        String sender = args[0];
        String recipient = args[1];
        String subject = args[2];

        Region region = Region.US_WEST_2;
        SesClient client = SesClient.builder()
                .region(region)
                .build();

        // The email body for non-HTML email clients
        String bodyText = "Hello,\r\n" + "See the list of customers. ";

        // The HTML body of the email
        String bodyHTML = "<html>" + "<head></head>" + "<body>" + "<h1>Hello!</h1>"
                + "<p> See the list of customers.</p>" + "</body>" + "</html>";

    try {
         send(client, sender, recipient, subject, bodyText, bodyHTML);
         client.close();
         System.out.println("Done");

    } catch (IOException | MessagingException e) {
        e.getStackTrace();
    }
  }

    public static void send(SesClient client,
                            String sender,
                            String recipient,
                            String subject,
                            String bodyText,
                            String bodyHTML
                            ) throws AddressException, MessagingException, IOException {

        Session session = Session.getDefaultInstance(new Properties());
        MimeMessage message = new MimeMessage(session);

        // Add subject, from and to lines
        message.setSubject(subject, "UTF-8");
        message.setFrom(new InternetAddress(sender));
        message.setRecipients(Message.RecipientType.TO, InternetAddress.parse(recipient));

        // Create a multipart/alternative child container
        MimeMultipart msgBody = new MimeMultipart("alternative");

        // Create a wrapper for the HTML and text parts
        MimeBodyPart wrap = new MimeBodyPart();

        // Define the text part
        MimeBodyPart textPart = new MimeBodyPart();
        textPart.setContent(bodyText, "text/plain; charset=UTF-8");

        // Define the HTML part
        MimeBodyPart htmlPart = new MimeBodyPart();
        htmlPart.setContent(bodyHTML, "text/html; charset=UTF-8");

        // Add the text and HTML parts to the child container
        msgBody.addBodyPart(textPart);
        msgBody.addBodyPart(htmlPart);

        // Add the child container to the wrapper object
        wrap.setContent(msgBody);

        // Create a multipart/mixed parent container
        MimeMultipart msg = new MimeMultipart("mixed");

        // Add the parent container to the message
        message.setContent(msg);

        // Add the multipart/alternative part to the message
        msg.addBodyPart(wrap);

        try {
            System.out.println("Attempting to send an email through Amazon SES " + "using the AWS SDK for Java...");

             ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
             message.writeTo(outputStream);
             ByteBuffer buf = ByteBuffer.wrap(outputStream.toByteArray());

             byte[] arr = new byte[buf.remaining()];
             buf.get(arr);

             SdkBytes data = SdkBytes.fromByteArray(arr);
             RawMessage rawMessage = RawMessage.builder()
                    .data(data)
                    .build();

             SendRawEmailRequest rawEmailRequest = SendRawEmailRequest.builder()
                    .rawMessage(rawMessage)
                    .build();

             client.sendRawEmail(rawEmailRequest);

         } catch (SesException e) {
            System.err.println(e.awsErrorDetails().errorMessage());
            System.exit(1);
         }

    }
}
```

------
#### [ JavaScript ]

The following code example is a complete solution for sending email through Amazon SES using the [AWS SDK for JavaScript in Node\.js](https://aws.amazon.com/sdk-for-node-js/)\. This code example assumes that you have installed the SDK for JavaScript in Node\.js\. You must also create a configuration file that contains your AWS Access Key ID, Secret Access Key, and preferred AWS Region\. For more information about creating this file, see [Loading Credentials in Node\.js from a JSON File](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/loading-node-credentials-json-file.html)\.

**Important**  
You use a shared credentials file to pass your AWS access key ID and secret access key\. As an alternative to using a shared credentials file, you can specify your AWS access key ID and secret access key by setting two environment variables \(`AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`, respectively\)\. This example doesn't function unless you specify your credentials using one of these methods\.

```
 1. 'use strict';
 2. 
 3. var aws = require('aws-sdk');
 4. 
 5. // Provide the full path to your config.json file. 
 6. aws.config.loadFromPath('./config.json');
 7. 
 8. // Replace sender@example.com with your "From" address.
 9. // This address must be verified with Amazon SES.
10. const sender = "Sender Name <sender@example.com>";
11. 
12. // Replace recipient@example.com with a "To" address. If your account 
13. // is still in the sandbox, this address must be verified.
14. const recipient = "recipient@example.com";
15. 
16. // Specify a configuration set. If you do not want to use a configuration
17. // set, comment the following variable, and the 
18. // ConfigurationSetName : configuration_set argument below.
19. const configuration_set = "ConfigSet";
20. 
21. // The subject line for the email.
22. const subject = "Amazon SES Test (AWS SDK for JavaScript in Node.js)";
23. 
24. // The email body for recipients with non-HTML email clients.
25. const body_text = "Amazon SES Test (SDK for JavaScript in Node.js)\r\n"
26.                 + "This email was sent with Amazon SES using the "
27.                 + "AWS SDK for JavaScript in Node.js.";
28.             
29. // The HTML body of the email.
30. const body_html = `<html>
31. <head></head>
32. <body>
33.   <h1>Amazon SES Test (SDK for JavaScript in Node.js)</h1>
34.   <p>This email was sent with
35.     <a href='https://aws.amazon.com/ses/'>Amazon SES</a> using the
36.     <a href='https://aws.amazon.com/sdk-for-node-js/'>
37.       AWS SDK for JavaScript in Node.js</a>.</p>
38. </body>
39. </html>`;
40. 
41. // The character encoding for the email.
42. const charset = "UTF-8";
43. 
44. // Create a new SES object. 
45. var ses = new aws.SES();
46. 
47. // Specify the parameters to pass to the API.
48. var params = { 
49.   Source: sender, 
50.   Destination: { 
51.     ToAddresses: [
52.       recipient 
53.     ],
54.   },
55.   Message: {
56.     Subject: {
57.       Data: subject,
58.       Charset: charset
59.     },
60.     Body: {
61.       Text: {
62.         Data: body_text,
63.         Charset: charset 
64.       },
65.       Html: {
66.         Data: body_html,
67.         Charset: charset
68.       }
69.     }
70.   },
71.   ConfigurationSetName: configuration_set
72. };
73. 
74. //Try to send the email.
75. ses.sendEmail(params, function(err, data) {
76.   // If something goes wrong, print an error message.
77.   if(err) {
78.     console.log(err.message);
79.   } else {
80.     console.log("Email sent! Message ID: ", data.MessageId);
81.   }
82. });
```

------
#### [ PHP ]

The following code example is a complete solution for sending email through Amazon SES using the [AWS SDK for PHP](https://aws.amazon.com/sdk-for-php/)\. This code example assumes that you have installed the AWS SDK for PHP, and that you have created a shared credentials file\. For more information about creating a shared credentials file, see [Create a shared credentials file](create-shared-credentials-file.md)\.

**Important**  
You use a shared credentials file to pass your AWS access key ID and secret access key\. As an alternative to using a shared credentials file, you can specify your AWS access key ID and secret access key by setting two environment variables \(`AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`, respectively\)\. This example doesn't function unless you specify your credentials using one of these methods\.

```
 1. <?php
 2. 
 3. // If necessary, modify the path in the require statement below to refer to the 
 4. // location of your Composer autoload.php file.
 5. require 'vendor/autoload.php';
 6. 
 7. use Aws\Ses\SesClient;
 8. use Aws\Exception\AwsException;
 9. 
10. // Create an SesClient. Change the value of the region parameter if you're 
11. // using an AWS Region other than US West (Oregon). Change the value of the
12. // profile parameter if you want to use a profile in your credentials file
13. // other than the default.
14. $SesClient = new SesClient([
15.     'profile' => 'default',
16.     'version' => '2010-12-01',
17.     'region'  => 'us-west-2'
18. ]);
19. 
20. // Replace sender@example.com with your "From" address.
21. // This address must be verified with Amazon SES.
22. $sender_email = 'sender@example.com';
23. 
24. // Replace these sample addresses with the addresses of your recipients. If
25. // your account is still in the sandbox, these addresses must be verified.
26. $recipient_emails = ['recipient1@example.com','recipient2@example.com'];
27. 
28. // Specify a configuration set. If you do not want to use a configuration
29. // set, comment the following variable, and the
30. // 'ConfigurationSetName' => $configuration_set argument below.
31. $configuration_set = 'ConfigSet';
32. 
33. $subject = 'Amazon SES test (AWS SDK for PHP)';
34. $plaintext_body = 'This email was sent with Amazon SES using the AWS SDK for PHP.' ;
35. $html_body =  '<h1>AWS Amazon Simple Email Service Test Email</h1>'.
36.               '<p>This email was sent with <a href="https://aws.amazon.com/ses/">'.
37.               'Amazon SES</a> using the <a href="https://aws.amazon.com/sdk-for-php/">'.
38.               'AWS SDK for PHP</a>.</p>';
39. $char_set = 'UTF-8';
40. 
41. try {
42.     $result = $SesClient->sendEmail([
43.         'Destination' => [
44.             'ToAddresses' => $recipient_emails,
45.         ],
46.         'ReplyToAddresses' => [$sender_email],
47.         'Source' => $sender_email,
48.         'Message' => [
49.           'Body' => [
50.               'Html' => [
51.                   'Charset' => $char_set,
52.                   'Data' => $html_body,
53.               ],
54.               'Text' => [
55.                   'Charset' => $char_set,
56.                   'Data' => $plaintext_body,
57.               ],
58.           ],
59.           'Subject' => [
60.               'Charset' => $char_set,
61.               'Data' => $subject,
62.           ],
63.         ],
64.         // If you aren't using a configuration set, comment or delete the
65.         // following line
66.         'ConfigurationSetName' => $configuration_set,
67.     ]);
68.     $messageId = $result['MessageId'];
69.     echo("Email sent! Message ID: $messageId"."\n");
70. } catch (AwsException $e) {
71.     // output error message if fails
72.     echo $e->getMessage();
73.     echo("The email was not sent. Error message: ".$e->getAwsErrorMessage()."\n");
74.     echo "\n";
75. }
```

------
#### [ Python ]

The following code example is a complete solution for sending email through Amazon SES using the [AWS SDK for Python \(Boto\)](https://aws.amazon.com/sdk-for-python/)\. This code example assumes that you have installed the AWS SDK for Python \(Boto\), and that you have created a shared credentials file\. For more information about creating a shared credentials file, see [Create a shared credentials file](create-shared-credentials-file.md)\.

**Important**  
You use a shared credentials file to pass your AWS access key ID and secret access key\. As an alternative to using a shared credentials file, you can specify your AWS access key ID and secret access key by setting two environment variables \(`AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`, respectively\)\. This example doesn't function unless you specify your credentials using one of these methods\.

```
 1. import boto3
 2. from botocore.exceptions import ClientError
 3. 
 4. # Replace sender@example.com with your "From" address.
 5. # This address must be verified with Amazon SES.
 6. SENDER = "Sender Name <sender@example.com>"
 7. 
 8. # Replace recipient@example.com with a "To" address. If your account 
 9. # is still in the sandbox, this address must be verified.
10. RECIPIENT = "recipient@example.com"
11. 
12. # Specify a configuration set. If you do not want to use a configuration
13. # set, comment the following variable, and the 
14. # ConfigurationSetName=CONFIGURATION_SET argument below.
15. CONFIGURATION_SET = "ConfigSet"
16. 
17. # If necessary, replace us-west-2 with the AWS Region you're using for Amazon SES.
18. AWS_REGION = "us-west-2"
19. 
20. # The subject line for the email.
21. SUBJECT = "Amazon SES Test (SDK for Python)"
22. 
23. # The email body for recipients with non-HTML email clients.
24. BODY_TEXT = ("Amazon SES Test (Python)\r\n"
25.              "This email was sent with Amazon SES using the "
26.              "AWS SDK for Python (Boto)."
27.             )
28.             
29. # The HTML body of the email.
30. BODY_HTML = """<html>
31. <head></head>
32. <body>
33.   <h1>Amazon SES Test (SDK for Python)</h1>
34.   <p>This email was sent with
35.     <a href='https://aws.amazon.com/ses/'>Amazon SES</a> using the
36.     <a href='https://aws.amazon.com/sdk-for-python/'>
37.       AWS SDK for Python (Boto)</a>.</p>
38. </body>
39. </html>
40.             """            
41. 
42. # The character encoding for the email.
43. CHARSET = "UTF-8"
44. 
45. # Create a new SES resource and specify a region.
46. client = boto3.client('ses',region_name=AWS_REGION)
47. 
48. # Try to send the email.
49. try:
50.     #Provide the contents of the email.
51.     response = client.send_email(
52.         Destination={
53.             'ToAddresses': [
54.                 RECIPIENT,
55.             ],
56.         },
57.         Message={
58.             'Body': {
59.                 'Html': {
60.                     'Charset': CHARSET,
61.                     'Data': BODY_HTML,
62.                 },
63.                 'Text': {
64.                     'Charset': CHARSET,
65.                     'Data': BODY_TEXT,
66.                 },
67.             },
68.             'Subject': {
69.                 'Charset': CHARSET,
70.                 'Data': SUBJECT,
71.             },
72.         },
73.         Source=SENDER,
74.         # If you are not using a configuration set, comment or delete the
75.         # following line
76.         ConfigurationSetName=CONFIGURATION_SET,
77.     )
78. # Display an error if something goes wrong.	
79. except ClientError as e:
80.     print(e.response['Error']['Message'])
81. else:
82.     print("Email sent! Message ID:"),
83.     print(response['MessageId'])
```

------
#### [ Ruby ]

The following code example is a complete solution for sending email through Amazon SES using the [AWS SDK for Ruby](https://aws.amazon.com/sdk-for-ruby/)\. This code example assumes that you've installed the AWS SDK for Ruby, and that you've created a shared credentials file\. For more information about installing the SDK for Ruby, see [Installing the AWS SDK for Ruby](https://docs.aws.amazon.com/sdk-for-ruby/v3/developer-guide/setup-install.html) in the *AWS SDK for Ruby Developer Guide*\. For more information about creating a shared credentials file, see [Create a shared credentials file](create-shared-credentials-file.md)\.

**Important**  
You use a shared credentials file to pass your AWS access key ID and secret access key\. As an alternative to using a shared credentials file, you can specify your AWS access key ID and secret access key by setting two environment variables \(`AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`, respectively\)\. This example doesn't function unless you specify your credentials using one of these methods\.

```
 1. require 'aws-sdk'
 2. 
 3. # Replace sender@example.com with your "From" address.
 4. # This address must be verified with Amazon SES.
 5. sender = "sender@example.com"
 6. 
 7. # Replace recipient@example.com with a "To" address. If your account 
 8. # is still in the sandbox, this address must be verified.
 9. recipient = "recipient@example.com"
10. 
11. # Specify a configuration set. If you do not want to use a configuration
12. # set, comment the following variable and the 
13. # configuration_set_name: configsetname argument below. 
14. configsetname = "ConfigSet"
15.   
16. # Replace us-west-2 with the AWS Region you're using for Amazon SES.
17. awsregion = "us-west-2"
18. 
19. # The subject line for the email.
20. subject = "Amazon SES test (AWS SDK for Ruby)"
21. 
22. # The HTML body of the email.
23. htmlbody =
24.   '<h1>Amazon SES test (AWS SDK for Ruby)</h1>'\
25.   '<p>This email was sent with <a href="https://aws.amazon.com/ses/">'\
26.   'Amazon SES</a> using the <a href="https://aws.amazon.com/sdk-for-ruby/">'\
27.   'AWS SDK for Ruby</a>.'
28. 
29. # The email body for recipients with non-HTML email clients.  
30. textbody = "This email was sent with Amazon SES using the AWS SDK for Ruby."
31. 
32. # Specify the text encoding scheme.
33. encoding = "UTF-8"
34. 
35. # Create a new SES resource and specify a region
36. ses = Aws::SES::Client.new(region: awsregion)
37. 
38. # Try to send the email.
39. begin
40. 
41.   # Provide the contents of the email.
42.   resp = ses.send_email({
43.     destination: {
44.       to_addresses: [
45.         recipient,
46.       ],
47.     },
48.     message: {
49.       body: {
50.         html: {
51.           charset: encoding,
52.           data: htmlbody,
53.         },
54.         text: {
55.           charset: encoding,
56.           data: textbody,
57.         },
58.       },
59.       subject: {
60.         charset: encoding,
61.         data: subject,
62.       },
63.     },
64.   source: sender,
65.   # Comment or remove the following line if you are not using 
66.   # a configuration set
67.   configuration_set_name: configsetname,
68.   })
69.   puts "Email sent!"
70. 
71. # If something goes wrong, display an error message.
72. rescue Aws::SES::Errors::ServiceError => error
73.   puts "Email not sent. Error message: #{error}"
74. 
75. end
```

------


****  

|  | 
| --- |
| For information and discussions about a variety of topics related to Amazon SES, see the [AWS Messaging and Targeting Blog](https://aws.amazon.com//blogs/messaging-and-targeting/)\. To browse and post questions, go to the [Amazon SES Forum](https://forums.aws.amazon.com/forum.jspa?forumID=90)\. | 
