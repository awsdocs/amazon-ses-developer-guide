# Sending raw email using AWS SDKs<a name="examples-send-raw-using-sdk"></a>

The AWS SDKs contain built\-in methods for interacting with Amazon SES and several other AWS services\. If you plan to use Amazon SES along with other AWS services, we recommend that you use an SDK\. To learn more about the AWS SDKs, see [Tools for Amazon Web Services](https://aws.amazon.com/tools/?nc1=h_ls#sdk)

In this section, you will find code examples in several programming languages that demonstrate the process of sending raw email through Amazon SES using the AWS SDKs\.

------
#### [ Java ]

The following code example shows how to use the [JavaMail](https://javaee.github.io/javamail/) library and the [AWS SDK for Java](https://aws.amazon.com/sdk-for-java) to compose and send a raw email that contains an HTML part, a text part, and an attachment\.

This code example assumes you have installed the AWS SDK for Java, and that you have created a shared credentials file\. For more information about creating a shared credentials file, see [Create a shared credentials file](create-shared-credentials-file.md)\.

**Important**  
You use a shared credentials file to pass your AWS access key ID and secret access key\. As an alternative to using a shared credentials file, you can specify your AWS access key ID and secret access key by setting two environment variables \(`AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`, respectively\)\. This example doesn't function unless you specify your credentials using one of these methods\.

```
  1. package com.amazonaws.samples;
  2. 
  3. import java.io.ByteArrayOutputStream;
  4. import java.io.IOException;
  5. import java.io.PrintStream;
  6. import java.nio.ByteBuffer;
  7. import java.util.Properties;
  8. 
  9. // JavaMail libraries. Download the JavaMail API 
 10. // from https://javaee.github.io/javamail/
 11. import javax.activation.DataHandler;
 12. import javax.activation.DataSource;
 13. import javax.activation.FileDataSource;
 14. import javax.mail.Message;
 15. import javax.mail.MessagingException;
 16. import javax.mail.Session;
 17. import javax.mail.internet.AddressException;
 18. import javax.mail.internet.InternetAddress;
 19. import javax.mail.internet.MimeBodyPart;
 20. import javax.mail.internet.MimeMessage;
 21. import javax.mail.internet.MimeMultipart;
 22. 
 23. // AWS SDK libraries. Download the AWS SDK for Java 
 24. // from https://aws.amazon.com/sdk-for-java
 25. import com.amazonaws.regions.Regions;
 26. import com.amazonaws.services.simpleemail.AmazonSimpleEmailService;
 27. import com.amazonaws.services.simpleemail.AmazonSimpleEmailServiceClientBuilder;
 28. import com.amazonaws.services.simpleemail.model.RawMessage;
 29. import com.amazonaws.services.simpleemail.model.SendRawEmailRequest;
 30. 
 31. public class AmazonSESSample {
 32. 
 33. 	// Replace sender@example.com with your "From" address.
 34. 	// This address must be verified with Amazon SES.
 35. 	private static String SENDER = "Sender Name <sender@example.com>";
 36. 
 37. 	// Replace recipient@example.com with a "To" address. If your account 
 38. 	// is still in the sandbox, this address must be verified.
 39. 	private static String RECIPIENT = "recipient@example.com";
 40. 
 41. 	// Specify a configuration set. If you do not want to use a configuration
 42. 	// set, comment the following variable, and the 
 43. 	// ConfigurationSetName=CONFIGURATION_SET argument below.
 44. 	private static String CONFIGURATION_SET = "ConfigSet";
 45. 
 46. 	// The subject line for the email.
 47. 	private static String SUBJECT = "Customer service contact info";
 48. 
 49. 	// The full path to the file that will be attached to the email.
 50. 	// If you're using Windows, escape backslashes as shown in this variable.
 51. 	private static String ATTACHMENT = "C:\\Users\\sender\\customers-to-contact.xlsx";
 52. 
 53. 	// The email body for recipients with non-HTML email clients.
 54. 	private static String BODY_TEXT = "Hello,\r\n"
 55.                                         + "Please see the attached file for a list "
 56.                                         + "of customers to contact.";
 57. 
 58. 	// The HTML body of the email.
 59. 	private static String BODY_HTML = "<html>"
 60.                                         + "<head></head>"
 61.                                         + "<body>"
 62.                                         + "<h1>Hello!</h1>"
 63.                                         + "<p>Please see the attached file for a "
 64.                                         + "list of customers to contact.</p>"
 65.                                         + "</body>"
 66.                                         + "</html>";
 67. 
 68.     public static void main(String[] args) throws AddressException, MessagingException, IOException {
 69.             	
 70.     	Session session = Session.getDefaultInstance(new Properties());
 71.         
 72.         // Create a new MimeMessage object.
 73.         MimeMessage message = new MimeMessage(session);
 74.         
 75.         // Add subject, from and to lines.
 76.         message.setSubject(SUBJECT, "UTF-8");
 77.         message.setFrom(new InternetAddress(SENDER));
 78.         message.setRecipients(Message.RecipientType.TO, InternetAddress.parse(RECIPIENT));
 79. 
 80.         // Create a multipart/alternative child container.
 81.         MimeMultipart msg_body = new MimeMultipart("alternative");
 82.         
 83.         // Create a wrapper for the HTML and text parts.        
 84.         MimeBodyPart wrap = new MimeBodyPart();
 85.         
 86.         // Define the text part.
 87.         MimeBodyPart textPart = new MimeBodyPart();
 88.         textPart.setContent(BODY_TEXT, "text/plain; charset=UTF-8");
 89.                 
 90.         // Define the HTML part.
 91.         MimeBodyPart htmlPart = new MimeBodyPart();
 92.         htmlPart.setContent(BODY_HTML,"text/html; charset=UTF-8");
 93.                 
 94.         // Add the text and HTML parts to the child container.
 95.         msg_body.addBodyPart(textPart);
 96.         msg_body.addBodyPart(htmlPart);
 97.         
 98.         // Add the child container to the wrapper object.
 99.         wrap.setContent(msg_body);
100.         
101.         // Create a multipart/mixed parent container.
102.         MimeMultipart msg = new MimeMultipart("mixed");
103.         
104.         // Add the parent container to the message.
105.         message.setContent(msg);
106.         
107.         // Add the multipart/alternative part to the message.
108.         msg.addBodyPart(wrap);
109.         
110.         // Define the attachment
111.         MimeBodyPart att = new MimeBodyPart();
112.         DataSource fds = new FileDataSource(ATTACHMENT);
113.         att.setDataHandler(new DataHandler(fds));
114.         att.setFileName(fds.getName());
115.         
116.         // Add the attachment to the message.
117.         msg.addBodyPart(att);
118. 
119.         // Try to send the email.
120.         try {
121.             System.out.println("Attempting to send an email through Amazon SES "
122.                               +"using the AWS SDK for Java...");
123. 
124.             // Instantiate an Amazon SES client, which will make the service 
125.             // call with the supplied AWS credentials.
126.             AmazonSimpleEmailService client = 
127.                     AmazonSimpleEmailServiceClientBuilder.standard()
128.                     // Replace US_WEST_2 with the AWS Region you're using for
129.                     // Amazon SES.
130.                     .withRegion(Regions.US_WEST_2).build();
131.             
132.             // Print the raw email content on the console
133.             PrintStream out = System.out;
134.             message.writeTo(out);
135. 
136.             // Send the email.
137.             ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
138.             message.writeTo(outputStream);
139.             RawMessage rawMessage = 
140.             		new RawMessage(ByteBuffer.wrap(outputStream.toByteArray()));
141. 
142.             SendRawEmailRequest rawEmailRequest = 
143.             		new SendRawEmailRequest(rawMessage)
144.             		    .withConfigurationSetName(CONFIGURATION_SET);
145.             
146.             client.sendRawEmail(rawEmailRequest);
147.             System.out.println("Email sent!");
148.         // Display an error if something goes wrong.
149.         } catch (Exception ex) {
150.           System.out.println("Email Failed");
151.             System.err.println("Error message: " + ex.getMessage());
152.             ex.printStackTrace();
153.         }
154.     }
155. }
```

------
#### [ PHP ]

The following code example shows how to use the [PHPMailer](https://github.com/PHPMailer/PHPMailer) package and the [AWS SDK for PHP](https://aws.amazon.com/sdk-for-php/) to compose and send a raw email that contains an HTML part, a text part, and an attachment\.

This code example assumes that you have installed the [PHPMailer package](https://packagist.org/packages/phpmailer/phpmailer) using [Composer](https://getcomposer.org/)\. It also assumes that you have installed the AWS SDK for PHP, and that you have created a shared credentials file\. For more information about creating a shared credentials file, see [Create a shared credentials file](create-shared-credentials-file.md)\.

**Important**  
You use a shared credentials file to pass your AWS access key ID and secret access key\. As an alternative to using a shared credentials file, you can specify your AWS access key ID and secret access key by setting two environment variables \(`AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`, respectively\)\. This example doesn't function unless you specify your credentials using one of these methods\.

```
 1. <?php
 2. require 'vendor/autoload.php';
 3. use PHPMailer\PHPMailer\PHPMailer;
 4. use Aws\Ses\SesClient;
 5. use Aws\Ses\Exception\SesException;
 6. 
 7. // Replace sender@example.com with your "From" address. 
 8. // This address must be verified with Amazon SES.
 9. $sender = 'sender@example.com';           
10. $sendername = 'Sender Name';
11. 
12. // Replace recipient@example.com with a "To" address. If your account 
13. // is still in the sandbox, this address must be verified.
14. $recipient = 'recipient@example.com';    
15. 
16. // Specify a configuration set.
17. $configset = 'ConfigSet';
18. 
19. // Replace us-west-2 with the AWS Region you're using for Amazon SES.
20. $region = 'us-west-2'; 
21. 
22. $subject = 'List of customers to contact';
23. 
24. $htmlbody = <<<EOD
25. <html>
26. <head></head>
27. <body>
28. <h1>Hello!</h1>
29. <p>Please see the attached file for a list of customers to contact.</p>
30. </body>
31. </html>
32. EOD;
33. 
34. $textbody = <<<EOD
35. Hello,
36. Please see the attached file for a list of customers to contact.
37. EOD;
38. 
39. // The full path to the file that will be attached to the email. 
40. $att = 'path/to/customers-to-contact.xlsx';
41. 
42. // Create an SesClient.
43. $client = SesClient::factory(array(
44.     'version'=> 'latest',     
45.     'region' => $region
46. ));
47. 
48. // Create a new PHPMailer object.
49. $mail = new PHPMailer;
50. 
51. // Add components to the email.
52. $mail->setFrom($sender, $sendername);
53. $mail->addAddress($recipient);
54. $mail->Subject = $subject;
55. $mail->Body = $htmlbody;
56. $mail->AltBody = $textbody;
57. $mail->addAttachment($att);
58. $mail->addCustomHeader('X-SES-CONFIGURATION-SET', $configset);
59. 
60. // Attempt to assemble the above components into a MIME message.
61. if (!$mail->preSend()) {
62.     echo $mail->ErrorInfo;
63. } else {
64.     // Create a new variable that contains the MIME message.
65.     $message = $mail->getSentMIMEMessage();
66. }
67. 
68. // Try to send the message.
69. try {
70.     $result = $client->sendRawEmail([
71.         'RawMessage' => [
72.             'Data' => $message
73.         ]
74.     ]);
75.     // If the message was sent, show the message ID.
76.     $messageId = $result->get('MessageId');
77.     echo("Email sent! Message ID: $messageId"."\n");
78. } catch (SesException $error) {
79.     // If the message was not sent, show a message explaining what went wrong.
80.     echo("The email was not sent. Error message: "
81.          .$error->getAwsErrorMessage()."\n");
82. }
83. 
84. ?>
```

------
#### [ Python ]

The following code example shows how to use the Python [email](https://docs.python.org/3/library/email.html) package and the [AWS SDK for Python \(Boto\)](https://aws.amazon.com/sdk-for-python/) to compose and send a raw email that contains an HTML part, a text part, and an attachment\.

This code example assumes that you have installed the AWS SDK for Python \(Boto\), and that you have created a shared credentials file\. For more information about creating a shared credentials file, see [Create a shared credentials file](create-shared-credentials-file.md)\.

**Important**  
You use a shared credentials file to pass your AWS access key ID and secret access key\. As an alternative to using a shared credentials file, you can specify your AWS access key ID and secret access key by setting two environment variables \(`AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`, respectively\)\. This example doesn't function unless you specify your credentials using one of these methods\.

```
  1. import os
  2. import boto3
  3. from botocore.exceptions import ClientError
  4. from email.mime.multipart import MIMEMultipart
  5. from email.mime.text import MIMEText
  6. from email.mime.application import MIMEApplication
  7. 
  8. # Replace sender@example.com with your "From" address.
  9. # This address must be verified with Amazon SES.
 10. SENDER = "Sender Name <sender@example.com>"
 11. 
 12. # Replace recipient@example.com with a "To" address. If your account 
 13. # is still in the sandbox, this address must be verified.
 14. RECIPIENT = "recipient@example.com"
 15. 
 16. # Specify a configuration set. If you do not want to use a configuration
 17. # set, comment the following variable, and the 
 18. # ConfigurationSetName=CONFIGURATION_SET argument below.
 19. CONFIGURATION_SET = "ConfigSet"
 20. 
 21. # If necessary, replace us-west-2 with the AWS Region you're using for Amazon SES.
 22. AWS_REGION = "us-west-2"
 23. 
 24. # The subject line for the email.
 25. SUBJECT = "Customer service contact info"
 26. 
 27. # The full path to the file that will be attached to the email.
 28. ATTACHMENT = "path/to/customers-to-contact.xlsx"
 29. 
 30. # The email body for recipients with non-HTML email clients.
 31. BODY_TEXT = "Hello,\r\nPlease see the attached file for a list of customers to contact."
 32. 
 33. # The HTML body of the email.
 34. BODY_HTML = """\
 35. <html>
 36. <head></head>
 37. <body>
 38. <h1>Hello!</h1>
 39. <p>Please see the attached file for a list of customers to contact.</p>
 40. </body>
 41. </html>
 42. """
 43. 
 44. # The character encoding for the email.
 45. CHARSET = "utf-8"
 46. 
 47. # Create a new SES resource and specify a region.
 48. client = boto3.client('ses',region_name=AWS_REGION)
 49. 
 50. # Create a multipart/mixed parent container.
 51. msg = MIMEMultipart('mixed')
 52. # Add subject, from and to lines.
 53. msg['Subject'] = SUBJECT 
 54. msg['From'] = SENDER 
 55. msg['To'] = RECIPIENT
 56. 
 57. # Create a multipart/alternative child container.
 58. msg_body = MIMEMultipart('alternative')
 59. 
 60. # Encode the text and HTML content and set the character encoding. This step is
 61. # necessary if you're sending a message with characters outside the ASCII range.
 62. textpart = MIMEText(BODY_TEXT.encode(CHARSET), 'plain', CHARSET)
 63. htmlpart = MIMEText(BODY_HTML.encode(CHARSET), 'html', CHARSET)
 64. 
 65. # Add the text and HTML parts to the child container.
 66. msg_body.attach(textpart)
 67. msg_body.attach(htmlpart)
 68. 
 69. # Define the attachment part and encode it using MIMEApplication.
 70. att = MIMEApplication(open(ATTACHMENT, 'rb').read())
 71. 
 72. # Add a header to tell the email client to treat this part as an attachment,
 73. # and to give the attachment a name.
 74. att.add_header('Content-Disposition','attachment',filename=os.path.basename(ATTACHMENT))
 75. 
 76. # Attach the multipart/alternative child container to the multipart/mixed
 77. # parent container.
 78. msg.attach(msg_body)
 79. 
 80. # Add the attachment to the parent container.
 81. msg.attach(att)
 82. #print(msg)
 83. try:
 84.     #Provide the contents of the email.
 85.     response = client.send_raw_email(
 86.         Source=SENDER,
 87.         Destinations=[
 88.             RECIPIENT
 89.         ],
 90.         RawMessage={
 91.             'Data':msg.as_string(),
 92.         },
 93.         ConfigurationSetName=CONFIGURATION_SET
 94.     )
 95. # Display an error if something goes wrong.	
 96. except ClientError as e:
 97.     print(e.response['Error']['Message'])
 98. else:
 99.     print("Email sent! Message ID:"),
100.     print(response['MessageId'])
```

------
#### [ Ruby ]

The following code example shows how to use the Ruby [MIME](https://rubygems.org/gems/mime/versions/0.4.4) gem and the [AWS SDK for Ruby](https://aws.amazon.com/sdk-for-ruby/) to compose and send a raw email that contains an HTML part, a text part, and an attachment\.

This code example assumes that you have installed the AWS SDK for Ruby and the MIME gem, and that you have created a shared credentials file\. For more information about creating a shared credentials file, see [Create a shared credentials file](create-shared-credentials-file.md)\.

**Important**  
You use a shared credentials file to pass your AWS access key ID and secret access key\. As an alternative to using a shared credentials file, you can specify your AWS access key ID and secret access key by setting two environment variables \(`AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`, respectively\)\. This example doesn't function unless you specify your credentials using one of these methods\.

```
 1. require 'base64'  #standard library
 2. require 'aws-sdk' #gem install aws-sdk
 3. require 'mime'    #gem install mime
 4. 
 5. # Replace sender@example.com with your "From" address.
 6. # This address must be verified with Amazon SES.
 7. sender = "sender@example.com"
 8. sendername = "Sender Name"
 9. 
10. # Replace recipient@example.com with a "To" address. If your account 
11. # is still in the sandbox, this address must be verified.
12. recipient = "recipient@example.com"
13. 
14. # Specify a configuration set. 
15. configsetname = "ConfigSet"
16.   
17. # Replace us-west-2 with the AWS Region you're using for Amazon SES.
18. awsregion = "us-west-2"
19. 
20. # The subject line for the email.
21. subject = "Customer service contact info"
22. 
23. # The full path to the file that will be attached to the email.
24. attachment = "path/to/customers-to-contact.xlsx"
25. 
26. # The email body for recipients with non-HTML email clients.  
27. textbody = """
28. Hello,
29. Please see the attached file for a list of customers to contact.
30. """
31. 
32. # The HTML body of the email.
33. htmlbody = """
34. <html>
35. <head></head>
36. <body>
37. <h1>Hello!</h1>
38. <p>Please see the attached file for a list of customers to contact.</p>
39. </body>
40. </html>
41. """ 
42. 
43. # Create a new MIME text object that contains the base64-encoded content of the
44. # file that will be attached to the message.
45. file = MIME::Application.new(Base64::encode64(open(attachment,"rb").read))
46. 
47. # Specify that the file is a base64-encoded attachment to ensure that the 
48. # receiving client handles it correctly. 
49. file.transfer_encoding = 'base64'
50. file.disposition = 'attachment'
51. 
52. # Create a MIME Multipart Mixed object. This object will contain the body of the
53. # email and the attachment.
54. msg_mixed = MIME::Multipart::Mixed.new
55. 
56. # Create a MIME Multipart Alternative object. This object will contain both the
57. # HTML and plain text versions of the email.
58. msg_body = MIME::Multipart::Alternative.new
59. 
60. # Add the plain text and HTML content to the Multipart Alternative part.
61. msg_body.add(MIME::Text.new(textbody,'plain'))
62. msg_body.add(MIME::Text.new(htmlbody,'html'))
63. 
64. # Add the Multipart Alternative part to the Multipart Mixed part.
65. msg_mixed.add(msg_body)
66. 
67. # Add the attachment to the Multipart Mixed part.
68. msg_mixed.attach(file, 'filename' => attachment)
69. 
70. # Create a new Mail object that contains the entire Multipart Mixed object. 
71. # This object also contains the message headers.
72. msg = MIME::Mail.new(msg_mixed)
73. msg.to = { recipient => nil }
74. msg.from = { sender => sendername }
75. msg.subject = subject
76. msg.headers.set('X-SES-CONFIGURATION-SET',configsetname)
77. 
78. # Create a new SES resource and specify a region
79. ses = Aws::SES::Client.new(region: awsregion)
80. 
81. # Try to send the email.
82. begin
83. 
84.   # Provide the contents of the email.
85.   resp = ses.send_raw_email({
86.     raw_message: {
87.       data: msg.to_s
88.     }
89.   })
90.   # If the message was sent, show the message ID.
91.   puts "Email sent! Message ID: " + resp[0].to_s
92. 
93. # If the message was not sent, show a message explaining what went wrong.
94. rescue Aws::SES::Errors::ServiceError => error
95.   puts "Email not sent. Error message: #{error}"
96. 
97. end
```

------