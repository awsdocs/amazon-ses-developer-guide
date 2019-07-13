# Send an Email Using the AWS SDK for PHP<a name="send-using-sdk-php"></a>

This topic shows how to use the [AWS SDK for PHP](https://aws.amazon.com/sdk-for-php/) to send an email through Amazon SES\. 

**Important**  
In this tutorial, you send an email to yourself so that you can check to see if you received it\. For further experimentation or load testing, use the Amazon SES mailbox simulator\. Emails that you send to the mailbox simulator do not count toward your sending quota or your bounce and complaint rates\. For more information, see [Testing Email Sending in Amazon SES](mailbox-simulator.md)\.

## Prerequisites<a name="send-using-sdk-php-prerequisites"></a>

Before you begin, perform the following tasks:
+ **Verify your email address with Amazon SES**—Before you can send an email with Amazon SES, you must verify that you own the sender's email address\. If your account is still in the Amazon SES sandbox, you must also verify the recipient email address\. The easiest way to verify email addresses is by using the Amazon SES console\. For more information, see [Verifying Email Addresses in Amazon SES](verify-email-addresses.md)\. 
+ **Get your AWS credentials**—You need an AWS access key ID and AWS secret access key to access Amazon SES using an SDK\. You can find your credentials by using the [Security Credentials](https://console.aws.amazon.com/iam/home?#security_credential) page of the AWS Management Console\. For more information about credentials, see [Using Credentials With Amazon SES](using-credentials.md)\.
+ **Install PHP**—PHP is available at [http://php\.net/downloads\.php](http://php.net/downloads.php)\. This tutorial requires PHP version 5\.5 or higher\. After you install PHP, add the path to PHP in your environment variables so that you can run PHP from any command prompt\. The code in this tutorial was tested using PHP 7\.2\.7\.
+ **Install the AWS SDK for PHP version 3**—For download and installation instructions, see the [AWS SDK for PHP documentation](https://docs.aws.amazon.com/aws-sdk-php/v3/guide/getting-started/installation.html)\. The code in this tutorial was tested using version 3\.64\.13 of the SDK\. 
+ **Create a shared credentials file**—For the sample code in this section to function properly, you must create a shared credentials file\. For more information, see [Create a Shared Credentials File](create-shared-credentials-file.md)\.

## Procedure<a name="send-using-sdk-php-procedure"></a>

The following procedure shows how to send an email through Amazon SES using the AWS SDK for PHP\.

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
   + **`sender@example.com`**—Replace with an email address that you have verified with Amazon SES\. For more information, see [Verifying Identities](verify-addresses-and-domains.md)\. Email addresses in Amazon SES are case\-sensitive\. Make sure that the address you enter is exactly the same as the one you verified\.
   + **`recipient1@example.com`, `recipient2@example.com`**—Replace with the addresses of your recipients\. If your account is still in the sandbox, your recipients' addresses must also be verified\. For more information, see [Moving Out of the Amazon SES Sandbox](request-production-access.md)\. Make sure that the address you enter is exactly the same as the one you verified\.
   + **\(Optional\) `ConfigSet`**—If you want to use a configuration set when sending this email, replace this value with the name of the configuration set\. For more information about configuration sets, see [Using Amazon SES Configuration Sets](using-configuration-sets.md)\.
   + **\(Optional\) `us-west-2`**—If you want to use Amazon SES in a region other than US West \(Oregon\), replace this with the region you want to use\. For a list of regions where Amazon SES is available, see [Amazon Simple Email Service \(Amazon SES\)](https://docs.aws.amazon.com/general/latest/gr/rande.html#ses_region) in the *AWS General Reference*\.

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

1. Sign in to the email client of the recipient address\. You will find the message that you sent\.