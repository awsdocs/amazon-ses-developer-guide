# Using the Command Line to Send Email Through the Amazon SES SMTP Interface<a name="send-email-smtp-client-command-line"></a>

You can interact with the Amazon SES SMTP interface from the command line by using a common utility called OpenSSL\. In most cases, you'll only use OpenSSL to test your ability to connect to the SMTP interface\. However, you can also use OpenSSL to write your own applications that send email using Amazon SES\.

OpenSSL is an open\-source utility, and is compatible with all operating systems\. It is included by default in most versions of Linux and macOS, and can be compiled to run on Windows\. To learn more about OpenSSL, see [https://www\.openssl\.org](https://www.openssl.org)\.

## Prerequisites<a name="send-email-smtp-client-command-line-prereqs"></a>

In order to connect to the Amazon SES SMTP interface using OpenSSL, you must first obtain your SMTP credentials\.

**Important**  
Your SMTP credentials are different from your standard AWS credentials\. For more information about obtaining your SMTP credentials, see [[ERROR] BAD/MISSING LINK TEXT](smtp-credentials.md)\.

After you obtain your SMTP credentials, you must encode them using base64 encoding\.

**To encode your SMTP user name and password using base64**

1. At the command line, type the following command to encode your SMTP user name, replacing `SMTPUsername` with your SMTP user name:

   ```
   echo -n "SMTPUsername" | openssl enc -base64
   ```

   Make a note of the output of this command\.

1. At the command line, type the following command to encode your SMTP password, replacing `SMTPPassword` with your SMTP password:

   ```
   echo -n "SMTPPassword" | openssl enc -base64
   ```

   Make a note of the output of this command\.

## Testing Your Connection to the Amazon SES SMTP Interface<a name="send-email-smtp-client-command-line-testing"></a>

There are two ways to connect to the Amazon SES SMTP interface with OpenSSL: using STARTTLS on port 587, and using SSL on port 465\. Both options offer the same capabilities\. If you are unsure of which option to choose, we recommend using STARTTLS on port 587\.

------
#### [ STARTTLS \(Port 587\) ]

**To connect to the SMTP interface using STARTTLS**

+ At the command line, type the following command to connect to the Amazon SES SMTP server:

  ```
  openssl s_client -crlf -quiet -starttls smtp -connect email-smtp.us-west-2.amazonaws.com:587
  ```
**Note**  
Replace *email\-smtp\.us\-west\-2\.amazonaws\.com* with the URL of the Amazon SES SMTP endpoint for your AWS Region\. For more information, see [[ERROR] BAD/MISSING LINK TEXT](regions.md)\.

  If the connection was successful, you will see output similar to the following:

  ```
  depth=1 C = US, O = Symantec Corporation, OU = Symantec Trust Network, CN = Symantec Class 3 Secure Server CA - G4
  250 Ok
  ```

  The connection automatically closes after about 10 seconds of inactivity\.

------
#### [ SSL \(Port 465\) ]

**To connect to the SMTP interface using SSL**

+ At the command line, type the following command to connect to the Amazon SES SMTP server:

  ```
  openssl s_client -crlf -quiet -connect email-smtp.us-west-2.amazonaws.com:465
  ```
**Note**  
Replace *email\-smtp\.us\-west\-2\.amazonaws\.com* with the URL of the Amazon SES SMTP endpoint for your AWS Region\. For more information, see [[ERROR] BAD/MISSING LINK TEXT](regions.md)\.

  If the connection was successful, you will see output similar to the following:

  ```
  depth=1 C = US, O = Symantec Corporation, OU = Symantec Trust Network, CN = Symantec Class 3 Secure Server CA - G4
  220 email-smtp.amazonaws.com ESMTP SimpleEmailService-2638499997 QSByYW5kb20gc3RyaW5n
  ```

  The connection automatically closes after about 10 seconds of inactivity\.

------

## Using OpenSSL to Send Email Using the Amazon SES SMTP Interface<a name="send-email-using-openssl"></a>

When an email sender \(a client\) connects to an SMTP server, the client issues a standard set of requests, and the server replies to each request with a standard response\. This series of requests and responses is called an *SMTP conversation*\. When you connect to the Amazon SES SMTP server using OpenSSL, the server expects an SMTP conversation to occur\.

In this example, you'll add all of the client requests to a text file, and then use that file as input to one of the OpenSSL commands listed in the previous section\. 

------
#### [ STARTTLS \(Port 587\) ]

**To send an email from the command line using the SMTP interface**

1. In a text editor, create a new file\. Paste the following code into the file\.

   ```
   EHLO example.com
   AUTH LOGIN
   Base64EncodedSMTPUserName
   Base64EncodedSMTPPassword
   MAIL FROM: sender@example.com
   RCPT TO: recipient@example.com
   DATA
   X-SES-CONFIGURATION-SET: ConfigSet
   From: Sender Name <sender@example.com>
   To: recipient@example.com
   Subject: Amazon SES SMTP Test
   
   This message was sent using the Amazon SES SMTP interface.
   .
   QUIT
   ```

1. Make the following changes to the file you created in the previous step:

   + Replace *example\.com* with your sending domain\.

   + Replace *Base64EncodedSMTPUserName* with your base64\-encoded SMTP user name\.

   + Replace *Base64EncodedSMTPPassword* with your base64\-encoded SMTP password\.

   + Replace *sender@example\.com* with the email address you are sending from\. This identity must be verified\.

   + Replace *recipient@example\.com* with the destination email address\. If your Amazon SES account is still in the sandbox, this address must be verified\.

   + Replace *ConfigSet* with the name of the configuration set that you want to apply to this email\.
**Note**  
If you do not want to use a configuration set, remove the entire line that begins with "`X-SES-CONFIGURATION-SET`"\.

1. Save the file as `input.txt`\.

1. At the command line, type the following command:

   ```
   openssl s_client -crlf -quiet -starttls smtp -connect email-smtp.us-west-2.amazonaws.com:587 < input.txt
   ```
**Note**  
Replace *email\-smtp\.us\-west\-2\.amazonaws\.com* with the URL of the Amazon SES SMTP endpoint for your AWS Region\. For more information, see [[ERROR] BAD/MISSING LINK TEXT](regions.md)\.

   If the message was sent, you'll see output similar to the following:

   ```
   250 Ok 01010160d7de98d8-21e57d9a-JZho-416c-bbe1-8ebaAexample-000000
   ```

   The string of numbers and text that follows `250 Ok` is the message ID of the email\.
**Note**  
The connection may remain open after the message is sent\. If it does, it will automatically close after about 10 seconds of inactivity\.

------
#### [ SSL \(Port 465\) ]

**To send an email from the command line using the SMTP interface**

1. In a text editor, create a new file\. Paste the following code into the file\.

   ```
   EHLO example.com
   AUTH LOGIN
   Base64EncodedSMTPUserName
   Base64EncodedSMTPPassword
   MAIL FROM: sender@example.com
   RCPT TO: recipient@example.com
   DATA
   X-SES-CONFIGURATION-SET: ConfigSet
   From: Sender Name <sender@example.com>
   To: recipient@example.com
   Subject: Amazon SES SMTP Test
   
   This message was sent using the Amazon SES SMTP interface.
   .
   QUIT
   ```

1. Make the following changes to the file you created in the previous step:

   + Replace *example\.com* with your sending domain\.

   + Replace *Base64EncodedSMTPUserName* with your base64\-encoded SMTP user name\.

   + Replace *Base64EncodedSMTPPassword* with your base64\-encoded SMTP password\.

   + Replace *sender@example\.com* with the email address you are sending from\. This identity must be verified\.

   + Replace *recipient@example\.com* with the destination email address\. If your Amazon SES account is still in the sandbox, this address must be verified\.

   + Replace *ConfigSet* with the name of the configuration set that you want to apply to this email\.
**Note**  
If you do not want to use a configuration set, remove the entire line that begins with "`X-SES-CONFIGURATION-SET`"\.

1. Save the file as `input.txt`\.

1. At the command line, type the following command:

   ```
   openssl s_client -crlf -quiet -connect email-smtp.us-west-2.amazonaws.com:465 < input.txt
   ```
**Note**  
Replace *email\-smtp\.us\-west\-2\.amazonaws\.com* with the URL of the Amazon SES SMTP endpoint for your AWS Region\. For more information, see [[ERROR] BAD/MISSING LINK TEXT](regions.md)\.

   If the message was sent, you'll see output similar to the following:

   ```
   250 Ok 01010160d7de98d8-21e57d9a-JZho-416c-bbe1-8ebaAexample-000000
   ```

   The string of numbers and text that follows `250 Ok` is the message ID of the email\.
**Note**  
The connection may remain open after the message is sent\. If it does, it will automatically close after about 10 seconds of inactivity\.

------