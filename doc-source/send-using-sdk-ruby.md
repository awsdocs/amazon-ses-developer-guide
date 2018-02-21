# Send an Email Using the AWS SDK for Ruby<a name="send-using-sdk-ruby"></a>

This topic shows how to use the [AWS SDK for Ruby](https://aws.amazon.com/sdk-for-ruby/) to send an email through Amazon SES\. 

**Important**  
In this tutorial, you send an email to yourself so that you can check to see if you received it\. For further experimentation or load testing, use the Amazon SES mailbox simulator\. Emails that you send to the mailbox simulator do not count toward your sending quota or your bounce and complaint rates\. For more information, see [Testing Amazon SES Email Sending](mailbox-simulator.md)\.

## Prerequisites<a name="send-using-sdk-ruby-prerequisites"></a>

Before you begin, perform the following tasks:

+ **Verify your email address with Amazon SES**—Before you can send an email with Amazon SES, you must verify that you own the sender's email address\. If your account is still in the Amazon SES sandbox, you must also verify the recipient email address\. The easiest way to verify email addresses is by using the Amazon SES console\. For more information, see [Verifying Email Addresses in Amazon SES](verify-email-addresses.md)\. 

+ **Get your AWS credentials**—You need an AWS access key ID and AWS secret access key to access Amazon SES using an SDK\. You can find your credentials by using the [Security Credentials](https://console.aws.amazon.com/iam/home?#security_credential) page of the AWS Management Console\. For more information about credentials, see [Using Credentials With Amazon SES](using-credentials.md)\.

+ **Install Ruby**—Ruby is available at [https://www\.ruby\-lang\.org/en/downloads/](https://www.ruby-lang.org/en/downloads/)\. The code in this tutorial was tested using Ruby 1\.9\.3\. After you install Ruby, add the path to Ruby in your environment variables so that you can run Ruby from any command prompt\.

+ **Install the AWS SDK for Ruby**—For download and installation instructions, see [Installing the AWS SDK for Ruby](http://docs.aws.amazon.com/sdk-for-ruby/v2/developer-guide/setup-install.html) in the *AWS SDK for Ruby Developer Guide*\. The sample code in this tutorial was tested using version 2\.9\.36 of the AWS SDK for Ruby\.

+ **Create a shared credentials file**—For the sample code in this section to function properly, you must create a shared credentials file\. For more information, see [Create a Shared Credentials File](create-shared-credentials-file.md)\.

## Procedure<a name="send-using-sdk-ruby-procedure"></a>

The following procedure shows how to send an email through Amazon SES using the AWS SDK for Ruby\.

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

   + **`sender@example.com`**—Replace with an email address that you have verified with Amazon SES\. For more information, see [Verifying Identities](verify-addresses-and-domains.md)\. Email addresses in Amazon SES are case\-sensitive\. Make sure that the address you enter is exactly the same as the one you verified\.

   + **`recipient@example.com`**—Replace with the address of the recipient\. If your account is still in the sandbox, you must verify this address before you use it\. For more information, see [Moving Out of the Amazon SES Sandbox](request-production-access.md)\. Make sure that the address you enter is exactly the same as the one you verified\.

   + **\(Optional\) `us-west-2`**—If you want to use Amazon SES in a region other than US West \(Oregon\), replace this with the region you want to use\. For a list of regions in which Amazon SES is available, see [Regions and Amazon SES](regions.md)\. 

1. Save `amazon-ses-sample.rb`\.

1. To run the program, open a command prompt in the same directory as `amazon-ses-sample.rb`, and type ruby amazon\-ses\-sample\.rb

1. Review the output\. If the email was successfully sent, the console displays "Email sent\!" Otherwise, it displays an error message\.

1. Sign in to the email client of the recipient address\. You will find the message that you sent\.