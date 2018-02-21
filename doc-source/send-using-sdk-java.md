# Send an Email Using the AWS SDK for Java<a name="send-using-sdk-java"></a>

The following procedure shows you how to use [Eclipse IDE for Java EE Developers](http://www.eclipse.org/) and [AWS Toolkit for Eclipse](http://docs.aws.amazon.com/AWSToolkitEclipse/latest/GettingStartedGuide/) to create an AWS SDK project and modify the Java code to send an email through Amazon SES\. 

**Important**  
In this getting started tutorial, you send an email to yourself so that you can check to see if you received it\. For further experimentation or load testing, use the Amazon SES mailbox simulator\. Emails that you send to the mailbox simulator do not count toward your sending quota or your bounce and complaint rates\. For more information, see [Testing Amazon SES Email Sending](mailbox-simulator.md)\.

## Prerequisites<a name="send-using-sdk-java-prerequisites"></a>

Before you begin, perform the following tasks:

+ **Verify your email address with Amazon SES**—Before you can send an email with Amazon SES, you must verify that you own the sender's email address\. If your account is still in the Amazon SES sandbox, you must also verify the recipient email address\. The easiest way to verify email addresses is by using the Amazon SES console\. For more information, see [Verifying Email Addresses in Amazon SES](verify-email-addresses.md)\. 

+ **Get your AWS credentials**—You need an AWS access key ID and AWS secret access key to access Amazon SES using an SDK\. You can find your credentials by using the [Security Credentials](https://console.aws.amazon.com/iam/home?#security_credential) page in the AWS Management Console\. For more information about credentials, see [Using Credentials With Amazon SES](using-credentials.md)\.

+ **Install Eclipse**—Eclipse is available at [https://www\.eclipse\.org/downloads](https://www.eclipse.org/downloads)\. The code in this tutorial was tested using Eclipse Neon\.3 \(version 4\.6\.3\), running version 1\.8 of the Java Runtime Environment\.

+ **Install the AWS Toolkit for Eclipse**—Instructions for adding the AWS Toolkit for Eclipse to your Eclipse installation are available at [https://aws\.amazon\.com/eclipse](https://aws.amazon.com/eclipse)\. The code in this tutorial was tested using version 2\.3\.1 of the AWS Toolkit for Eclipse\.

+ **Create a shared credentials file**—For the sample code in this section to function properly, you must create a shared credentials file\. For more information, see [Create a Shared Credentials File](create-shared-credentials-file.md)\.

## Procedure<a name="send-using-sdk-java-procedure"></a>

The following procedure shows how to send an email through Amazon SES using the AWS SDK for Java\.

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

   + `SENDER@EXAMPLE.COM`—Replace with your "From" email address\. You must verify this address before you run this program\. For more information, see [Verifying Identities in Amazon SES](verify-addresses-and-domains.md)\.

   + `RECIPIENT@EXAMPLE.COM`—Replace with your "To" email address\. If your account is still in the sandbox, you must verify this address before you use it\. For more information, see [Moving Out of the Amazon SES Sandbox](request-production-access.md)\.

   + `US_WEST_2`—Set this to the AWS Region of the Amazon SES endpoint you want to connect to\. Note that your sandbox status, sending limits, and Amazon SES identity\-related settings are specific to a given AWS Region, so be sure to select an AWS Region in which you set up Amazon SES\. In this example, we are using the US West \(Oregon\) Region\. Examples of other Regions that Amazon SES supports are US\_EAST\_1 and EU\_WEST\_1\. For a complete list of AWS Regions that Amazon SES supports, see [Regions and Amazon SES](regions.md)\.

1. Save `AmazonSESSample.java`\.

1. To build the project, choose **Project** and then choose **Build Project**\.
**Note**  
If this option is disabled, automatic building may be enabled; if so, skip this step\.

1. To start the program and send the email, choose **Run** and then choose **Run** again\.

1. Review the output of the console pane in Eclipse\. If the email was successfully sent, the console displays "Email sent\!" Otherwise, it displays an error message\.

1. If the email was successfully sent, sign in to the email client of the recipient address\. You will find the message that you sent\.