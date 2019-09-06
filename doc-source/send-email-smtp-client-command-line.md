# Testing Email Sending Using the Command Line<a name="send-email-smtp-client-command-line"></a>

You can interact with the Amazon SES SMTP interface from the command line by using widely\-available applications\. In most cases, you only use these methods to test your ability to connect to the SMTP interface\. However, you can also use these methods to write your own applications that send email using Amazon SES\.

This section includes procedures for testing connections and sending email using both OpenSSL \(which is included with most Linux, macOS, and Unix distributions\) and Windows PowerShell \(which is included with most recent versions of Windows\)\.

## Prerequisites<a name="send-email-smtp-client-command-line-prereqs"></a>

In order to connect to the Amazon SES SMTP interface using the command line, you first have to obtain your SMTP credentials\. For more information about obtaining your SMTP credentials, see [Obtaining Your Amazon SES SMTP Credentials](smtp-credentials.md)\.

**Important**  
Your SMTP credentials are different from your standard AWS credentials\. The two types of credentials aren't interchangeable\.

If you plan to use OpenSSL to connect to the SMTP interface, you have to encode your SMTP credentials using base64 encoding\.

**To encode your SMTP user name and password using base64**

1. At the command line, enter the following command to encode your SMTP user name, replacing `SMTPUsername` with your SMTP user name:

   ```
   echo -n "SMTPUsername" | openssl enc -base64
   ```

   Make a note of the output of this command\.

1. At the command line, enter the following command to encode your SMTP password, replacing `SMTPPassword` with your SMTP password:

   ```
   echo -n "SMTPPassword" | openssl enc -base64
   ```

   Make a note of the output of this command\.

## Testing Your Connection to the Amazon SES SMTP Interface<a name="send-email-smtp-client-command-line-testing"></a>

You can use the command line to test your connection to the Amazon SES SMTP interface without authenticating or sending any messages\. This procedure is useful for troubleshooting basic connectivity issues\.

This section includes procedures for testing your connection using both OpenSSL \(which is included with most Linux, macOS, and Unix distributions\) and Windows PowerShell \(which is included with most recent versions of Windows\)\.

------
#### [ Linux, macOS, or Unix ]

There are two ways to connect to the Amazon SES SMTP interface with OpenSSL: using Explicit SSL over port 587, or using Implicit SSL over port 465\.

**To connect to the SMTP interface using Explicit SSL**
+ At the command line, enter the following command to connect to the Amazon SES SMTP server:

  ```
  openssl s_client -crlf -quiet -starttls smtp -connect email-smtp.us-west-2.amazonaws.com:587
  ```

  In the preceding command, replace *email\-smtp\.us\-west\-2\.amazonaws\.com* with the URL of the Amazon SES SMTP endpoint for your AWS Region\. For more information, see [Regions and Amazon SES](regions.md)\.

  If the connection was successful, you see output similar to the following:

  ```
  depth=2 C = US, O = Amazon, CN = Amazon Root CA 1
  verify return:1
  depth=1 C = US, O = Amazon, OU = Server CA 1B, CN = Amazon
  verify return:1
  depth=0 CN = email-smtp.us-west-2.amazonaws.com
  verify return:1
  250 Ok
  ```

  The connection automatically closes after about 10 seconds of inactivity\.

Alternatively, you can use Implicit SSL to connect to the SMTP interface over port 465\.

**To connect to the SMTP interface using Implicit SSL**
+ At the command line, enter the following command to connect to the Amazon SES SMTP server:

  ```
  openssl s_client -crlf -quiet -connect email-smtp.us-west-2.amazonaws.com:465
  ```

  In the preceding command, replace *email\-smtp\.us\-west\-2\.amazonaws\.com* with the URL of the Amazon SES SMTP endpoint for your AWS Region\. For more information, see [Regions and Amazon SES](regions.md)\.

  If the connection was successful, you see output similar to the following:

  ```
  depth=2 C = US, O = Amazon, CN = Amazon Root CA 1
  verify return:1
  depth=1 C = US, O = Amazon, OU = Server CA 1B, CN = Amazon
  verify return:1
  depth=0 CN = email-smtp.us-west-2.amazonaws.com
  verify return:1
  220 email-smtp.amazonaws.com ESMTP SimpleEmailService-d-VCSHDP1YZ A1b2C3d4E5f6G7h8I9j0
  ```

  The connection automatically closes after about 10 seconds of inactivity\.

------
#### [ Windows PowerShell ]

You can use the `Test-NetConnection` cmdlet to connect to the Amazon SES SMTP server\.
+ In Windows PowerShell, enter the following command to connect to the Amazon SES SMTP server:

  ```
  Test-NetConnection -Port 587 -ComputerName email-smtp.us-west-2.amazonaws.com
  ```

  In the preceding command, replace *email\-smtp\.us\-west\-2\.amazonaws\.com* with the URL of the Amazon SES SMTP endpoint for your AWS Region\. For more information, see [Regions and Amazon SES](regions.md)\.

  If the connection was successful, you see output that resembles the following example:

  ```
  ComputerName     : email-smtp.us-west-2.amazonaws.com
  RemoteAddress    : 52.35.84.126
  RemotePort       : 587
  InterfaceAlias   : Ethernet
  SourceAddress    : 172.31.20.46
  TcpTestSucceeded : True
  ```

------

## Using the Command Line to Send Email Using the Amazon SES SMTP Interface<a name="send-email-using-openssl"></a>

You can also use the command line to send messages using the Amazon SES SMTP interface\. This procedure is useful for testing SMTP credentials and for testing the ability of specific recipients to receive messages that you send by using Amazon SES\.

This section includes procedures for sending email using both OpenSSL \(which is included with most Linux, macOS, and Unix distributions\) and Windows PowerShell \(which is included with most recent versions of Windows\)\.

------
#### [ Linux, macOS, or Unix ]

When an email sender connects to an SMTP server, the client issues a standard set of requests, and the server replies to each request with a standard response\. This series of requests and responses is called an *SMTP conversation*\. When you connect to the Amazon SES SMTP server using OpenSSL, the server expects an SMTP conversation to occur\.

In this example, you'll add all of the client requests to a text file, and then use that file as input to one of the OpenSSL commands listed in the previous section\.

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
   + Replace *ConfigSet* with the name of the configuration set that you want to use when you send this email\.

   When you finish, save the file as `input.txt`\.

1. At the command line, choose one of the following options:
   + **To send using Explicit SSL over port 587** – Enter the following command:

     ```
     openssl s_client -crlf -quiet -starttls smtp -connect email-smtp.us-west-2.amazonaws.com:587 < input.txt
     ```
   + **To send using Implicit SSL over port 465** – Enter the following command:

     ```
     openssl s_client -crlf -quiet -connect email-smtp.us-west-2.amazonaws.com:465 < input.txt
     ```
**Note**  
Replace *email\-smtp\.us\-west\-2\.amazonaws\.com* with the URL of the Amazon SES SMTP endpoint for your AWS Region\. For more information, see [Regions and Amazon SES](regions.md)\.

   If the message was accepted by Amazon SES, you see output that resembles the following example:

   ```
   250 Ok 01010160d7de98d8-21e57d9a-JZho-416c-bbe1-8ebaAexample-000000
   ```

   The string of numbers and text that follows `250 Ok` is the message ID of the email\.
**Note**  
The connection closes automatically after about 10 seconds of inactivity\.

------
#### [ Windows PowerShell ]

You can use the `Net.Mail.SmtpClient` class to send email through Windows PowerShell\.

**To send an email using Windows PowerShell using the SMTP interface**
+ In Windows PowerShell, enter the following command:

  ```
  $EmailFrom = "sender@example.com"
  $EmailTo = "recipient@example.com"
  $Subject = "Test email sent from Amazon SES"
  $Body = "This message was sent from Amazon SES using Windows PowerShell."
  $SMTPServer = "email-smtp.us-west-2.amazonaws.com"
  $SMTPClient = New-Object Net.Mail.SmtpClient($SmtpServer, 587)
  $SMTPClient.EnableSsl = $true
  $SMTPClient.Credentials = New-Object System.Net.NetworkCredential("SMTPUserName", "SMTPPassword");
  $SMTPClient.Send($EmailFrom, $EmailTo, $Subject, $Body)
  Remove-Variable -Name SMTPClient
  ```

  In the preceding command, make the following changes:
  + Replace *sender@example\.com* with the email address that you want to send the message from\.
  + Replace *recipient@example\.com* with the email address that you want to send the message to\.
  + Replace *email\-smtp\.us\-west\-2\.amazonaws\.com* with the URL of the Amazon SES SMTP endpoint for your AWS Region\. For more information, see [Regions and Amazon SES](regions.md)\.
  + Replace *SMTPUserName* with your SMTP user name\.
  + Replace *SMTPPassword* with your SMTP password\.

  If the message was accepted by Amazon SES, this command terminates without producing any output\.

------