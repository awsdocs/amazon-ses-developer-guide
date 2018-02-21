# Send an Email Using SMTP with Java<a name="send-using-smtp-java"></a>

This example uses the [Eclipse IDE](http://www.eclipse.org/) and the [JavaMail API](https://github.com/javaee/javamail/releases) to send email through Amazon SES using the SMTP interface\.

Before you perform the following procedure, complete the setup tasks described in [Before You Begin with Amazon SES](before-you-begin.md) and [Send an Email Through Amazon SES Using SMTP](send-an-email-using-smtp.md)\.

**Important**  
In this getting started tutorial, you send an email to yourself so that you can check to see if you received it\. For further experimentation or load testing, use the Amazon SES mailbox simulator\. Emails that you send to the mailbox simulator do not count toward your sending quota or your bounce and complaint rates\. For more information, see [Testing Amazon SES Email Sending](mailbox-simulator.md)\.

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
   32.     // See http://docs.aws.amazon.com/ses/latest/DeveloperGuide/regions.html#region-endpoints
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

   + `SENDER@EXAMPLE.COM`—Replace with your "From" email address\. You must verify this address before you run this program\. For more information, see [Verifying Identities in Amazon SES](verify-addresses-and-domains.md)\.

   + `RECIPIENT@EXAMPLE.COM`—Replace with your "To" email address\. If your account is still in the sandbox, you must verify this address before you use it\. For more information, see [Moving Out of the Amazon SES Sandbox](request-production-access.md)\.

1. In `AmazonSESSample.java`, replace the following SMTP credentials with the values that you obtained in [Obtaining Your Amazon SES SMTP Credentials](smtp-credentials.md):
**Important**  
Your SMTP credentials are different from your AWS credentials\. For more information about credentials, see [Using Credentials With Amazon SES](using-credentials.md)\.

   + `YOUR_SMTP_USERNAME`—Replace with your SMTP username credential\. Note that your SMTP username credential is a 20\-character string of letters and numbers, not an intelligible name\.

   + `YOUR_SMTP_PASSWORD`—Replace with your SMTP password\.

1. \(Optional\) If you want to use an Amazon SES SMTP endpoint in a Region other than US West \(Oregon\), change the value of the variable `HOST` to the endpoint you want to use\. For a list of Amazon SES endpoints, see [Regions and Amazon SES](regions.md)\.

1. Save `AmazonSESSample.java`\.

1. To build the project, choose **Project** and then choose **Build Project**\. \(If this option is disabled, then you may have automatic building enabled\.\)

1. To start the program and send the email, choose **Run** and then choose **Run** again\.

1. Review the output\. If the email was successfully sent, the console displays "Email sent\!" Otherwise, it displays an error message\.

1. Sign into the email client of the recipient address\. You will find the message that you sent\.