# Sending email through Amazon SES using an AWS SDK<a name="send-an-email-using-sdk-programmatically"></a>

You can use an AWS SDK to send email through Amazon SES\. AWS SDKs are available for several programming languages\. For more information, see [Tools for Amazon Web Services](https://aws.amazon.com/tools/#sdk)\.

## Prerequisites<a name="send-an-email-using-sdk-programmatically-prereqs"></a>

The following prerequisites must be completed in order to complete any of the code samples in the next section:
+ If you haven't already done so, complete the tasks in [Setting up Amazon Simple Email Service](setting-up.md)\.
+ **Verify your email address with Amazon SES**—Before you can send an email with Amazon SES, you must verify that you own the sender's email address\. If your account is still in the Amazon SES sandbox, you must also verify the recipient email address\. We recommend you use the Amazon SES console to verify email addresses\. For more information, see [Creating an email address identity](creating-identities.md#verify-email-addresses-procedure)\. 
+ **Get your AWS credentials**—You need an AWS access key ID and AWS secret access key to access Amazon SES using an SDK\. You can find your credentials by using the [Security Credentials](https://console.aws.amazon.com/iam/home?#security_credential) page in the AWS Management Console\. For more information about credentials, see [Types of Amazon SES credentials](send-email-concepts-credentials.md)\.
+ **Create a shared credentials file**—For the sample code in this section to function properly, you must create a shared credentials file\. For more information, see [Creating a shared credentials file to use when sending email through Amazon SES using an AWS SDK](create-shared-credentials-file.md)\.

## Code examples<a name="send-an-email-using-sdk-programmatically-examples"></a>

**Important**  
In the following tutorials, you send an email to yourself so that you can check to see if you received it\. For further experimentation or load testing, use the Amazon SES mailbox simulator\. Emails that you send to the mailbox simulator do not count toward your sending quota or your bounce and complaint rates\. For more information, see [Using the mailbox simulator manually](send-an-email-from-console.md#send-email-simulator)\.

**Topics**

------
#### [ \.NET ]

The following procedure shows you how to send an email through Amazon SES using [Visual Studio](https://www.visualstudio.com/) and the AWS SDK for \.NET\.

This solution was tested using the following components:
+ Microsoft Visual Studio Community 2017, version 15\.4\.0\.
+ Microsoft \.NET Framework version 4\.6\.1\.
+ The AWSSDK\.Core package \(version 3\.3\.19\), installed using NuGet\.
+ The AWSSDK\.SimpleEmail package \(version 3\.3\.6\.1\), installed using NuGet\.

**Before you begin, perform the following tasks:**
+ **Install Visual Studio**—Visual Studio is available at [https://www\.visualstudio\.com/](https://www.visualstudio.com/)\.

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
   + Replace *sender@example\.com* with the "From:" email address\. This address must be verified\. For more information, see [Verified identities in Amazon SES](verify-addresses-and-domains.md)\.
   + Replace *recipient@example\.com* with the "To:" address\. If your account is still in the sandbox, this address must also be verified\.
   + Replace *ConfigSet* with the name of the configuration set to use when sending this email\.
   + Replace *USWest2* with the name of the AWS Region endpoint you use to send email using Amazon SES\. For a list of regions where Amazon SES is available, see [Amazon Simple Email Service \(Amazon SES\)](https://docs.aws.amazon.com/general/latest/gr/rande.html#ses_region) in the *AWS General Reference*\.

   When you finish, save `Program.cs`\.

1. Build and run the application by completing the following steps:

   1. On the **Build** menu, choose **Build Solution**\.

   1. On the **Debug** menu, choose **Start Debugging**\. A console window appears\.

1. Review the output of the console\. If the email was successfully sent, the console displays "The email was sent successfully\." 

1. If the email was successfully sent, sign in to the email client of the recipient address\. You will see the message that you sent\.

------
#### [ Java ]

The following procedure shows you how to use [Eclipse IDE for Java EE Developers](http://www.eclipse.org/) and [AWS Toolkit for Eclipse](https://docs.aws.amazon.com/AWSToolkitEclipse/latest/GettingStartedGuide/) to create an AWS SDK project and modify the Java code to send an email through Amazon SES\. 

**Before you begin, perform the following tasks:**
+ **Install Eclipse**—Eclipse is available at [https://www\.eclipse\.org/downloads](https://www.eclipse.org/downloads)\. The code in this tutorial was tested using Eclipse Neon\.3 \(version 4\.6\.3\), running version 1\.8 of the Java Runtime Environment\.
+ **Install the AWS Toolkit for Eclipse**—Instructions for adding the AWS Toolkit for Eclipse to your Eclipse installation are available at [https://aws\.amazon\.com/eclipse](https://aws.amazon.com/eclipse)\. The code in this tutorial was tested using version 2\.3\.1 of the AWS Toolkit for Eclipse\.

**To send an email using the AWS SDK for Java**

1. Create an AWS Java Project in Eclipse by performing the following steps:

   1. Start Eclipse\.

   1. On the **File** menu, choose **New**, and then choose **Other**\. On the **New** window, expand the **AWS** folder, and then choose **AWS Java Project**\.

   1. In the **New AWS Java Project** dialog box, do the following:

      1. For **Project name**, type a project name\.

      1. Under **AWS SDK for Java Samples**, select **Amazon Simple Email Service JavaMail Sample**\.

      1. Choose **Finish**\.

1. In Eclipse, in the **Package Explorer** pane, expand your project\.

1. Under your project, expand the `src/main/java` folder, expand the `com.amazon.aws.samples` folder, and then double\-click `AmazonSESSample.java`\.

1. Replace the entire contents of `AmazonSESSample.java` with the following code:

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

1. In `AmazonSESSample.java`, replace the following with your own values:
**Important**  
The email addresses are case\-sensitive\. Make sure that the addresses are exactly the same as the ones you verified\.
   + `SENDER@EXAMPLE.COM`—Replace with your "From" email address\. You must verify this address before you run this program\. For more information, see [Verified identities in Amazon SES](verify-addresses-and-domains.md)\.
   + `RECIPIENT@EXAMPLE.COM`—Replace with your "To" email address\. If your account is still in the sandbox, you must verify this address before you use it\. For more information, see [Moving out of the Amazon SES sandbox](request-production-access.md)\.
   + **\(Optional\) `us-west-2`**—If you want to use Amazon SES in a Region other than US West \(Oregon\), replace this with the Region you want to use\. For a list of Regions where Amazon SES is available, see [Amazon Simple Email Service \(Amazon SES\)](https://docs.aws.amazon.com/general/latest/gr/rande.html#ses_region) in the *AWS General Reference*\.

1. Save `AmazonSESSample.java`\.

1. To build the project, choose **Project** and then choose **Build Project**\.
**Note**  
If this option is disabled, automatic building may be enabled; if so, skip this step\.

1. To start the program and send the email, choose **Run** and then choose **Run** again\.

1. Review the output of the console pane in Eclipse\. If the email was successfully sent, the console displays "Email sent\!" Otherwise, it displays an error message\.

1. If the email was successfully sent, sign in to the email client of the recipient address\. You will see the message that you sent\.

------
#### [ PHP ]

This topic shows how to use the [AWS SDK for PHP](https://aws.amazon.com/sdk-for-php/) to send an email through Amazon SES\. 

**Before you begin, perform the following tasks:**
+ **Install PHP**—PHP is available at [http://php\.net/downloads\.php](http://php.net/downloads.php)\. This tutorial requires PHP version 5\.5 or higher\. After you install PHP, add the path to PHP in your environment variables so that you can run PHP from any command prompt\. The code in this tutorial was tested using PHP 7\.2\.7\.
+ **Install the AWS SDK for PHP version 3**—For download and installation instructions, see the [AWS SDK for PHP documentation](https://docs.aws.amazon.com/aws-sdk-php/v3/guide/getting-started/installation.html)\. The code in this tutorial was tested using version 3\.64\.13 of the SDK\. 

**To send an email through Amazon SES using the AWS SDK for PHP**

1. In a text editor, create a file named `amazon-ses-sample.php`\. Paste the following code:

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

1. In `amazon-ses-sample.php`, replace the following with your own values:
   + **`path_to_sdk_inclusion`**—Replace with the path required to include the AWS SDK for PHP in the program\. For more information, see the [AWS SDK for PHP documentation](https://docs.aws.amazon.com/aws-sdk-php/v3/guide/getting-started/basic-usage.html)\. 
   + **`sender@example.com`**—Replace with an email address that you have verified with Amazon SES\. For more information, see [Verified identities](verify-addresses-and-domains.md)\. Email addresses in Amazon SES are case\-sensitive\. Make sure that the address you enter is exactly the same as the one you verified\.
   + **`recipient1@example.com`, `recipient2@example.com`**—Replace with the addresses of your recipients\. If your account is still in the sandbox, your recipients' addresses must also be verified\. For more information, see [Moving out of the Amazon SES sandbox](request-production-access.md)\. Make sure that the address you enter is exactly the same as the one you verified\.
   + **\(Optional\) `ConfigSet`**—If you want to use a configuration set when sending this email, replace this value with the name of the configuration set\. For more information about configuration sets, see [Using configuration sets in Amazon SES](using-configuration-sets.md)\.
   + **\(Optional\) `us-west-2`**—If you want to use Amazon SES in a Region other than US West \(Oregon\), replace this with the Region you want to use\. For a list of Regions where Amazon SES is available, see [Amazon Simple Email Service \(Amazon SES\)](https://docs.aws.amazon.com/general/latest/gr/rande.html#ses_region) in the *AWS General Reference*\.

1. Save `amazon-ses-sample.php`\.

1. To run the program, open a command prompt in the same directory as `amazon-ses-sample.php`, and then type the following command:

   ```
   $ php amazon-ses-sample.php
   ```

1. Review the output\. If the email was successfully sent, the console displays "Email sent\!" Otherwise, it displays an error message\.
**Note**  
If you encounter a "cURL error 60: SSL certificate problem" error when you run the program, download the latest CA bundle as described in the [AWS SDK for PHP documentation](https://docs.aws.amazon.com/aws-sdk-php/v3/guide/faq.html#what-do-i-do-about-a-curl-ssl-certificate-error)\. Then, in `amazon-ses-sample.php`, add the following lines to the `SesClient::factory` array, replace `path_of_certs` with the path to the CA bundle you downloaded, and re\-run the program\.  

   ```
   1. 'http' => [
   2.    'verify' => 'path_of_certs\ca-bundle.crt'
   3. ]
   ```

1. Sign in to the email client of the recipient address\. You will see the message that you sent\.

------
#### [ Ruby ]

This topic shows how to use the [AWS SDK for Ruby](https://aws.amazon.com/sdk-for-ruby/) to send an email through Amazon SES\. 

**Before you begin, perform the following tasks:**
+ **Install Ruby**—Ruby is available at [https://www\.ruby\-lang\.org/en/downloads/](https://www.ruby-lang.org/en/downloads/)\. The code in this tutorial was tested using Ruby 1\.9\.3\. After you install Ruby, add the path to Ruby in your environment variables so that you can run Ruby from any command prompt\.
+ **Install the AWS SDK for Ruby**—For download and installation instructions, see [Installing the AWS SDK for Ruby](https://docs.aws.amazon.com/sdk-for-ruby/latest/developer-guide/setup-install.html) in the *AWS SDK for Ruby Developer Guide*\. The sample code in this tutorial was tested using version 2\.9\.36 of the AWS SDK for Ruby\.
+ **Create a shared credentials file**—For the sample code in this section to function properly, you must create a shared credentials file\. For more information, see [Creating a shared credentials file to use when sending email through Amazon SES using an AWS SDK](create-shared-credentials-file.md)\.

**To send an email through Amazon SES using the AWS SDK for Ruby**

1. In a text editor, create a file named `amazon-ses-sample.rb`\. Paste the following code into the file:

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

1. In `amazon-ses-sample.rb`, replace the following with your own values:
   + **`sender@example.com`**—Replace with an email address that you have verified with Amazon SES\. For more information, see [Verified identities](verify-addresses-and-domains.md)\. Email addresses in Amazon SES are case\-sensitive\. Make sure that the address you enter is exactly the same as the one you verified\.
   + **`recipient@example.com`**—Replace with the address of the recipient\. If your account is still in the sandbox, you must verify this address before you use it\. For more information, see [Moving out of the Amazon SES sandbox](request-production-access.md)\. Make sure that the address you enter is exactly the same as the one you verified\.
   + **\(Optional\) `us-west-2`**—If you want to use Amazon SES in a Region other than US West \(Oregon\), replace this with the Region you want to use\. For a list of Regions where Amazon SES is available, see [Amazon Simple Email Service \(Amazon SES\)](https://docs.aws.amazon.com/general/latest/gr/rande.html#ses_region) in the *AWS General Reference*\.

1. Save `amazon-ses-sample.rb`\.

1. To run the program, open a command prompt in the same directory as `amazon-ses-sample.rb`, and type ruby amazon\-ses\-sample\.rb

1. Review the output\. If the email was successfully sent, the console displays "Email sent\!" Otherwise, it displays an error message\.

1. Sign in to the email client of the recipient address\. You will find the message that you sent\.

------
#### [ Python ]

This topic shows how to use the [AWS SDK for Python \(Boto\)](https://aws.amazon.com/sdk-for-python/) to send an email through Amazon SES\. 

**Before you begin, perform the following tasks:**
+ **Verify your email address with Amazon SES**—Before you can send an email with Amazon SES, you must verify that you own the sender's email address\. If your account is still in the Amazon SES sandbox, you must also verify the recipient email address\. We recommend you use the Amazon SES console to verify email addresses\. For more information, see [Creating an email address identity](creating-identities.md#verify-email-addresses-procedure)\. 
+ **Get your AWS credentials**—You need an AWS access key ID and AWS secret access key to access Amazon SES using an SDK\. You can find your credentials by using the [Security Credentials](https://console.aws.amazon.com/iam/home?#security_credential) page of the AWS Management Console\. For more information about credentials, see [Types of Amazon SES credentials](send-email-concepts-credentials.md)\.
+ **Install Python**—Python is available at [https://www\.python\.org/downloads/](https://www.python.org/downloads/)\. The code in this tutorial was tested using Python 2\.7\.6 and Python 3\.6\.1\. After you install Python, add the path to Python in your environment variables so that you can run Python from any command prompt\.
+ **Install the AWS SDK for Python \(Boto\)**—For download and installation instructions, see the [AWS SDK for Python \(Boto\) documentation](https://boto3.readthedocs.io/en/latest/guide/quickstart.html#installation)\. The sample code in this tutorial was tested using version 1\.4\.4 of the SDK for Python\.

**To send an email through Amazon SES using the SDK for Python**

1. In a text editor, create a file named `amazon-ses-sample.py`\. Paste the following code into the file:

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

1. In `amazon-ses-sample.py`, replace the following with your own values:
   + **`sender@example.com`**—Replace with an email address that you have verified with Amazon SES\. For more information, see [Verified identities](verify-addresses-and-domains.md)\. Email addresses in Amazon SES are case sensitive\. Make sure that the address you enter is exactly the same as the one you verified\.
   + **`recipient@example.com`**—Replace with the address of the recipient\. If your account is still in the sandbox, you must verify this address before you use it\. For more information, see [Moving out of the Amazon SES sandbox](request-production-access.md)\. Make sure that the address you enter is exactly the same as the one you verified\.
   + **\(Optional\) `us-west-2`**—If you want to use Amazon SES in a Region other than US West \(Oregon\), replace this with the Region you want to use\. For a list of Regions where Amazon SES is available, see [Amazon Simple Email Service \(Amazon SES\)](https://docs.aws.amazon.com/general/latest/gr/rande.html#ses_region) in the *AWS General Reference*\.

1. Save `amazon-ses-sample.py`\.

1. To run the program, open a command prompt in the same directory as `amazon-ses-sample.py`, and then type python amazon\-ses\-sample\.py\.

1. Review the output\. If the email was successfully sent, the console displays "Email sent\!" Otherwise, it displays an error message\.

1. Sign in to the email client of the recipient address\. You will see the message that you sent\.

------