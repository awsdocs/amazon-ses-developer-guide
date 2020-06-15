# Send an email using the Amazon SES console<a name="send-an-email-from-console"></a>

The easiest way to send an email with Amazon SES is to use the Amazon SES console\. Because the console requires you to manually enter information, you typically only use it to send test emails\. After you get started with Amazon SES, you will most likely send your emails using either the Amazon SES SMTP interface or API, but the console is useful for monitoring your sending activity\.

**Important**  
In this getting started tutorial, you send an email to yourself so that you can check to see if you received it\. For further experimentation or load testing, use the Amazon SES mailbox simulator\. Emails that you send to the mailbox simulator do not count toward your sending quota or your bounce and complaint rates\. For more information, see [Testing Email Sending in Amazon SES](send-email-simulator.md)\.

Before you follow these steps, make sure you review the setup instructions in [Before you begin with Amazon SES](send-email-getting-started-prerequisites.md)\.

**To send an email message from the Amazon SES console**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.
**Note**  
If you are not currently signed in to your AWS account, this link takes you to a sign\-in page\. After you sign in, you are directed to the Amazon SES console\. 

1. In the navigation pane on the left side of the Amazon SES console, under **Identity Management**, choose **Email Addresses** to view the email address that you verified in [Verifying email addresses in Amazon SES](verify-email-addresses.md)\.

1. In the list of identities, check the box next to email address that you have verified\.

1. Choose **Send a Test Email**\.

1. For **Send Test Email**, choose the **Email Format**\. The two choices are as follows:
   + ****Formatted****—This is the simplest option\. Choose this option if you simply want to type the text of your message into the **Body** text box\. When you send the email, Amazon SES puts the text into email format for you\.
   + ****Raw****—Choose this option if you want to send a more complex message, such as a message that includes HTML or an attachment\. Because of this flexibility, you need to format the message, as described in [Sending Raw Email Using the Amazon SES API](send-email-raw.md), yourself, and then paste the entire formatted message, including the headers, into the **Body** text box\. You can use the following example, which contains HTML, to send a test email using the **Raw** email format\. Copy and paste this message in its entirety into the **Body** text box\. Ensure that there is not a blank line between the `MIME-Version` header and the `Content-Type` header; a blank line between these two lines causes the email to be formatted as plain text instead of HTML\.

     ```
      1. Subject: Amazon SES Raw Email Test
      2. MIME-Version: 1.0
      3. Content-Type: text/html
      4. 
      5. <!DOCTYPE html>
      6. <html>
      7. <body>
      8. <h1>This text should be large, because it is formatted as a header in HTML.</h1>
      9. <p>Here is a formatted link: <a href="https://docs.aws.amazon.com/ses/latest/DeveloperGuide/Welcome.html">Amazon Simple Email Service Developer Guide</a>.</p>
     10. </body>
     11. </html>
     ```

1. For **Send Test Email**, fill out the rest of the fields\. If you are still in the Amazon SES sandbox, make sure that the address in the **To** field is a verified email address\. For more information, see [Verifying email addresses in Amazon SES](verify-email-addresses.md)\. 

1. Choose **Send Test Email**\.

1. Sign in to the email client of the address you sent the email to\. You will find the message that you sent\.