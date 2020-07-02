# Send an email using the AWS SDK for \.NET<a name="send-using-sdk-net"></a>

The following procedure shows you how to send an email through Amazon SES using [Visual Studio](https://www.visualstudio.com/) and the AWS SDK for \.NET\.

This solution was tested using the following components:
+ Microsoft Visual Studio Community 2017, version 15\.4\.0\.
+ Microsoft \.NET Framework version 4\.6\.1\.
+ The AWSSDK\.Core package \(version 3\.3\.19\), installed using NuGet\.
+ The AWSSDK\.SimpleEmail package \(version 3\.3\.6\.1\), installed using NuGet\.

**Note**  
In this getting started tutorial, you send an email to yourself so that you can check to see if you received it\. For further experimentation or load testing, use the Amazon SES mailbox simulator\. Emails that you send to the mailbox simulator do not count toward your sending quota or your bounce and complaint rates\. For more information, see [Testing email sending in Amazon SES](send-email-simulator.md)\.

## Prerequisites<a name="send-using-sdk-net-prerequisites"></a>

Before you begin, perform the following tasks:
+ **Verify your email address with Amazon SES**—Before you can send an email with Amazon SES, you must verify that you own the sender's email address\. If your account is still in the Amazon SES sandbox, you must also verify the recipient email address\. The easiest way to verify email addresses is by using the Amazon SES console\. For more information, see [Verifying email addresses in Amazon SES](verify-email-addresses.md)\. 
+ **Get your AWS credentials**—You need an AWS access key ID and AWS secret access key to access Amazon SES using an SDK\. You can find your credentials by using the [Security Credentials](https://console.aws.amazon.com/iam/home?#security_credential) page of the AWS Management Console\. For more information about credentials, see [Using credentials with Amazon SES](using-credentials.md)\.
+ **Install Visual Studio**—Visual Studio is available at [https://www\.visualstudio\.com/](https://www.visualstudio.com/)\.
+ **Create a shared credentials file**—For the sample code in this section to function properly, you must create a shared credentials file\. For more information, see [Create a shared credentials file](create-shared-credentials-file.md)\.

## Procedure<a name="send-using-sdk-net-procedure"></a>

The following procedure shows how to send an email through Amazon SES using the AWS SDK for \.NET\.

**To send an email using the AWS SDK for \.NET**

1. Create a new project by performing the following steps:

   1. Start Visual Studio\.

   1. On the **File** menu, choose **New**, **Project**\.

   1. On the **New Project** window, in the panel on the left, expand **Installed**, and then expand **Visual C\#**\.

   1. In the panel on the right, choose **Console App \(\.NET Framework\)**\.

   1. For **Name**, type **AmazonSESSample**, and then choose **OK**\.

1. Use NuGet to include the Amazon SES packages in your solution by completing the following steps:

   1. In the **Solution Explorer** pane, right\-click your project, and then choose **Manage NuGet Packages**\.

   1. On the **NuGet: AmazonSESSample** tab, choose **Browse**\.

   1. In the search box, type **AWSSDK\.SimpleEmail**\. 

   1. Choose the **AWSSDK\.SimpleEmail** package, and then choose **Install**\.

   1. On the **Preview Changes** window, choose **OK**\.

1. On the **Program\.cs** tab, paste the following code:

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

1. In the code editor, do the following:
   + Replace *sender@example\.com* with the "From:" email address\. This address must be verified\. For more information, see [Verifying identities in Amazon SES](verify-addresses-and-domains.md)\.
   + Replace *recipient@example\.com* with the "To:" address\. If your account is still in the sandbox, this address must also be verified\.
   + Replace *ConfigSet* with the name of the configuration set to use when sending this email\.
   + Replace *USWest2* with the name of the AWS Region endpoint you use to send email using Amazon SES\. For a list of regions where Amazon SES is available, see [Amazon Simple Email Service \(Amazon SES\)](https://docs.aws.amazon.com/general/latest/gr/rande.html#ses_region) in the *AWS General Reference*\.

   When you finish, save `Program.cs`\.

1. Build and run the application by completing the following steps:

   1. On the **Build** menu, choose **Build Solution**\.

   1. On the **Debug** menu, choose **Start Debugging**\. A console window appears\.

1. Review the output of the console\. If the email was successfully sent, the console displays "The email was sent successfully\." 

1. If the email was successfully sent, sign in to the email client of the recipient address\. You will find the message that you sent\.