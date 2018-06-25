# Send an Email Using the AWS SDK for PHP<a name="send-using-sdk-php"></a>

This topic shows how to use the [AWS SDK for PHP](https://aws.amazon.com/sdk-for-php/) to send an email through Amazon SES\. 

**Important**  
In this tutorial, you send an email to yourself so that you can check to see if you received it\. For further experimentation or load testing, use the Amazon SES mailbox simulator\. Emails that you send to the mailbox simulator do not count toward your sending quota or your bounce and complaint rates\. For more information, see [Testing Email Sending in Amazon SES](mailbox-simulator.md)\.

## Prerequisites<a name="send-using-sdk-php-prerequisites"></a>

Before you begin, perform the following tasks:
+ **Verify your email address with Amazon SES**—Before you can send an email with Amazon SES, you must verify that you own the sender's email address\. If your account is still in the Amazon SES sandbox, you must also verify the recipient email address\. The easiest way to verify email addresses is by using the Amazon SES console\. For more information, see [Verifying Email Addresses in Amazon SES](verify-email-addresses.md)\. 
+ **Get your AWS credentials**—You need an AWS access key ID and AWS secret access key to access Amazon SES using an SDK\. You can find your credentials by using the [Security Credentials](https://console.aws.amazon.com/iam/home?#security_credential) page of the AWS Management Console\. For more information about credentials, see [Using Credentials With Amazon SES](using-credentials.md)\.
+ **Install PHP**—PHP is available at [http://php\.net/downloads\.php](http://php.net/downloads.php)\. This tutorial requires PHP version 5\.5 or higher\. After you install PHP, add the path to PHP in your environment variables so that you can run PHP from any command prompt\. The code in this tutorial was tested using PHP 7\.0\.20\.
+ **Install the AWS SDK for PHP version 3**—For download and installation instructions, see the [AWS SDK for PHP documentation](http://docs.aws.amazon.com/aws-sdk-php/v3/guide/getting-started/installation.html)\. The code in this tutorial was tested using version 3\.31\.0 of the SDK\. 
+ **Create a shared credentials file**—For the sample code in this section to function properly, you must create a shared credentials file\. For more information, see [Create a Shared Credentials File](create-shared-credentials-file.md)\.

## Procedure<a name="send-using-sdk-php-procedure"></a>

The following procedure shows how to send an email through Amazon SES using the AWS SDK for PHP\.

**To send an email through Amazon SES using the AWS SDK for PHP**

1. In a text editor, create a file named `amazon-ses-sample.php`\. Paste the following code:

   ```
    1. <?php
    2. 
    3. // Replace path_to_sdk_inclusion with the path to the SDK as described in 
    4. // http://docs.aws.amazon.com/aws-sdk-php/v3/guide/getting-started/basic-usage.html
    5. define('REQUIRED_FILE','path_to_sdk_inclusion'); 
    6.                                                   
    7. // Replace sender@example.com with your "From" address. 
    8. // This address must be verified with Amazon SES.
    9. define('SENDER', 'sender@example.com');           
   10. 
   11. // Replace recipient@example.com with a "To" address. If your account 
   12. // is still in the sandbox, this address must be verified.
   13. define('RECIPIENT', 'recipient@example.com');    
   14. 
   15. // Specify a configuration set. If you do not want to use a configuration
   16. // set, comment the following variable, and the 
   17. // 'ConfigurationSetName' => CONFIGSET argument below.
   18. define('CONFIGSET','ConfigSet');
   19. 
   20. // Replace us-west-2 with the AWS Region you're using for Amazon SES.
   21. define('REGION','us-west-2'); 
   22. 
   23. define('SUBJECT','Amazon SES test (AWS SDK for PHP)');
   24. 
   25. define('HTMLBODY','<h1>AWS Amazon Simple Email Service Test Email</h1>'.
   26.                   '<p>This email was sent with <a href="https://aws.amazon.com/ses/">'.
   27.                   'Amazon SES</a> using the <a href="https://aws.amazon.com/sdk-for-php/">'.
   28.                   'AWS SDK for PHP</a>.</p>');
   29. define('TEXTBODY','This email was send with Amazon SES using the AWS SDK for PHP.');
   30. 
   31. define('CHARSET','UTF-8');
   32. 
   33. require REQUIRED_FILE;
   34. 
   35. use Aws\Ses\SesClient;
   36. use Aws\Ses\Exception\SesException;
   37. 
   38. $client = SesClient::factory(array(
   39.     'version'=> 'latest',     
   40.     'region' => REGION
   41. ));
   42. 
   43. try {
   44.      $result = $client->sendEmail([
   45.     'Destination' => [
   46.         'ToAddresses' => [
   47.             RECIPIENT,
   48.         ],
   49.     ],
   50.     'Message' => [
   51.         'Body' => [
   52.             'Html' => [
   53.                 'Charset' => CHARSET,
   54.                 'Data' => HTMLBODY,
   55.             ],
   56. 			'Text' => [
   57.                 'Charset' => CHARSET,
   58.                 'Data' => TEXTBODY,
   59.             ],
   60.         ],
   61.         'Subject' => [
   62.             'Charset' => CHARSET,
   63.             'Data' => SUBJECT,
   64.         ],
   65.     ],
   66.     'Source' => SENDER,
   67.     // If you are not using a configuration set, comment or delete the
   68.     // following line
   69.     'ConfigurationSetName' => CONFIGSET,
   70. ]);
   71.      $messageId = $result->get('MessageId');
   72.      echo("Email sent! Message ID: $messageId"."\n");
   73. 
   74. } catch (SesException $error) {
   75.      echo("The email was not sent. Error message: ".$error->getAwsErrorMessage()."\n");
   76. }
   77. 
   78. ?>
   ```

1. In `amazon-ses-sample.php`, replace the following with your own values:
   + **`path_to_sdk_inclusion`**—Replace with the path required to include the AWS SDK for PHP in the program\. For more information, see the [AWS SDK for PHP documentation](http://docs.aws.amazon.com/aws-sdk-php/v3/guide/getting-started/basic-usage.html)\. 
   + **`sender@example.com`**—Replace with an email address that you have verified with Amazon SES\. For more information, see [Verifying Identities](verify-addresses-and-domains.md)\. Email addresses in Amazon SES are case\-sensitive\. Make sure that the address you enter is exactly the same as the one you verified\.
   + **`recipient@example.com`**—Replace with the address of the recipient\. If your account is still in the sandbox, you must verify this address before you use it\. For more information, see [Moving Out of the Amazon SES Sandbox](request-production-access.md)\. Make sure that the address you enter is exactly the same as the one you verified\.
   + **\(Optional\) `us-west-2`**—If you want to use Amazon SES in a Region other than US West \(Oregon\), replace this with the Region you want to use\. For a list of Regions in which Amazon SES is available, see [Regions and Amazon SES](regions.md)\. 

1. Save `amazon-ses-sample.php`\.

1. To run the program, open a command prompt in the same directory as `amazon-ses-sample.php`, and then type **php amazon\-ses\-sample\.php**

1. Review the output\. If the email was successfully sent, the console displays "Email sent\!" Otherwise, it displays an error message\.
**Note**  
If you encounter a "cURL error 60: SSL certificate problem" error when you run the program, download the latest CA bundle as described in the [AWS SDK for PHP documentation](http://docs.aws.amazon.com/aws-sdk-php/v3/guide/faq.html#what-do-i-do-about-a-curl-ssl-certificate-error)\. Then, in `amazon-ses-sample.php`, add the following lines to the `SesClient::factory` array, replace `path_of_certs` with the path to the CA bundle you downloaded, and re\-run the program\.  

   ```
   1. 'http' => [
   2.    'verify' => 'path_of_certs\ca-bundle.crt'
   3. ]
   ```

1. Sign in to the email client of the recipient address\. You will find the message that you sent\.