# Step 5: View the received email<a name="receiving-email-getting-started-view"></a>

After you send a test message to an address on your domain, you can retrieve it from your Amazon S3 bucket and view its contents\.

**To view a message that you received through Amazon SES**

1. Open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. In the Amazon S3 console, choose the bucket you created in [Step 3: Set up a receipt rule](receiving-email-getting-started-receipt-rule.md)\.

1. In the Amazon S3 bucket, find the email you received\. The name of the email is a unique string of letters and numbers\.
**Note**  
The bucket may also contain a file named `AMAZON_SES_SETUP_NOTIFICATION`\. You can ignore or delete this file\.

1. Select the check box next to the name of the file\. On the **Actions** menu, choose **Download**\.

1. Open the folder on your computer that contains the file you downloaded in the preceding step\. There are several ways to view the downloaded message, including the following:
   + Open the file in a text editor and read its contents directly\. Depending on the method you used to send the email, part of the message may be encoded\. If part of the message is encoded, you'll need to decode them manually \(for example, by using a base64 decoder\)\. 
   + Add the `.eml` extension to the end of the file name, and then open the file using an email client such as Microsoft Outlook or Mozilla Thunderbird\. Most email clients will automatically decode the encoded parts of a message, and will display things like HTML formatting and file attachments\.

Next step: [Step 6: Clean up](receiving-email-getting-started-clean.md)
