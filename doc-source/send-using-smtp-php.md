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
    3. // Modify the path in the require statement below to refer to the 
    4. // location of your Composer autoload.php file.
    5. require 'path_to_sdk_inclusion';
    6. 
    7. // Instantiate a new PHPMailer 
    8. $mail = new PHPMailer;
    9. 
   10. // Tell PHPMailer to use SMTP
   11. $mail->isSMTP();
   12. 
   13. // Replace sender@example.com with your "From" address. 
   14. // This address must be verified with Amazon SES.
   15. $mail->setFrom('sender@example.com', 'Sender Name');
   16. 
   17. // Replace recipient@example.com with a "To" address. If your account 
   18. // is still in the sandbox, this address must be verified.
   19. // Also note that you can include several addAddress() lines to send
   20. // email to multiple recipients.
   21. $mail->addAddress('recipient@example.com', 'Recipient Name');
   22. 
   23. // Replace smtp_username with your Amazon SES SMTP user name.
   24. $mail->Username = 'smtp_username';
   25. 
   26. // Replace smtp_password with your Amazon SES SMTP password.
   27. $mail->Password = 'smtp_password';
   28.     
   29. // Specify a configuration set. If you do not want to use a configuration
   30. // set, comment or remove the next line.
   31. $mail->addCustomHeader('X-SES-CONFIGURATION-SET', 'ConfigSet');
   32.  
   33. // If you're using Amazon SES in a region other than US West (Oregon), 
   34. // replace email-smtp.us-west-2.amazonaws.com with the Amazon SES SMTP  
   35. // endpoint in the appropriate region.
   36. $mail->Host = 'email-smtp.us-west-2.amazonaws.com';
   37. 
   38. // The subject line of the email
   39. $mail->Subject = 'Amazon SES test (SMTP interface accessed using PHP)';
   40. 
   41. // The HTML-formatted body of the email
   42. $mail->Body = '<h1>Email Test</h1>
   43.     <p>This email was sent through the 
   44.     <a href="https://aws.amazon.com/ses">Amazon SES</a> SMTP
   45.     interface using the <a href="https://github.com/PHPMailer/PHPMailer">
   46.     PHPMailer</a> class.</p>';
   47. 
   48. // Tells PHPMailer to use SMTP authentication
   49. $mail->SMTPAuth = true;
   50. 
   51. // Enable TLS encryption over port 587
   52. $mail->SMTPSecure = 'tls';
   53. $mail->Port = 587;
   54. 
   55. // Tells PHPMailer to send HTML-formatted email
   56. $mail->isHTML(true);
   57. 
   58. // The alternative email body; this is only displayed when a recipient
   59. // opens the email in a non-HTML email client. The \r\n represents a 
   60. // line break.
   61. $mail->AltBody = "Email Test\r\nThis email was sent through the 
   62.     Amazon SES SMTP interface using the PHPMailer class.";
   63. 
   64. if(!$mail->send()) {
   65.     echo "Email not sent. " , $mail->ErrorInfo , PHP_EOL;
   66. } else {
   67.     echo "Email sent!" , PHP_EOL;
   68. }
   69. ?>
   ```

1. In `amazon-ses-smtp-sample.php`, replace the following with your own values:
   + **`sender@example.com`**—Replace with an email address that you have verified with Amazon SES\. For more information, see [Verifying Identities](verify-addresses-and-domains.md)\. Email addresses in Amazon SES are case\-sensitive\. Make sure that the address you enter is exactly the same as the one you verified\.
   + **`recipient@example.com`**—Replace with the address of the recipient\. If your account is still in the sandbox, you must verify this address before you use it\. For more information, see [Moving Out of the Amazon SES Sandbox](request-production-access.md)\. Make sure that the address you enter is exactly the same as the one you verified\.
   + **`smtp_username`**—Replace with your SMTP user name credential, which you obtained from the [SMTP Settings](https://console.aws.amazon.com/ses/home?#smtp-settings:) page of the Amazon SES console\. This is **not** the same as your AWS access key ID\. Note that your SMTP user name credential is a 20\-character string of letters and numbers, not an intelligible name\.
   + **`smtp_password`**—Replace with your SMTP password, which you obtained from the [SMTP Settings](https://console.aws.amazon.com/ses/home?#smtp-settings:) page of the Amazon SES console\. This is **not** the same as your AWS secret access key\.
   + **\(Optional\) `email-smtp.us-west-2.amazonaws.com`**—If you want to use an Amazon SES SMTP endpoint in a Region other than US West \(Oregon\), replace this with the Amazon SES SMTP endpoint in the Region you want to use\. For a list of Amazon SES SMTP endpoints, see [Regions and Amazon SES](regions.md)\.

1. Save `amazon-ses-smtp-sample.php`\.

1. To run the program, open a command prompt in the same directory as `amazon-ses-smtp-sample.php`, and then type php amazon\-ses\-smtp\-sample\.php\.

1. Review the output\. If the email was successfully sent, the console displays "Email sent\!" Otherwise, it displays an error message\.

1. Sign in to the email client of the recipient address\. You will find the message that you sent\.