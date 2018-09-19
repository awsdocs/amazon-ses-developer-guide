# Send an Email Using SMTP with PHP<a name="send-using-smtp-php"></a>

This example uses the PHPMailer package to send email through Amazon SES using the SMTP interface\. 

**Important**  
In this tutorial, you send an email to yourself so that you can check to see if you received it\. For further experimentation or load testing, use the Amazon SES mailbox simulator\. Emails that you send to the mailbox simulator do not count toward your sending quota or your bounce and complaint rates\. For more information, see [Testing Email Sending in Amazon SES](mailbox-simulator.md)\.

## Prerequisites<a name="send-using-smtp-php-prerequisites"></a>

Before you begin, perform the following tasks:
+ **Verify your email address with Amazon SES**— Before you can send an email with Amazon SES, you must verify that you own the sender's email address\. If your account is still in the Amazon SES sandbox, you must also verify the recipient email address\. The easiest way to verify email addresses is by using the Amazon SES console\. For more information, see [Verifying Email Addresses in Amazon SES](verify-email-addresses.md)\. 
+ **Get your SMTP credentials**—You need an Amazon SES SMTP user name and password to access the Amazon SES SMTP interface\. Your SMTP credentials are **not** the same as your AWS credentials\. You can find your SMTP credentials by going to the [SMTP Settings](https://console.aws.amazon.com/ses/home?#smtp-settings:) page of the Amazon SES console\. For more information about SMTP credentials, see [Obtaining Your Amazon SES SMTP Credentials](smtp-credentials.md)\.
+ **Install PHP**—PHP is available at [http://php\.net/downloads\.php](https://php.net/downloads.php)\. After you install PHP, add the path to PHP in your environment variables so that you can run PHP from any command prompt\.
+ **Install the Composer dependency manager**—The Composer dependency manager will enable you to download and install the PHPMailer class and its dependencies\. To install Composer, follow the installation instructions at [https://getcomposer\.org/download](https://getcomposer.org/download)\.
+ **Install the PHPMailer package**— Once you have installed Composer, open the file `composer.json` in a text editor\. In the `require` section, add the following line: `"phpmailer/phpmailer":"~5.2"`, and then save the file\. At the command line, change to the directory that contains the `composer.json` file, and then type php composer\.phar update to download and install PHPMailer\.

## Procedure<a name="send-using-smtp-php-procedure"></a>

The following procedure shows how to send an email through Amazon SES with PHP\.

**To send an email using the Amazon SES SMTP interface with PHP**

1. Create a file named `amazon-ses-smtp-sample.php`\. Open the file with a text editor and paste in the following code:

   ```
    1. <?php
    2. 
    3. // If necessary, modify the path in the require statement below to refer to the 
    4. // location of your Composer autoload.php file.
    5. require 'vendor/autoload.php';
    6. 
    7. use PHPMailer\PHPMailer\PHPMailer;
    8. 
    9. // Instantiate a new PHPMailer 
   10. $mail = new PHPMailer;
   11. 
   12. // Tell PHPMailer to use SMTP
   13. $mail->isSMTP();
   14. 
   15. // Replace sender@example.com with your "From" address. 
   16. // This address must be verified with Amazon SES.
   17. $mail->setFrom('sender@example.com', 'Sender Name');
   18. 
   19. // Replace recipient@example.com with a "To" address. If your account 
   20. // is still in the sandbox, this address must be verified.
   21. // Also note that you can include several addAddress() lines to send
   22. // email to multiple recipients.
   23. $mail->addAddress('recipient@example.com', 'Recipient Name');
   24. 
   25. // Replace smtp_username with your Amazon SES SMTP user name.
   26. $mail->Username = 'smtp_username';
   27. 
   28. // Replace smtp_password with your Amazon SES SMTP password.
   29. $mail->Password = 'smtp_password';
   30.     
   31. // Specify a configuration set. If you do not want to use a configuration
   32. // set, comment or remove the next line.
   33. $mail->addCustomHeader('X-SES-CONFIGURATION-SET', 'ConfigSet');
   34.  
   35. // If you're using Amazon SES in a region other than US West (Oregon), 
   36. // replace email-smtp.us-west-2.amazonaws.com with the Amazon SES SMTP  
   37. // endpoint in the appropriate region.
   38. $mail->Host = 'email-smtp.us-west-2.amazonaws.com';
   39. 
   40. // The subject line of the email
   41. $mail->Subject = 'Amazon SES test (SMTP interface accessed using PHP)';
   42. 
   43. // The HTML-formatted body of the email
   44. $mail->Body = '<h1>Email Test</h1>
   45.     <p>This email was sent through the 
   46.     <a href="https://aws.amazon.com/ses">Amazon SES</a> SMTP
   47.     interface using the <a href="https://github.com/PHPMailer/PHPMailer">
   48.     PHPMailer</a> class.</p>';
   49. 
   50. // Tells PHPMailer to use SMTP authentication
   51. $mail->SMTPAuth = true;
   52. 
   53. // Enable TLS encryption over port 587
   54. $mail->SMTPSecure = 'tls';
   55. $mail->Port = 587;
   56. 
   57. // Tells PHPMailer to send HTML-formatted email
   58. $mail->isHTML(true);
   59. 
   60. // The alternative email body; this is only displayed when a recipient
   61. // opens the email in a non-HTML email client. The \r\n represents a 
   62. // line break.
   63. $mail->AltBody = "Email Test\r\nThis email was sent through the 
   64.     Amazon SES SMTP interface using the PHPMailer class.";
   65. 
   66. if(!$mail->send()) {
   67.     echo "Email not sent. " , $mail->ErrorInfo , PHP_EOL;
   68. } else {
   69.     echo "Email sent!" , PHP_EOL;
   70. }
   71. ?>
   ```

1. In `amazon-ses-smtp-sample.php`, replace the following with your own values:
   + **`sender@example.com`**—Replace with an email address that you have verified with Amazon SES\. For more information, see [Verifying Identities](verify-addresses-and-domains.md)\. Email addresses in Amazon SES are case\-sensitive\. Make sure that the address you enter is exactly the same as the one you verified\.
   + **`recipient@example.com`**—Replace with the address of the recipient\. If your account is still in the sandbox, you must verify this address before you use it\. For more information, see [Moving Out of the Amazon SES Sandbox](request-production-access.md)\. Make sure that the address you enter is exactly the same as the one you verified\.
   + **`smtp_username`**—Replace with your SMTP user name credential, which you obtained from the [SMTP Settings](https://console.aws.amazon.com/ses/home?#smtp-settings:) page of the Amazon SES console\. This is **not** the same as your AWS access key ID\. Note that your SMTP user name credential is a 20\-character string of letters and numbers, not an intelligible name\.
   + **`smtp_password`**—Replace with your SMTP password, which you obtained from the [SMTP Settings](https://console.aws.amazon.com/ses/home?#smtp-settings:) page of the Amazon SES console\. This is **not** the same as your AWS secret access key\.
   + **\(Optional\) `ConfigSet`**—If you want to use a configuration set when sending this email, replace this value with the name of the configuration set\. For more information about configuration sets, see [Using Amazon SES Configuration Sets](using-configuration-sets.md)\.
   + **\(Optional\) `email-smtp.us-west-2.amazonaws.com`**—If you want to use an Amazon SES SMTP endpoint in a Region other than US West \(Oregon\), replace this with the Amazon SES SMTP endpoint in the Region you want to use\. For a list of Amazon SES SMTP endpoints, see [Regions and Amazon SES](regions.md)\.

1. Save `amazon-ses-smtp-sample.php`\.

1. To run the program, open a command prompt in the same directory as `amazon-ses-smtp-sample.php`, and then type php amazon\-ses\-smtp\-sample\.php\.

1. Review the output\. If the email was successfully sent, the console displays "Email sent\!" Otherwise, it displays an error message\.

1. Sign in to the email client of the recipient address\. You will find the message that you sent\.