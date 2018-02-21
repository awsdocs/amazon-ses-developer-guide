# Sending Raw Email Using the Amazon SES API<a name="send-email-raw"></a>

You can use the Amazon SES `SendRawEmail` operation to send highly customized messages to your recipients\.

This section includes procedures for constructing and sending raw email using the Amazon SES API\.

## About Email Header Fields<a name="send-email-raw-headers"></a>

Simple Mail Transfer Protocol \(SMTP\) specifies how email messages are to be sent by defining the mail envelope and some of its parameters, but it does not concern itself with the content of the message\. Instead, the Internet Message Format \([RFC 5322](https://www.ietf.org/rfc/rfc5322.txt)\) defines how the message is to be constructed\.

With the Internet Message Format specification, every email message consists of a header and a body\. The header consists of message metadata, and the body contains the message itself\. For more information about email headers and bodies, see [Email Format and Amazon SES](email-format.md)\.

## Using MIME<a name="send-email-raw-mime"></a>

The SMTP protocol is designed for sending email messages composed of 7\-bit ASCII characters\. While this works well for many use cases, it is insufficient for non\-ASCII text encodings \(such as Unicode\), binary content, or attachments\. The Multipurpose Internet Mail Extensions standard \(MIME\) was developed to overcome these limitations, making it possible to send many other kinds of content using SMTP\.

The MIME standard works by breaking the message body into multiple parts and then specifying what is to be done with each part\. For example, one part of an email message body might be plain text, while another might be HTML\. In addition, MIME allows email messages to contain one or more attachments\. Message recipients can view the attachments from within their email clients, or they can save the attachments\.

The message header and content are separated by a blank line\. Each part of the email is separated by a boundary, a string of characters that denotes the beginning and ending of each part\.

The multipart message in the following example contains a text and an HTML part\. It also contains an attachment\. 

```
 1. From: "Sender Name" <sender@example.com>
 2. To: recipient@example.com
 3. Subject: Customer service contact info
 4. Content-Type: multipart/mixed;
 5.     boundary="a3f166a86b56ff6c37755292d690675717ea3cd9de81228ec2b76ed4a15d6d1a"
 6. 
 7. --a3f166a86b56ff6c37755292d690675717ea3cd9de81228ec2b76ed4a15d6d1a
 8. Content-Type: multipart/alternative;
 9.     boundary="sub_a3f166a86b56ff6c37755292d690675717ea3cd9de81228ec2b76ed4a15d6d1a"
10. 
11. --sub_a3f166a86b56ff6c37755292d690675717ea3cd9de81228ec2b76ed4a15d6d1a
12. Content-Type: text/plain; charset=iso-8859-1
13. Content-Transfer-Encoding: quoted-printable
14. 
15. Please see the attached file for a list of customers to contact.
16. 
17. --sub_a3f166a86b56ff6c37755292d690675717ea3cd9de81228ec2b76ed4a15d6d1a
18. Content-Type: text/html; charset=iso-8859-1
19. Content-Transfer-Encoding: quoted-printable
20. 
21. <html>
22. <head></head>
23. <body>
24. <h1>Hello!</h1>
25. <p>Please see the attached file for a list of customers to contact.</p>
26. </body>
27. </html>
28. 
29. --sub_a3f166a86b56ff6c37755292d690675717ea3cd9de81228ec2b76ed4a15d6d1a--
30. 
31. --a3f166a86b56ff6c37755292d690675717ea3cd9de81228ec2b76ed4a15d6d1a
32. Content-Type: text/plain; name="customers.txt"
33. Content-Description: customers.txt
34. Content-Disposition: attachment;filename="customers.txt";
35.     creation-date="Sat, 05 Aug 2017 19:35:36 GMT";
36. Content-Transfer-Encoding: base64
37. 
38. SUQsRmlyc3ROYW1lLExhc3ROYW1lLENvdW50cnkKMzQ4LEpvaG4sU3RpbGVzLENhbmFkYQo5MjM4
39. OSxKaWUsTGl1LENoaW5hCjczNCxTaGlybGV5LFJvZHJpZ3VleixVbml0ZWQgU3RhdGVzCjI4OTMs
40. QW5heWEsSXllbmdhcixJbmRpYQ==
41. 
42. --a3f166a86b56ff6c37755292d690675717ea3cd9de81228ec2b76ed4a15d6d1a--
```

The content type for the message is `multipart/mixed`, which indicates that the message has many parts \(in this example, a body and an attachment\), and the receiving client must handle each part separately\. Nested within the body section, there is a second part that uses the `multipart/alternative` content type\. This content type indicates that each part contains alternative versions of the same content \(in this case, a text version and an HTML version\)\. When sent, if the recipient's email client can display HTML, then the HTML version of the body text is displayed\. If the recipient's email client cannot display HTML, then the plain text version is shown\. Both versions of the message will also contain an attachment \(in this case, a short text file that contains some customer names\)\.

When you nest a MIME part within another part, as in this example, the nested part must use a `boundary` parameter that is distinct from the `boundary` parameter in the parent part\. These boundaries should be unique strings of characters\. To define a boundary between MIME parts, type two hyphens \(\-\-\) followed by the boundary string\. At the end of a MIME part, place two hyphens at both the beginning and the end of the boundary string\.

### MIME Encoding<a name="send-email-mime-encoding"></a>

Because of the 7\-bit ASCII restriction of SMTP, any content containing 8\-bit characters must first be converted to 7\-bit ASCII before sending\. MIME defines a *Content\-Transfer\-Encoding* header field for this purpose\. 

By convention, the most common encoding scheme is base64, where 8\-bit binary content is encoded using a well\-defined set of 7\-bit ASCII characters\. Upon receipt, the email client inspects the Content\-Transfer\-Encoding header field, and can immediately perform a base64 decode operation on the content, thus returning it to its original form\. With most email clients, the encoding and decoding occur automatically, and the user need not be aware of it\.

In the example above, the "customers\.txt" attachment must be decoded from base64 format in order to be read\. Some email clients will encode all MIME parts in base64 format, even if they were originally in plain text\. This is not normally an issue, since email clients perform the encoding and decoding automatically\.

**Note**  
For a list of MIME types that Amazon SES accepts, see [Appendix: Unsupported Attachment Types](mime-types-appendix.md)\.

If you want certain parts of a message, like some headers, to contain characters other than 7\-bit ASCII, then you must use MIME encoded\-word syntax \(RFC 2047\) instead of a literal string\. MIME encoded\-word syntax uses the following form: *=?charset?encoding?encoded\-text?=*\. For more information, see [RFC 2047](https://tools.ietf.org/html/rfc2047)\. If you want to send to or from email addresses that contain unicode characters in the domain part of an address, you must encode the domain using Punycode\. For more information, see [RFC 3492](https://tools.ietf.org/html/rfc3492)\. 

## Sending a Raw Email Using the Amazon SES API<a name="send-email-raw-api"></a>

The Amazon SES API provides the `SendRawEmail` action, which lets you compose and send an email message in the format that you specify\. For a complete description of `SendRawEmail`, see the [Amazon Simple Email Service API Reference](http://docs.aws.amazon.com/ses/latest/APIReference/)\.

**Note**  
For tips on how to increase your email sending speed when you make multiple calls to `SendRawEmail`, see [Increasing Throughput with Amazon SES](throughput-problems.md)\.

The message body must contain a properly formatted, raw email message, with appropriate header fields and message body encoding\. Although it is possible to construct the raw message manually within an application, it is much easier to do so using existing mail libraries\. 

------
#### [ Java ]

The following code example shows how to use the [JavaMail](https://javaee.github.io/javamail/) library and the [AWS SDK for Java](https://aws.amazon.com/sdk-for-java) to compose and send a raw email\.

```
  1. import java.io.ByteArrayOutputStream;
  2. import java.io.IOException;
  3. import java.io.PrintStream;
  4. import java.nio.ByteBuffer;
  5. import java.util.Properties;
  6. 
  7. // JavaMail libraries. Download the JavaMail API from 
  8. // https://javaee.github.io/javamail. After you download the jar file, 
  9. // include it in the build path for your project. 
 10. import javax.activation.DataHandler;
 11. import javax.activation.DataSource;
 12. import javax.activation.FileDataSource;
 13. import javax.mail.Message;
 14. import javax.mail.MessagingException;
 15. import javax.mail.Session;
 16. import javax.mail.internet.AddressException;
 17. import javax.mail.internet.InternetAddress;
 18. import javax.mail.internet.MimeBodyPart;
 19. import javax.mail.internet.MimeMessage;
 20. import javax.mail.internet.MimeMultipart;
 21. import javax.mail.internet.MimeUtility;
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
 33.     // Replace sender@example.com with your "From" address.
 34.     // This address must be verified with Amazon SES.
 35.     private static String SENDER = "Sender Name <sender@example.com>";
 36. 
 37.     // Replace recipient@example.com with a "To" address. If your account 
 38.     // is still in the sandbox, this address must be verified.
 39.     private static String RECIPIENT = "recipient@example.com";
 40. 
 41.     // Specify a configuration set. If you do not want to use a configuration
 42.     // set, comment the following variable, and the 
 43.     // ConfigurationSetName=CONFIGURATION_SET argument below.
 44.     private static String CONFIGURATION_SET = "ConfigSet";
 45. 
 46.     // The subject line for the email.
 47.     private static String SUBJECT = "Customer service contact info";
 48. 
 49.     // The full path to the file that will be attached to the email.
 50.     // If you are using Windows, escape backslashes as shown in this variable.
 51.     private static String ATTACHMENT = "C:\\Users\\sender\\customers-to-contact.xlsx";
 52. 
 53.     // The email body for recipients with non-HTML email clients.
 54.     private static String BODY_TEXT = "Hello,\r\n"
 55.                                     + "Please see the attached file for a list "
 56.                                     + "of customers to contact.";
 57. 
 58.     // The HTML body of the email.
 59.     private static String BODY_HTML = "<html>"
 60.                                     + "<head></head>"
 61.                                     + "<body>"
 62.                                     + "<h1>Hello!</h1>"
 63.                                     + "<p>Please see the attached file for a "
 64.                                     + "list of customers to contact.</p>"
 65.                                     + "</body>"
 66.                                     + "</html>";
 67. 
 68.     public static void main(String[] args) throws AddressException, MessagingException, IOException {
 69.         
 70.         String DefaultCharSet = MimeUtility.getDefaultJavaCharset();
 71.         
 72.         Session session = Session.getDefaultInstance(new Properties());
 73.         
 74.         // Create a new MimeMessage object.
 75.         MimeMessage message = new MimeMessage(session);
 76.         
 77.         // Add subject, from and to lines.
 78.         message.setSubject(SUBJECT, "UTF-8");
 79.         message.setFrom(new InternetAddress(SENDER));
 80.         message.setRecipients(Message.RecipientType.TO, InternetAddress.parse(RECIPIENT));
 81. 
 82.         // Create a multipart/alternative child container.
 83.         MimeMultipart msg_body = new MimeMultipart("alternative");
 84.         
 85.         // Create a wrapper for the HTML and text parts.        
 86.         MimeBodyPart wrap = new MimeBodyPart();
 87.         
 88.         // Define the text part.
 89.         MimeBodyPart textPart = new MimeBodyPart();
 90.         // Encode the text content and set the character encoding. This step is
 91.         // necessary if you're sending a message with characters outside the
 92.         // ASCII range.
 93.         textPart.setContent(MimeUtility
 94.                 .encodeText(BODY_TEXT,DefaultCharSet,"B"), "text/plain; charset=UTF-8");
 95.         textPart.setHeader("Content-Transfer-Encoding", "base64");
 96.         
 97.         // Define the HTML part.
 98.         MimeBodyPart htmlPart = new MimeBodyPart();
 99.         // Encode the HTML content and set the character encoding.
100.         htmlPart.setContent(MimeUtility
101.                 .encodeText(BODY_HTML,DefaultCharSet,"B"),"text/html; charset=UTF-8");
102.         htmlPart.setHeader("Content-Transfer-Encoding", "base64");
103.         
104.         // Add the text and HTML parts to the child container.
105.         msg_body.addBodyPart(textPart);
106.         msg_body.addBodyPart(htmlPart);
107.         
108.         // Add the child container to the wrapper object.
109.         wrap.setContent(msg_body);
110.         
111.         // Create a multipart/mixed parent container.
112.         MimeMultipart msg = new MimeMultipart("mixed");
113.         
114.         // Add the parent container to the message.
115.         message.setContent(msg);
116.         
117.         // Add the multipart/alternative part to the message.
118.         msg.addBodyPart(wrap);
119.         
120.         // Define the attachment
121.         MimeBodyPart att = new MimeBodyPart();
122.         DataSource fds = new FileDataSource(ATTACHMENT);
123.         att.setDataHandler(new DataHandler(fds));
124.         att.setFileName(fds.getName());
125.         
126.         // Add the attachment to the message.
127.         msg.addBodyPart(att);
128. 
129.         // Try to send the email.
130.         try {
131.             System.out.println("Attempting to send an email through Amazon SES "
132.                               +"using the AWS SDK for Java...");
133. 
134.             // Instantiate an Amazon SES client, which will make the service 
135.             // call with the supplied AWS credentials.
136.             AmazonSimpleEmailService client = 
137.                     AmazonSimpleEmailServiceClientBuilder.standard()
138.                     // Replace US_WEST_2 with the AWS Region you're using for
139.                     // Amazon SES.
140.                     .withRegion(Regions.US_WEST_2).build();
141.             
142.             // Print the raw email content on the console
143.             PrintStream out = System.out;
144.             message.writeTo(out);
145. 
146.             // Send the email.
147.             ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
148.             message.writeTo(outputStream);
149.             RawMessage rawMessage = 
150.                     new RawMessage(ByteBuffer.wrap(outputStream.toByteArray()));
151. 
152.             SendRawEmailRequest rawEmailRequest = 
153.                     new SendRawEmailRequest(rawMessage)
154.                         .withConfigurationSetName(CONFIGURATION_SET);
155.             
156.             client.sendRawEmail(rawEmailRequest);
157.             System.out.println("Email sent!");
158.         // Display an error if something goes wrong.
159.         } catch (Exception ex) {
160.           System.out.println("Email Failed");
161.             System.err.println("Error message: " + ex.getMessage());
162.             ex.printStackTrace();
163.         }
164.     }
165. }
```

------
#### [ Python ]

The following code example shows how to use the [Python email\.mime](https://docs.python.org/2/library/email.mime.html) packages and the [AWS SDK for Python \(Boto\)](https://aws.amazon.com/sdk-for-python) to compose and send a raw email\.

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
100.     print(response['ResponseMetadata']['RequestId'])
```

------