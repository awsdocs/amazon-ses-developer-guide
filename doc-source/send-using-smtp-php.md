# Send an Email Using SMTP with PHP<a name="send-using-smtp-php"></a>

This example uses the PHPMailer class to send email through Amazon SES using the SMTP interface\. 

**Important**  
In this tutorial, you send an email to yourself so that you can check to see if you received it\. For further experimentation or load testing, use the Amazon SES mailbox simulator\. Emails that you send to the mailbox simulator do not count toward your sending quota or your bounce and complaint rates\. For more information, see [Testing Email Sending in Amazon SES](send-email-simulator.md)\.

## Prerequisites<a name="send-using-smtp-php-prerequisites"></a>

Before you begin, perform the following tasks:
+ **Verify your email address with Amazon SES**— Before you can send an email with Amazon SES, you must verify that you own the sender's email address\. If your account is still in the Amazon SES sandbox, you must also verify the recipient email address\. The easiest way to verify email addresses is by using the Amazon SES console\. For more information, see [Verifying Email Addresses in Amazon SES](verify-email-addresses.md)\. 
+ **Get your SMTP credentials**—You need an Amazon SES SMTP user name and password to access the Amazon SES SMTP interface\. Your SMTP credentials are **not** the same as your AWS credentials\. You can find your SMTP credentials by going to the [SMTP Settings](https://console.aws.amazon.com/ses/home?#smtp-settings:) page of the Amazon SES console\. For more information about SMTP credentials, see [Obtaining Your Amazon SES SMTP Credentials](smtp-credentials.md)\.
+ **Install PHP**—PHP is available at [http://php\.net/downloads\.php](https://php.net/downloads.php)\. After you install PHP, add the path to PHP in your environment variables so that you can run PHP from any command prompt\.
+ **Install the Composer dependency manager**—The Composer dependency manager will enable you to download and install the PHPMailer class and its dependencies\. To install Composer, follow the installation instructions at [https://getcomposer\.org/download](https://getcomposer.org/download)\.
+ **Install the PHPMailer class**— After you install Composer, run the following command to install PHPMailer: 

  ```
  path/to/composer require phpmailer/phpmailer
  ```

  In the preceding command, replace *path/to/* with the path where you installed Composer\.

## Procedure<a name="send-using-smtp-php-procedure"></a>

The following procedure shows how to send an email through Amazon SES with PHP\.

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
   + **`sender@example.com`**—Replace with an email address that you have verified with Amazon SES\. For more information, see [Verifying Identities](verify-addresses-and-domains.md)\. Email addresses in Amazon SES are case\-sensitive\. Make sure that the address you enter is exactly the same as the one you verified\.
   + **`recipient@example.com`**—Replace with the address of the recipient\. If your account is still in the sandbox, you must verify this address before you use it\. For more information, see [Moving Out of the Amazon SES Sandbox](request-production-access.md)\. Make sure that the address you enter is exactly the same as the one you verified\.
   + **`smtp_username`**—Replace with your SMTP user name credential, which you obtained from the [SMTP Settings](https://console.aws.amazon.com/ses/home?#smtp-settings:) page of the Amazon SES console\. This is **not** the same as your AWS access key ID\. Note that your SMTP user name credential is a 20\-character string of letters and numbers, not an intelligible name\.
   + **`smtp_password`**—Replace with your SMTP password, which you obtained from the [SMTP Settings](https://console.aws.amazon.com/ses/home?#smtp-settings:) page of the Amazon SES console\. This is **not** the same as your AWS secret access key\.
   + **\(Optional\) `ConfigSet`**—If you want to use a configuration set when sending this email, replace this value with the name of the configuration set\. For more information about configuration sets, see [Using Amazon SES Configuration Sets](using-configuration-sets.md)\.
   + **\(Optional\) `email-smtp.us-west-2.amazonaws.com`**—If you want to use an Amazon SES SMTP endpoint in a Region other than US West \(Oregon\), replace this with the Amazon SES SMTP endpoint in the Region you want to use\. For a list of SMTP endpoint URLs for the AWS Regions where Amazon SES is available, see [Amazon Simple Email Service \(Amazon SES\)](https://docs.aws.amazon.com/general/latest/gr/rande.html#ses_region) in the *AWS General Reference*\.

1. Save `amazon-ses-smtp-sample.php`\.

1. To run the program, open a command prompt in the same directory as `amazon-ses-smtp-sample.php`, and then type php amazon\-ses\-smtp\-sample\.php\.

1. Review the output\. If the email was successfully sent, the console displays "Email sent\!" Otherwise, it displays an error message\.

1. Sign in to the email client of the recipient address\. You will find the message that you sent\.