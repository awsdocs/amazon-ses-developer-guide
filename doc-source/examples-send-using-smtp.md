# Sending email using the Amazon SES SMTP Interface<a name="examples-send-using-smtp"></a>

Several programming languages include standard libraries for sending email using SMTP\. You can use these libraries to create email sending applications that are lightweight and highly configurable\.

In this section, you will find code examples in several programming languages that demonstrate the process of sending email through Amazon SES using the SMTP interface\. Wherever possible, these code examples use standard libraries\.

------
#### [ C\# ]

The following code example is a complete solution for sending email through the Amazon SES SMTP interface using C\#\. In order to run this code example, you must obtain SMTP credentials; for more information, see [Obtaining your Amazon SES SMTP credentials](smtp-credentials.md)\.

```
 1. using System;
 2. using System.Net;
 3. using System.Net.Mail;
 4. 
 5. namespace AmazonSESSample
 6. {
 7.     class Program
 8.     {
 9.         static void Main(string[] args)
10.         {
11.             // Replace sender@example.com with your "From" address. 
12.             // This address must be verified with Amazon SES.
13.             String FROM = "sender@example.com";
14.             String FROMNAME = "Sender Name";
15. 
16.             // Replace recipient@example.com with a "To" address. If your account 
17.             // is still in the sandbox, this address must be verified.
18.             String TO = "recipient@amazon.com";
19. 
20.             // Replace smtp_username with your Amazon SES SMTP user name.
21.             String SMTP_USERNAME = "smtp_username";
22. 
23.             // Replace smtp_password with your Amazon SES SMTP password.
24.             String SMTP_PASSWORD = "smtp_password";
25. 
26.             // (Optional) the name of a configuration set to use for this message.
27.             // If you comment out this line, you also need to remove or comment out
28.             // the "X-SES-CONFIGURATION-SET" header below.
29.             String CONFIGSET = "ConfigSet";
30. 
31.             // If you're using Amazon SES in a region other than US West (Oregon), 
32.             // replace email-smtp.us-west-2.amazonaws.com with the Amazon SES SMTP  
33.             // endpoint in the appropriate AWS Region.
34.             String HOST = "email-smtp.us-west-2.amazonaws.com";
35. 
36.             // The port you will connect to on the Amazon SES SMTP endpoint. We
37.             // are choosing port 587 because we will use STARTTLS to encrypt
38.             // the connection.
39.             int PORT = 587;
40. 
41.             // The subject line of the email
42.             String SUBJECT =
43.                 "Amazon SES test (SMTP interface accessed using C#)";
44. 
45.             // The body of the email
46.             String BODY =
47.                 "<h1>Amazon SES Test</h1>" +
48.                 "<p>This email was sent through the " +
49.                 "<a href='https://aws.amazon.com/ses'>Amazon SES</a> SMTP interface " +
50.                 "using the .NET System.Net.Mail library.</p>";
51. 
52.             // Create and build a new MailMessage object
53.             MailMessage message = new MailMessage();
54.             message.IsBodyHtml = true;
55.             message.From = new MailAddress(FROM, FROMNAME);
56.             message.To.Add(new MailAddress(TO));
57.             message.Subject = SUBJECT;
58.             message.Body = BODY;
59.             // Comment or delete the next line if you are not using a configuration set
60.             message.Headers.Add("X-SES-CONFIGURATION-SET", CONFIGSET);
61. 
62.             using (var client = new System.Net.Mail.SmtpClient(HOST, PORT))
63.             {
64.                 // Pass SMTP credentials
65.                 client.Credentials =
66.                     new NetworkCredential(SMTP_USERNAME, SMTP_PASSWORD);
67. 
68.                 // Enable SSL encryption
69.                 client.EnableSsl = true;
70. 
71.                 // Try to send the message. Show status in console.
72.                 try
73.                 {
74.                     Console.WriteLine("Attempting to send email...");
75.                     client.Send(message);
76.                     Console.WriteLine("Email sent!");
77.                 }
78.                 catch (Exception ex)
79.                 {
80.                     Console.WriteLine("The email was not sent.");
81.                     Console.WriteLine("Error message: " + ex.Message);
82.                 }
83.             }
84.         }
85.     }
86. }
```

------
#### [ Go ]

The following code example is a complete solution for sending email through the Amazon SES SMTP interface using the Go programming language\. In order to run this code example, you must obtain SMTP credentials; for more information, see [Obtaining your Amazon SES SMTP credentials](smtp-credentials.md)\. You must also install the [Gomail package](https://github.com/go-gomail/gomail)\.

```
 1. package main
 2. 
 3. import (
 4.     "fmt"
 5.     "gopkg.in/gomail.v2" //go get gopkg.in/gomail.v2
 6. )
 7. 
 8. const (
 9.     // Replace sender@example.com with your "From" address. 
10.     // This address must be verified with Amazon SES.
11.     Sender = "sender@example.com"
12.     SenderName = "Sender Name"
13.     
14.     // Replace recipient@example.com with a "To" address. If your account 
15.     // is still in the sandbox, this address must be verified.
16.     Recipient = "recipient@example.com"
17. 
18.     // Replace SmtpUser with your Amazon SES SMTP user name.
19.     SmtpUser = "SmtpUser"
20.     
21.     // Replace SmtpPass with your Amazon SES SMTP password.
22.     SmtpPass = "SmtpPass"
23.     
24.     // The name of the configuration set to use for this message.
25.     // If you comment out or remove this variable, you will also need to
26.     // comment out or remove the header below.
27.     ConfigSet = "ConfigSet"
28.     
29.     // If you're using Amazon SES in an AWS Region other than US West (Oregon), 
30.     // replace email-smtp.us-west-2.amazonaws.com with the Amazon SES SMTP  
31.     // endpoint in the appropriate region.
32.     Host = "email-smtp.us-west-2.amazonaws.com"
33.     Port = 587 
34.     
35.     // The subject line for the email.
36.     Subject = "Amazon SES Test (Gomail)"
37.     
38.     // The HTML body for the email.
39.     HtmlBody =  "<html><head><title>SES Sample Email</title></head><body>" +
40.                 "<h1>Amazon SES Test Email (Gomail)</h1>" +
41.                 "<p>This email was sent with " +
42.                 "<a href='https://aws.amazon.com/ses/'>Amazon SES</a> using " +
43.                 "the <a href='https://github.com/go-gomail/gomail/'>Gomail " +
44.                 "package</a> for <a href='https://golang.org/'>Go</a>.</p>" +
45.                 "</body></html>"
46.     
47.     //The email body for recipients with non-HTML email clients.
48.     TextBody = "This email was sent with Amazon SES using the Gomail package."
49.     
50.     // The tags to apply to this message. Separate multiple key-value pairs
51.     // with commas.
52.     // If you comment out or remove this variable, you will also need to
53.     // comment out or remove the header on line 80.
54.     Tags = "genre=test,genre2=test2"
55.     
56.     // The character encoding for the email.
57.     CharSet = "UTF-8"
58. 
59. )
60. 
61. func main() {
62.     
63.     // Create a new message.
64.     m := gomail.NewMessage()
65.     
66.     // Set the main email part to use HTML.
67.     m.SetBody("text/html", HtmlBody)
68.     
69.     // Set the alternative part to plain text.
70.     m.AddAlternative("text/plain", TextBody)
71. 
72.     // Construct the message headers, including a Configuration Set and a Tag.
73.     m.SetHeaders(map[string][]string{
74.         "From": {m.FormatAddress(Sender,SenderName)},
75.         "To": {Recipient},
76.         "Subject": {Subject},
77.         // Comment or remove the next line if you are not using a configuration set
78.         "X-SES-CONFIGURATION-SET": {ConfigSet},
79.         // Comment or remove the next line if you are not using custom tags
80.         "X-SES-MESSAGE-TAGS": {Tags},
81.     })
82.     
83.     // Send the email.
84.     d := gomail.NewPlainDialer(Host, Port, SmtpUser, SmtpPass)
85.     
86.     // Display an error message if something goes wrong; otherwise, 
87.     // display a message confirming that the message was sent.
88.     if err := d.DialAndSend(m); err != nil {
89.         fmt.Println(err)
90.     } else {
91.         fmt.Println("Email sent!")
92.     }
93. }
```

------
#### [ Java ]

The following code example is a complete solution for sending email through the Amazon SES SMTP interface using Java\. In order to run this code example, you must obtain SMTP credentials; for more information, see [Obtaining your Amazon SES SMTP credentials](smtp-credentials.md)\. You must also download the [JavaMail API](https://github.com/javaee/javamail/releases)\. 

```
 1. import java.util.Properties;
 2. 
 3. import javax.mail.Message;
 4. import javax.mail.Session;
 5. import javax.mail.Transport;
 6. import javax.mail.internet.InternetAddress;
 7. import javax.mail.internet.MimeMessage;
 8. 
 9. public class AmazonSESSample {
10. 
11.     // Replace sender@example.com with your "From" address.
12.     // This address must be verified.
13.     static final String FROM = "sender@example.com";
14.     static final String FROMNAME = "Sender Name";
15. 	
16.     // Replace recipient@example.com with a "To" address. If your account 
17.     // is still in the sandbox, this address must be verified.
18.     static final String TO = "recipient@example.com";
19.     
20.     // Replace smtp_username with your Amazon SES SMTP user name.
21.     static final String SMTP_USERNAME = "smtp_username";
22.     
23.     // Replace smtp_password with your Amazon SES SMTP password.
24.     static final String SMTP_PASSWORD = "smtp_password";
25.     
26.     // The name of the Configuration Set to use for this message.
27.     // If you comment out or remove this variable, you will also need to
28.     // comment out or remove the header below.
29.     static final String CONFIGSET = "ConfigSet";
30.     
31.     // Amazon SES SMTP host name. This example uses the US West (Oregon) region.
32.     // See https://docs.aws.amazon.com/ses/latest/DeveloperGuide/regions.html#region-endpoints
33.     // for more information.
34.     static final String HOST = "email-smtp.us-west-2.amazonaws.com";
35.     
36.     // The port you will connect to on the Amazon SES SMTP endpoint. 
37.     static final int PORT = 587;
38.     
39.     static final String SUBJECT = "Amazon SES test (SMTP interface accessed using Java)";
40.     
41.     static final String BODY = String.join(
42.     	    System.getProperty("line.separator"),
43.     	    "<h1>Amazon SES SMTP Email Test</h1>",
44.     	    "<p>This email was sent with Amazon SES using the ", 
45.     	    "<a href='https://github.com/javaee/javamail'>Javamail Package</a>",
46.     	    " for <a href='https://www.java.com'>Java</a>."
47.     	);
48. 
49.     public static void main(String[] args) throws Exception {
50. 
51.         // Create a Properties object to contain connection configuration information.
52.     	Properties props = System.getProperties();
53.     	props.put("mail.transport.protocol", "smtp");
54.     	props.put("mail.smtp.port", PORT); 
55.     	props.put("mail.smtp.starttls.enable", "true");
56.     	props.put("mail.smtp.auth", "true");
57. 
58.         // Create a Session object to represent a mail session with the specified properties. 
59.     	Session session = Session.getDefaultInstance(props);
60. 
61.         // Create a message with the specified information. 
62.         MimeMessage msg = new MimeMessage(session);
63.         msg.setFrom(new InternetAddress(FROM,FROMNAME));
64.         msg.setRecipient(Message.RecipientType.TO, new InternetAddress(TO));
65.         msg.setSubject(SUBJECT);
66.         msg.setContent(BODY,"text/html");
67.         
68.         // Add a configuration set header. Comment or delete the 
69.         // next line if you are not using a configuration set
70.         msg.setHeader("X-SES-CONFIGURATION-SET", CONFIGSET);
71.             
72.         // Create a transport.
73.         Transport transport = session.getTransport();
74.                     
75.         // Send the message.
76.         try
77.         {
78.             System.out.println("Sending...");
79.             
80.             // Connect to Amazon SES using the SMTP username and password you specified above.
81.             transport.connect(HOST, SMTP_USERNAME, SMTP_PASSWORD);
82.         	
83.             // Send the email.
84.             transport.sendMessage(msg, msg.getAllRecipients());
85.             System.out.println("Email sent!");
86.         }
87.         catch (Exception ex) {
88.             System.out.println("The email was not sent.");
89.             System.out.println("Error message: " + ex.getMessage());
90.         }
91.         finally
92.         {
93.             // Close and terminate the connection.
94.             transport.close();
95.         }
96.     }
97. }
```

------
#### [ JavaScript ]

The following code example is a complete solution for sending email through the Amazon SES SMTP interface by using the [NodeMailer](https://nodemailer.com) module in Node\.js\. 

In order to run this code example, you first have to obtain SMTP credentials\. For more information, see [Obtaining your Amazon SES SMTP credentials](smtp-credentials.md)\. You must also install the [NodeMailer](https://nodemailer.com) module\. 

```
/*
This code uses callbacks to handle asynchronous function responses.
It currently demonstrates using an async-await pattern.
AWS supports both the async-await and promises patterns.
For more information, see the following:
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises
https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/calling-services-asynchronously.html
https://docs.aws.amazon.com/lambda/latest/dg/nodejs-prog-model-handler.html
*/

"use strict";
const nodemailer = require("nodemailer");

// If you're using Amazon SES in a region other than US West (Oregon),
// replace email-smtp.us-west-2.amazonaws.com with the Amazon SES SMTP
// endpoint in the appropriate AWS Region.
const smtpEndpoint = "email-smtp.us-west-2.amazonaws.com";

// The port to use when connecting to the SMTP server.
const port = 587;

// Replace sender@example.com with your "From" address.
// This address must be verified with Amazon SES.
const senderAddress = "Mary Major <sender@example.com>";

// Replace recipient@example.com with a "To" address. If your account
// is still in the sandbox, this address must be verified. To specify
// multiple addresses, separate each address with a comma.
var toAddresses = "recipient@example.com";

// CC and BCC addresses. If your account is in the sandbox, these
// addresses have to be verified. To specify multiple addresses, separate
// each address with a comma.
var ccAddresses = "cc-recipient0@example.com,cc-recipient1@example.com";
var bccAddresses = "bcc-recipient@example.com";

// Replace smtp_username with your Amazon SES SMTP user name.
const smtpUsername = "AKIAIOSFODNN7EXAMPLE";

// Replace smtp_password with your Amazon SES SMTP password.
const smtpPassword = "wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY";

// (Optional) the name of a configuration set to use for this message.
var configurationSet = "ConfigSet";

// The subject line of the email
var subject = "Amazon SES test (Nodemailer)";

// The email body for recipients with non-HTML email clients.
var body_text = `Amazon SES Test (Nodemailer)
---------------------------------
This email was sent through the Amazon SES SMTP interface using Nodemailer.
`;

// The body of the email for recipients whose email clients support HTML content.
var body_html = `<html>
<head></head>
<body>
  <h1>Amazon SES Test (Nodemailer)</h1>
  <p>This email was sent with <a href='https://aws.amazon.com/ses/'>Amazon SES</a>
        using <a href='https://nodemailer.com'>Nodemailer</a> for Node.js.</p>
</body>
</html>`;

// The message tags that you want to apply to the email.
var tag0 = "key0=value0";
var tag1 = "key1=value1";

async function main(){

  // Create the SMTP transport.
  let transporter = nodemailer.createTransport({
    host: smtpEndpoint,
    port: port,
    secure: false, // true for 465, false for other ports
    auth: {
      user: smtpUsername,
      pass: smtpPassword
    }
  });

  // Specify the fields in the email.
  let mailOptions = {
    from: senderAddress,
    to: toAddresses,
    subject: subject,
    cc: ccAddresses,
    bcc: bccAddresses,
    text: body_text,
    html: body_html,
    // Custom headers for configuration set and message tags.
    headers: {
      'X-SES-CONFIGURATION-SET': configurationSet,
      'X-SES-MESSAGE-TAGS': tag0,
      'X-SES-MESSAGE-TAGS': tag1
    }
  };

  // Send the email.
  let info = await transporter.sendMail(mailOptions)

  console.log("Message sent! Message ID: ", info.messageId);
}

main().catch(console.error);
```

------
#### [ Perl ]

The following code example is a complete solution for sending email through the Amazon SES SMTP interface using Perl\. In order to run this code example, you must obtain SMTP credentials; for more information, see [Obtaining your Amazon SES SMTP credentials](smtp-credentials.md)\. You must also install the [Email::Sender](https://metacpan.org/pod/Email::Sender), [Email::MIME](https://metacpan.org/pod/Email::MIME), and [Try::Tiny](https://metacpan.org/pod/Try::Tiny) modules from [CPAN](https://www.cpan.org/)\.

```
 1. #!/usr/bin/perl 
 2. use warnings;
 3. use strict;
 4. use Email::Sender::Simple qw(sendmail);
 5. use Email::Sender::Transport::SMTP;
 6. use Email::MIME;
 7. use Try::Tiny;
 8. 
 9. # Replace sender@example.com with your "From" address. 
10. # This address must be verified.
11. my $sender = 'Sender name <sender@example.com>';
12. 
13. # Replace recipient@example.com with a "To" address. If your account 
14. # is still in the sandbox, this address must be verified.
15. my $recipient = 'recipient@example.com';
16. 
17. # Replace smtp_username with your Amazon SES SMTP user name.
18. my $smtp_username = "smtp_username";
19. 
20. # Replace smtp_password with your Amazon SES SMTP password.
21. my $smtp_password = "smtp_password";
22. 
23. # (Optional) the name of a configuration set to use for this message.
24. # If you comment out this line, you also need to remove or comment out
25. # the "X-SES-CONFIGURATION-SET:" header below.
26. my $configset = "ConfigSet";
27. 
28. # If you're using Amazon SES in an AWS Region other than US West (Oregon), 
29. # replace email-smtp.us-west-2.amazonaws.com with the Amazon SES SMTP  
30. # endpoint in the appropriate region.
31. my $host = "email-smtp.us-west-2.amazonaws.com";
32. my $port = 587;
33. 
34. # The subject line of the email.
35. my $subject = "Amazon SES Test (Perl)";
36. 
37. # The HTML body for the email.
38. my $htmlbody = <<'END_HTML';
39. <html>
40.   <head></head>
41.   <body>
42.     <h1>Amazon SES SMTP Email Test</h1>
43.     <p>This email was sent with Amazon SES using the 
44.       <a href='https://www.perl.org/'>Perl</a>
45.       <a href='http://search.cpan.org/~rjbs/Email-Sender-1.300031/'>
46.         Email::Sender</a> library.</p>
47.   </body>
48. </html>
49. END_HTML
50. 
51. # The email body for recipients with non-HTML email clients.
52. my $textbody = "Amazon SES Test\r\n"
53.              . "This message was sent with Amazon SES using the Perl "
54.              . "Email::Sender module.";
55. 
56. # Create the SMTP transport.
57. my $transport = Email::Sender::Transport::SMTP->new(
58.   host          => "$host",
59.   port          => "$port",
60.   ssl           => 'starttls',
61.   sasl_username => "$smtp_username",
62.   sasl_password => "$smtp_password",
63. );
64. 
65. # Build a multipart MIME message with an HTML part and a text part.
66. my $message = Email::MIME->create(
67.     attributes  => {
68.         content_type => 'multipart/alternative',
69.         charset      => 'UTF-8',
70.     },
71.     header_str  => [
72.         From    => "$sender",
73.         To      => "$recipient",
74.         Subject => "$subject",
75.     ],
76.     parts => [
77.         Email::MIME->create(
78.             attributes => { content_type => 'text/plain' },
79.             body       => "$textbody",
80.         ),
81.         Email::MIME->create(
82.             attributes => { content_type => 'text/html' },
83.             body       => "$htmlbody",
84.         )
85.     ],
86. );
87. 
88. # Add the configuration set header to the MIME message.
89. $message->header_str_set( 'X-SES-CONFIGURATION-SET' => "$configset" );
90. 
91. # Try to send the email using the sendmail function from 
92. # Email::Sender::Simple.
93. try {
94.   sendmail($message, { transport => $transport });
95. # If something goes wrong, print an error message.
96. } catch {
97.   die "Error sending email: $_";
98. };
```

------
#### [ PHP ]

The following code example is a complete solution for sending email through the Amazon SES SMTP interface using PHP\. In order to run this code example, you must obtain SMTP credentials; for more information, see [Obtaining your Amazon SES SMTP credentials](smtp-credentials.md)\. You must also install the [PHPMailer package](https://packagist.org/packages/phpmailer/phpmailer) using [Composer](https://getcomposer.org/)\.

```
 1. <?php
 2. 
 3. // Import PHPMailer classes into the global namespace
 4. // These must be at the top of your script, not inside a function
 5. use PHPMailer\PHPMailer\PHPMailer;
 6. use PHPMailer\PHPMailer\Exception;
 7. 
 8. // If necessary, modify the path in the require statement below to refer to the
 9. // location of your Composer autoload.php file.
10. require 'vendor/autoload.php';
11. 
12. // Replace sender@example.com with your "From" address.
13. // This address must be verified with Amazon SES.
14. $sender = 'sender@example.com';
15. $senderName = 'Sender Name';
16. 
17. // Replace recipient@example.com with a "To" address. If your account
18. // is still in the sandbox, this address must be verified.
19. $recipient = 'recipient@example.com';
20. 
21. // Replace smtp_username with your Amazon SES SMTP user name.
22. $usernameSmtp = 'smtp_username';
23. 
24. // Replace smtp_password with your Amazon SES SMTP password.
25. $passwordSmtp = 'smtp_password';
26. 
27. // Specify a configuration set. If you do not want to use a configuration
28. // set, comment or remove the next line.
29. $configurationSet = 'ConfigSet';
30. 
31. // If you're using Amazon SES in a region other than US West (Oregon),
32. // replace email-smtp.us-west-2.amazonaws.com with the Amazon SES SMTP
33. // endpoint in the appropriate region.
34. $host = 'email-smtp.us-west-2.amazonaws.com';
35. $port = 587;
36. 
37. // The subject line of the email
38. $subject = 'Amazon SES test (SMTP interface accessed using PHP)';
39. 
40. // The plain-text body of the email
41. $bodyText =  "Email Test\r\nThis email was sent through the
42.     Amazon SES SMTP interface using the PHPMailer class.";
43. 
44. // The HTML-formatted body of the email
45. $bodyHtml = '<h1>Email Test</h1>
46.     <p>This email was sent through the
47.     <a href="https://aws.amazon.com/ses">Amazon SES</a> SMTP
48.     interface using the <a href="https://github.com/PHPMailer/PHPMailer">
49.     PHPMailer</a> class.</p>';
50. 
51. $mail = new PHPMailer(true);
52. 
53. try {
54.     // Specify the SMTP settings.
55.     $mail->isSMTP();
56.     $mail->setFrom($sender, $senderName);
57.     $mail->Username   = $usernameSmtp;
58.     $mail->Password   = $passwordSmtp;
59.     $mail->Host       = $host;
60.     $mail->Port       = $port;
61.     $mail->SMTPAuth   = true;
62.     $mail->SMTPSecure = 'tls';
63.     $mail->addCustomHeader('X-SES-CONFIGURATION-SET', $configurationSet);
64. 
65.     // Specify the message recipients.
66.     $mail->addAddress($recipient);
67.     // You can also add CC, BCC, and additional To recipients here.
68. 
69.     // Specify the content of the message.
70.     $mail->isHTML(true);
71.     $mail->Subject    = $subject;
72.     $mail->Body       = $bodyHtml;
73.     $mail->AltBody    = $bodyText;
74.     $mail->Send();
75.     echo "Email sent!" , PHP_EOL;
76. } catch (phpmailerException $e) {
77.     echo "An error occurred. {$e->errorMessage()}", PHP_EOL; //Catch errors from PHPMailer.
78. } catch (Exception $e) {
79.     echo "Email not sent. {$mail->ErrorInfo}", PHP_EOL; //Catch errors from Amazon SES.
80. }
81. 
82. ?>
```

------
#### [ Python ]

The following code example is a complete solution for sending email through the Amazon SES SMTP interface using Python\. In order to run this code example, you must obtain SMTP credentials; for more information, see [Obtaining your Amazon SES SMTP credentials](smtp-credentials.md)\.

```
 1. import smtplib  
 2. import email.utils
 3. from email.mime.multipart import MIMEMultipart
 4. from email.mime.text import MIMEText
 5. 
 6. # Replace sender@example.com with your "From" address. 
 7. # This address must be verified.
 8. SENDER = 'sender@example.com'  
 9. SENDERNAME = 'Sender Name'
10. 
11. # Replace recipient@example.com with a "To" address. If your account 
12. # is still in the sandbox, this address must be verified.
13. RECIPIENT  = 'recipient@example.com'
14. 
15. # Replace smtp_username with your Amazon SES SMTP user name.
16. USERNAME_SMTP = "smtp_username"
17. 
18. # Replace smtp_password with your Amazon SES SMTP password.
19. PASSWORD_SMTP = "smtp_password"
20. 
21. # (Optional) the name of a configuration set to use for this message.
22. # If you comment out this line, you also need to remove or comment out
23. # the "X-SES-CONFIGURATION-SET:" header below.
24. CONFIGURATION_SET = "ConfigSet"
25. 
26. # If you're using Amazon SES in an AWS Region other than US West (Oregon), 
27. # replace email-smtp.us-west-2.amazonaws.com with the Amazon SES SMTP  
28. # endpoint in the appropriate region.
29. HOST = "email-smtp.us-west-2.amazonaws.com"
30. PORT = 587
31. 
32. # The subject line of the email.
33. SUBJECT = 'Amazon SES Test (Python smtplib)'
34. 
35. # The email body for recipients with non-HTML email clients.
36. BODY_TEXT = ("Amazon SES Test\r\n"
37.              "This email was sent through the Amazon SES SMTP "
38.              "Interface using the Python smtplib package."
39.             )
40. 
41. # The HTML body of the email.
42. BODY_HTML = """<html>
43. <head></head>
44. <body>
45.   <h1>Amazon SES SMTP Email Test</h1>
46.   <p>This email was sent with Amazon SES using the
47.     <a href='https://www.python.org/'>Python</a>
48.     <a href='https://docs.python.org/3/library/smtplib.html'>
49.     smtplib</a> library.</p>
50. </body>
51. </html>
52.             """
53. 
54. # Create message container - the correct MIME type is multipart/alternative.
55. msg = MIMEMultipart('alternative')
56. msg['Subject'] = SUBJECT
57. msg['From'] = email.utils.formataddr((SENDERNAME, SENDER))
58. msg['To'] = RECIPIENT
59. # Comment or delete the next line if you are not using a configuration set
60. msg.add_header('X-SES-CONFIGURATION-SET',CONFIGURATION_SET)
61. 
62. # Record the MIME types of both parts - text/plain and text/html.
63. part1 = MIMEText(BODY_TEXT, 'plain')
64. part2 = MIMEText(BODY_HTML, 'html')
65. 
66. # Attach parts into message container.
67. # According to RFC 2046, the last part of a multipart message, in this case
68. # the HTML message, is best and preferred.
69. msg.attach(part1)
70. msg.attach(part2)
71. 
72. # Try to send the message.
73. try:  
74.     server = smtplib.SMTP(HOST, PORT)
75.     server.ehlo()
76.     server.starttls()
77.     #stmplib docs recommend calling ehlo() before & after starttls()
78.     server.ehlo()
79.     server.login(USERNAME_SMTP, PASSWORD_SMTP)
80.     server.sendmail(SENDER, RECIPIENT, msg.as_string())
81.     server.close()
82. # Display an error message if something goes wrong.
83. except Exception as e:
84.     print ("Error: ", e)
85. else:
86.     print ("Email sent!")
```

------
#### [ Ruby ]

The following code example is a complete solution for sending email through the Amazon SES SMTP interface using Ruby\. In order to run this code example, you must obtain SMTP credentials; for more information, see [Obtaining your Amazon SES SMTP credentials](smtp-credentials.md)\.

```
 1. require 'net/smtp'
 2. 
 3. # Replace sender@example.com with your "From" address.
 4. # This address must be verified with Amazon SES.
 5. sender = "sender@example.com"
 6. senderName = "Sender Name"
 7. 
 8. # Replace recipient@example.com with a "To" address. If your account 
 9. # is still in the sandbox, this address must be verified.
10. recipient = "recipient@example.com"
11. 
12. # Replace smtp_username with your Amazon SES SMTP user name.
13. smtp_username = "smtp_username"
14. 
15. # Replace smtp_password with your Amazon SES SMTP password.
16. smtp_password = "smtp_password"
17. 
18. # (Optional) the name of a configuration set to use for this message.
19. # If you comment out this line, you also need to remove or comment out
20. # the "X-SES-CONFIGURATION-SET" header below.
21. configSet = "ConfigSet"
22. 
23. # If you're using Amazon SES in an AWS Region other than US West (Oregon), 
24. # replace email-smtp.us-west-2.amazonaws.com with the Amazon SES SMTP  
25. # endpoint in the appropriate region.
26. server = "email-smtp.us-west-2.amazonaws.com"
27. port = 587 
28. 
29. # The subject line of the email.
30. subject = "Amazon SES Test (Ruby Net::SMTP library)"
31. 
32. # Specify the headers and body of the message as a variable.
33. message = [
34.     #Remove the next line if you are not using a configuration set
35.     "X-SES-CONFIGURATION-SET: #{configSet}",
36.     "Content-Type: text/html; charset=UTF-8",
37.     "Content-Transfer-Encoding: 7bit",
38.     "From: #{senderName} <#{sender}>",
39.     "To: #{recipient}",
40.     "Subject: #{subject}",
41.     "",
42.     "<h1>Amazon SES Test (Ruby Net::SMTP library)</h1>",
43.     "<p>This email was sent with \
44.     <a href='https://aws.amazon.com/ses/'>\
45.     Amazon SES</a> using the Ruby Net::SMTP library.</p>"
46.     ].join("\n")
47. 			
48. # Create a new SMTP object called "smtp."
49. smtp = Net::SMTP.new(server, port)
50. 
51. # Tell the smtp object to connect using TLS.
52. smtp.enable_starttls
53. 
54. # Open an SMTP session and log in to the server using SMTP authentication.
55. smtp.start(server,smtp_username,smtp_password, :login)
56. 
57. # Try to send the message.
58. begin
59.   smtp.send_message(message, sender, recipient)
60.   puts "Email sent!"
61. # Print an error message if something goes wrong.
62. rescue => e
63.   puts e
64. end
```

------


****  

|  | 
| --- |
| For information and discussions about a variety of topics related to Amazon SES, see the [AWS Messaging and Targeting Blog](https://aws.amazon.com//blogs/messaging-and-targeting/)\. To browse and post questions, go to the [Amazon SES Forum](https://forums.aws.amazon.com/forum.jspa?forumID=90)\. | 