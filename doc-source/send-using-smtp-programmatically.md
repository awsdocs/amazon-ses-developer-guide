# Sending emails programmatically through the Amazon SES SMTP interface<a name="send-using-smtp-programmatically"></a>

To send an email using the Amazon SES SMTP interface, you can use an SMTP\-enabled programming language, email server, or application\. Before you start, complete the tasks in [Setting up Amazon Simple Email Service](setting-up.md)\. You also need to get the following information: 
+ Your Amazon SES SMTP user name and password, which enable you to connect to the Amazon SES SMTP endpoint\. To get your Amazon SES SMTP user name and password, see [Obtaining Amazon SES SMTP credentials](smtp-credentials.md)\. 
**Important**  
Your SMTP credentials are different from your AWS credentials\. For more information about credentials, see [Types of Amazon SES credentials](send-email-concepts-credentials.md)\.
+ The SMTP endpoint address\. For a list of Amazon SES SMTP endpoints, see [Connecting to an Amazon SES SMTP endpoint](smtp-connect.md)\.
+ The Amazon SES SMTP interface port number, which depends on the connection method\. For more information, see [Connecting to an Amazon SES SMTP endpoint](smtp-connect.md)\.

## Code examples<a name="send-email-smtp-code-examples"></a>

You can access the Amazon SES SMTP interface by using an SMTP\-enabled programming language\. You provide the Amazon SES SMTP hostname and port number along with your SMTP credentials and then use the programming language's generic SMTP functions to send the email\.

Amazon Elastic Compute Cloud \(Amazon EC2\) restricts email traffic over port 25 by default\. To avoid timeouts when sending email through the SMTP endpoint from Amazon EC2, you can request that these restrictions be removed\. For more information, see [How do I remove the restriction on port 25 from my Amazon EC2 instance or AWS Lambda function?](https://aws.amazon.com/premiumsupport/knowledge-center/ec2-port-25-throttle/) in the AWS Knowledge Center\.

The code examples in this section for C\#, Java, and PHP, use port 587 to avoid this issue\. 

**Note**  
In these tutorials, you send an email to yourself so that you can check to see if you received it\. For further experimentation or load testing, use the Amazon SES mailbox simulator\. Emails that you send to the mailbox simulator do not count toward your sending quota or your bounce and complaint rates\. For more information, see [Using the mailbox simulator manually](send-an-email-from-console.md#send-email-simulator)\.

**Select a programing language to view the example for that language:**

------
#### [ C\# ]

The following procedure shows how to use [Microsoft Visual Studio](https://www.visualstudio.com/) to create a C\# console application that sends an email through Amazon SES\. The procedures in this section apply to Visual Studio 2017, but the process of creating C\# console applications is similar across Microsoft Visual Studio editions\.

Before you perform the following procedure, complete the tasks in [Setting up Amazon Simple Email Service](setting-up.md)\.

**To send an email using the Amazon SES SMTP interface with C\#**

1. Create a console project in Visual Studio by performing the following steps:

   1. Open Microsoft Visual Studio\.

   1. On the **File** menu, choose **New**, **Project**\.

   1. On the **New Project** window, in the left pane, expand **Installed**, expand **Templates**, and then expand **Visual C\#**\.

   1. Under **Visual C\#**, choose **Windows Classic Desktop**\.

   1. On the menu at the top of the window, choose **\.NET Framework 4\.5**, as shown in the following image\.  
![\[The .NET Framework selection menu on the New Project window.\]](http://docs.aws.amazon.com/ses/latest/dg/images/getting_started_dotnet_smtp_new_project.png)
**Note**  
You can choose a later version of the \.NET Framework if necessary\.

   1. Choose **Console App \(\.NET Framework\)**\.

   1. In the **Name** field, type `AmazonSESSample`\.

   1. Choose **OK**\.

1. In your Visual Studio project, replace the entire contents of `Program.cs` with the following code:

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

1. In `Program.cs`, replace the following email addresses with your own values:
**Important**  
The email addresses are case\-sensitive\. Make sure that the addresses are exactly the same as the ones you verified\.
   + `SENDER@EXAMPLE.COM`—Replace with your "From" email address\. You must verify this address before you run this program\. For more information, see [Verified identities in Amazon SES](verify-addresses-and-domains.md)\.
   + `RECIPIENT@EXAMPLE.COM`—Replace with your "To" email address\. If your account is still in the sandbox, you must verify this address before you use it\. For more information, see [Moving out of the Amazon SES sandbox](request-production-access.md)\.

1. In `Program.cs`, replace the following SMTP credentials with the values that you obtained in [Obtaining Amazon SES SMTP credentials](smtp-credentials.md):
**Important**  
Your SMTP credentials are different from your AWS credentials\. For more information about credentials, see [Types of Amazon SES credentials](send-email-concepts-credentials.md)\.
   + YOUR\_SMTP\_USERNAME—Replace with your SMTP user name\. Note that your SMTP user name credential is a 20\-character string of letters and numbers, not an intelligible name\.
   + YOUR\_SMTP\_PASSWORD—Replace with your SMTP password\.

1. \(Optional\) If you want to use an Amazon SES SMTP endpoint in a Region other than US West \(Oregon\), change the value of the variable `HOST` to the endpoint you want to use\. For a list of SMTP endpoint URLs for the AWS Regions where Amazon SES is available, see [Amazon Simple Email Service \(Amazon SES\)](https://docs.aws.amazon.com/general/latest/gr/rande.html#ses_region) in the *AWS General Reference*\.

1. \(Optional\) If you want to use a configuration set when sending this email, change the value of the variable `CONFIGSET` to the name of the configuration set\. For more information about configuration sets, see [Using configuration sets in Amazon SES](using-configuration-sets.md)\.

1. Save `Program.cs`\.

1. To build the project, choose **Build** and then choose **Build Solution**\.

1. To run the program, choose **Debug** and then choose **Start Debugging**\.

1. Review the output\. If the email was successfully sent, the console displays "Email sent\!" Otherwise, it displays an error message\.

1. Sign in to the email client of the recipient address\. You will see the message that you sent\.

------
#### [ Java ]

This example uses the [Eclipse IDE](http://www.eclipse.org/) and the [JavaMail API](https://github.com/javaee/javamail/releases) to send email through Amazon SES using the SMTP interface\.

Before you perform the following procedure, complete the tasks in [Setting up Amazon Simple Email Service](setting-up.md)\.

**To send an email using the Amazon SES SMTP interface with Java**

1. In a web browser, go to the [JavaMail Github page](https://github.com/javaee/javamail/releases)\. Under **Downloads**, choose **javax\.mail\.jar** to download the latest version of JavaMail\.
**Important**  
This tutorial requires JavaMail version 1\.5 or later\. These procedures were tested using JavaMail version 1\.6\.1\.

1. Create a project in Eclipse by performing the following steps:

   1. Start Eclipse\.

   1. In Eclipse, choose **File**, choose **New**, and then choose **Java Project**\.

   1. In the **Create a Java Project** dialog box, type a project name and then choose **Next\.**

   1. In the **Java Settings** dialog box, choose the **Libraries** tab\.

   1. Choose **Add External JARs**\.

   1. Browse to the folder in which you downloaded JavaMail\. Choose the file `javax.mail.jar`, and then choose **Open**\.

   1. In the **Java Settings** dialog box, choose **Finish**\.

1. In Eclipse, in the **Package Explorer** window, expand your project\.

1. Under your project, right\-click the **src** directory, choose **New**, and then choose **Class**\.

1. In the **New Java Class** dialog box, in the **Name** field, type `AmazonSESSample` and then choose **Finish**\.

1. Replace the entire contents of `AmazonSESSample.java` with the following code:

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

1. In `AmazonSESSample.java`, replace the following email addresses with your own values:
**Important**  
The email addresses are case\-sensitive\. Make sure that the addresses are exactly the same as the ones you verified\.
   + `SENDER@EXAMPLE.COM`—Replace with your "From" email address\. You must verify this address before you run this program\. For more information, see [Verified identities in Amazon SES](verify-addresses-and-domains.md)\.
   + `RECIPIENT@EXAMPLE.COM`—Replace with your "To" email address\. If your account is still in the sandbox, you must verify this address before you use it\. For more information, see [Moving out of the Amazon SES sandbox](request-production-access.md)\.

1. In `AmazonSESSample.java`, replace the following SMTP credentials with the values that you obtained in [Obtaining Amazon SES SMTP credentials](smtp-credentials.md):
**Important**  
Your SMTP credentials are different from your AWS credentials\. For more information about credentials, see [Types of Amazon SES credentials](send-email-concepts-credentials.md)\.
   + `YOUR_SMTP_USERNAME`—Replace with your SMTP user name credential\. Note that your SMTP user name credential is a 20\-character string of letters and numbers, not an intelligible name\.
   + `YOUR_SMTP_PASSWORD`—Replace with your SMTP password\.

1. \(Optional\) If you want to use an Amazon SES SMTP endpoint in an AWS Region other than US West \(Oregon\), change the value of the variable `HOST` to the endpoint you want to use\. For a list of regions where Amazon SES is available, see [Amazon Simple Email Service \(Amazon SES\)](https://docs.aws.amazon.com/general/latest/gr/rande.html#ses_region) in the *AWS General Reference*\.

1. \(Optional\) If you want to use a configuration set when sending this email, change the value of the variable `CONFIGSET` to the name of the configuration set\. For more information about configuration sets, see [Using configuration sets in Amazon SES](using-configuration-sets.md)\.

1. Save `AmazonSESSample.java`\.

1. To build the project, choose **Project** and then choose **Build Project**\. \(If this option is disabled, then you may have automatic building enabled\.\)

1. To start the program and send the email, choose **Run** and then choose **Run** again\.

1. Review the output\. If the email was successfully sent, the console displays "Email sent\!" Otherwise, it displays an error message\.

1. Sign into the email client of the recipient address\. You will see the message that you sent\.

------
#### [ PHP  ]

This example uses the PHPMailer class to send email through Amazon SES using the SMTP interface\. 

Before you perform the following procedure you must complete the tasks in [Setting up Amazon Simple Email Service](setting-up.md)\. In addition to setting up Amazon SES you must complete the following prerequisites to sending email with PHP:

**Prerequisites:**
+ **Install PHP**—PHP is available at [http://php\.net/downloads\.php](https://php.net/downloads.php)\. After you install PHP, add the path to PHP in your environment variables so that you can run PHP from any command prompt\.
+ **Install the Composer dependency manager**—After you install the Composer dependency manager, you can download and install the PHPMailer class and its dependencies\. To install Composer, follow the installation instructions at [https://getcomposer\.org/download](https://getcomposer.org/download)\.
+ **Install the PHPMailer class**— After you install Composer, run the following command to install PHPMailer: 

  ```
  path/to/composer require phpmailer/phpmailer
  ```

  In the preceding command, replace *path/to/* with the path where you installed Composer\.

**To send an email using the Amazon SES SMTP interface with PHP**

1. Create a file named `amazon-ses-smtp-sample.php`\. Open the file with a text editor and paste in the following code:

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

1. In `amazon-ses-smtp-sample.php`, replace the following with your own values:
   + **`sender@example.com`**—Replace with an email address that you have verified with Amazon SES\. For more information, see [Verified identities](verify-addresses-and-domains.md)\. Email addresses in Amazon SES are case\-sensitive\. Make sure that the address you enter is exactly the same as the one you verified\.
   + **`recipient@example.com`**—Replace with the address of the recipient\. If your account is still in the sandbox, you must verify this address before you use it\. For more information, see [Moving out of the Amazon SES sandbox](request-production-access.md)\. Make sure that the address you enter is exactly the same as the one you verified\.
   + **`smtp_username`**—Replace with your SMTP user name credential, which you obtained from the [SMTP Settings](https://console.aws.amazon.com/ses/home?#smtp-settings:) page of the Amazon SES console\. This is **not** the same as your AWS access key ID\. Note that your SMTP user name credential is a 20\-character string of letters and numbers, not an intelligible name\.
   + **`smtp_password`**—Replace with your SMTP password, which you obtained from the [SMTP Settings](https://console.aws.amazon.com/ses/home?#smtp-settings:) page of the Amazon SES console\. This is **not** the same as your AWS secret access key\.
   + **\(Optional\) `ConfigSet`**—If you want to use a configuration set when sending this email, replace this value with the name of the configuration set\. For more information about configuration sets, see [Using configuration sets in Amazon SES](using-configuration-sets.md)\.
   + **\(Optional\) `email-smtp.us-west-2.amazonaws.com`**—If you want to use an Amazon SES SMTP endpoint in a Region other than US West \(Oregon\), replace this with the Amazon SES SMTP endpoint in the Region you want to use\. For a list of SMTP endpoint URLs for the AWS Regions where Amazon SES is available, see [Amazon Simple Email Service \(Amazon SES\)](https://docs.aws.amazon.com/general/latest/gr/rande.html#ses_region) in the *AWS General Reference*\.

1. Save `amazon-ses-smtp-sample.php`\.

1. To run the program, open a command prompt in the same directory as `amazon-ses-smtp-sample.php`, and then type php amazon\-ses\-smtp\-sample\.php\.

1. Review the output\. If the email was successfully sent, the console displays "Email sent\!" Otherwise, it displays an error message\.

1. Sign in to the email client of the recipient address\. You will see the message that you sent\.

------