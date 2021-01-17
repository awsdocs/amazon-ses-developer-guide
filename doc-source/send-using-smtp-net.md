# Send an email using SMTP with C\#<a name="send-using-smtp-net"></a>

The following procedure shows how to use [Microsoft Visual Studio](https://www.visualstudio.com/) to create a C\# console application that sends an email through Amazon SES\. The procedures in this section apply to Visual Studio 2017, but the process of creating C\# console applications is similar across Microsoft Visual Studio editions\.

Before you perform the following procedure, complete the setup tasks described in [Before you begin with Amazon SES](send-email-getting-started-prerequisites.md) and [Send an email through Amazon SES using SMTP](send-an-email-using-smtp.md)\.

**Important**  
In this getting started tutorial, you send an email to yourself so that you can check to see if you received it\. For further experimentation or load testing, use the Amazon SES mailbox simulator\. Emails that you send to the mailbox simulator do not count toward your sending quota or your bounce and complaint rates\. For more information, see [Testing email sending in Amazon SES](send-email-simulator.md)\.

**To send an email using the Amazon SES SMTP interface with C\#**

1. Create a console project in Visual Studio by performing the following steps:

   1. Open Microsoft Visual Studio\.

   1. On the **File** menu, choose **New**, **Project**\.

   1. On the **New Project** window, in the left pane, expand **Installed**, expand **Templates**, and then expand **Visual C\#**\.

   1. Under **Visual C\#**, choose **Windows Classic Desktop**\.

   1. On the menu at the top of the window, choose **\.NET Framework 4\.5**, as shown in the following image\.  
![\[The .NET Framework selection menu on the New Project window.\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/getting_started_dotnet_smtp_new_project.png)
**Note**  
You can select a later version of the \.NET Framework if necessary\.

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
   + `SENDER@EXAMPLE.COM`—Replace with your "From" email address\. You must verify this address before you run this program\. For more information, see [Verifying identities in Amazon SES](verify-addresses-and-domains.md)\.
   + `RECIPIENT@EXAMPLE.COM`—Replace with your "To" email address\. If your account is still in the sandbox, you must verify this address before you use it\. For more information, see [Moving out of the Amazon SES sandbox](request-production-access.md)\.

1. In `Program.cs`, replace the following SMTP credentials with the values that you obtained in [Obtaining your Amazon SES SMTP credentials](smtp-credentials.md):
**Important**  
Your SMTP credentials are different from your AWS credentials\. For more information about credentials, see [Types of Amazon SES credentials](send-email-concepts-credentials.md)\.
   + YOUR\_SMTP\_USERNAME—Replace with your SMTP username\. Note that your SMTP username credential is a 20\-character string of letters and numbers, not an intelligible name\.
   + YOUR\_SMTP\_PASSWORD—Replace with your SMTP password\.

1. \(Optional\) If you want to use an Amazon SES SMTP endpoint in a Region other than US West \(Oregon\), change the value of the variable `HOST` to the endpoint you want to use\. For a list of SMTP endpoint URLs for the AWS Regions where Amazon SES is available, see [Amazon Simple Email Service \(Amazon SES\)](https://docs.aws.amazon.com/general/latest/gr/rande.html#ses_region) in the *AWS General Reference*\.

1. \(Optional\) If you want to use a configuration set when sending this email, change the value of the variable `CONFIGSET` to the name of the configuration set\. For more information about configuration sets, see [Using Amazon SES configuration sets](using-configuration-sets.md)\.

1. Save `Program.cs`\.

1. To build the project, choose **Build** and then choose **Build Solution**\.

1. To run the program, choose **Debug** and then choose **Start Debugging**\.

1. Review the output\. If the email was successfully sent, the console displays "Email sent\!" Otherwise, it displays an error message\.

1. Sign in to the email client of the recipient address\. You will find the message that you sent\.