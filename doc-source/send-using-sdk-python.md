# Send an email using the AWS SDK for Python \(Boto\)<a name="send-using-sdk-python"></a>

This topic shows how to use the [AWS SDK for Python \(Boto\)](https://aws.amazon.com/sdk-for-python/) to send an email through Amazon SES\. 

**Important**  
In this tutorial, you send an email to yourself so that you can check to see if you received it\. For further experimentation or load testing, use the Amazon SES mailbox simulator\. Emails that you send to the mailbox simulator do not count toward your sending quota or your bounce and complaint rates\. For more information, see [Testing email sending in Amazon SES](send-email-simulator.md)\.

## Prerequisites<a name="send-using-sdk-python-prerequisites"></a>

Before you begin, perform the following tasks:
+ **Verify your email address with Amazon SES**—Before you can send an email with Amazon SES, you must verify that you own the sender's email address\. If your account is still in the Amazon SES sandbox, you must also verify the recipient email address\. The easiest way to verify email addresses is by using the Amazon SES console\. For more information, see [Verifying email addresses in Amazon SES](verify-email-addresses.md)\. 
+ **Get your AWS credentials**—You need an AWS access key ID and AWS secret access key to access Amazon SES using an SDK\. You can find your credentials by using the [Security Credentials](https://console.aws.amazon.com/iam/home?#security_credential) page of the AWS Management Console\. For more information about credentials, see [Types of Amazon SES credentials](send-email-concepts-credentials.md)\.
+ **Install Python**—Python is available at [https://www\.python\.org/downloads/](https://www.python.org/downloads/)\. The code in this tutorial was tested using Python 2\.7\.6 and Python 3\.6\.1\. After you install Python, add the path to Python in your environment variables so that you can run Python from any command prompt\.
+ **Install the AWS SDK for Python \(Boto\)**—For download and installation instructions, see the [AWS SDK for Python \(Boto\) documentation](https://boto3.readthedocs.io/en/latest/guide/quickstart.html#installation)\. The sample code in this tutorial was tested using version 1\.4\.4 of the SDK for Python\.
+ **Create a shared credentials file**—For the sample code in this section to function properly, you must create a shared credentials file\. For more information, see [Create a shared credentials file](create-shared-credentials-file.md)\.

## Procedure<a name="send-using-sdk-python-procedure"></a>

The following procedure shows how to send an email through Amazon SES using the SDK for Python\.

**To send an email through Amazon SES using the SDK for Python**

1. In a text editor, create a file named `amazon-ses-sample.py`\. Paste the following code into the file:

   ```
    1. import boto3
    2. from botocore.exceptions import ClientError
    3. 
    4. # Replace sender@example.com with your "From" address.
    5. # This address must be verified with Amazon SES.
    6. SENDER = "Sender Name <sender@example.com>"
    7. 
    8. # Replace recipient@example.com with a "To" address. If your account 
    9. # is still in the sandbox, this address must be verified.
   10. RECIPIENT = "recipient@example.com"
   11. 
   12. # Specify a configuration set. If you do not want to use a configuration
   13. # set, comment the following variable, and the 
   14. # ConfigurationSetName=CONFIGURATION_SET argument below.
   15. CONFIGURATION_SET = "ConfigSet"
   16. 
   17. # If necessary, replace us-west-2 with the AWS Region you're using for Amazon SES.
   18. AWS_REGION = "us-west-2"
   19. 
   20. # The subject line for the email.
   21. SUBJECT = "Amazon SES Test (SDK for Python)"
   22. 
   23. # The email body for recipients with non-HTML email clients.
   24. BODY_TEXT = ("Amazon SES Test (Python)\r\n"
   25.              "This email was sent with Amazon SES using the "
   26.              "AWS SDK for Python (Boto)."
   27.             )
   28.             
   29. # The HTML body of the email.
   30. BODY_HTML = """<html>
   31. <head></head>
   32. <body>
   33.   <h1>Amazon SES Test (SDK for Python)</h1>
   34.   <p>This email was sent with
   35.     <a href='https://aws.amazon.com/ses/'>Amazon SES</a> using the
   36.     <a href='https://aws.amazon.com/sdk-for-python/'>
   37.       AWS SDK for Python (Boto)</a>.</p>
   38. </body>
   39. </html>
   40.             """            
   41. 
   42. # The character encoding for the email.
   43. CHARSET = "UTF-8"
   44. 
   45. # Create a new SES resource and specify a region.
   46. client = boto3.client('ses',region_name=AWS_REGION)
   47. 
   48. # Try to send the email.
   49. try:
   50.     #Provide the contents of the email.
   51.     response = client.send_email(
   52.         Destination={
   53.             'ToAddresses': [
   54.                 RECIPIENT,
   55.             ],
   56.         },
   57.         Message={
   58.             'Body': {
   59.                 'Html': {
   60.                     'Charset': CHARSET,
   61.                     'Data': BODY_HTML,
   62.                 },
   63.                 'Text': {
   64.                     'Charset': CHARSET,
   65.                     'Data': BODY_TEXT,
   66.                 },
   67.             },
   68.             'Subject': {
   69.                 'Charset': CHARSET,
   70.                 'Data': SUBJECT,
   71.             },
   72.         },
   73.         Source=SENDER,
   74.         # If you are not using a configuration set, comment or delete the
   75.         # following line
   76.         ConfigurationSetName=CONFIGURATION_SET,
   77.     )
   78. # Display an error if something goes wrong.	
   79. except ClientError as e:
   80.     print(e.response['Error']['Message'])
   81. else:
   82.     print("Email sent! Message ID:"),
   83.     print(response['MessageId'])
   ```

1. In `amazon-ses-sample.py`, replace the following with your own values:
   + **`sender@example.com`**—Replace with an email address that you have verified with Amazon SES\. For more information, see [Verifying identities](verify-addresses-and-domains.md)\. Email addresses in Amazon SES are case\-sensitive\. Make sure that the address you enter is exactly the same as the one you verified\.
   + **`recipient@example.com`**—Replace with the address of the recipient\. If your account is still in the sandbox, you must verify this address before you use it\. For more information, see [Moving out of the Amazon SES sandbox](request-production-access.md)\. Make sure that the address you enter is exactly the same as the one you verified\.
   + **\(Optional\) `us-west-2`**—If you want to use Amazon SES in a region other than US West \(Oregon\), replace this with the region you want to use\. For a list of regions where Amazon SES is available, see [Amazon Simple Email Service \(Amazon SES\)](https://docs.aws.amazon.com/general/latest/gr/rande.html#ses_region) in the *AWS General Reference*\.

1. Save `amazon-ses-sample.py`\.

1. To run the program, open a command prompt in the same directory as `amazon-ses-sample.py`, and then type python amazon\-ses\-sample\.py

1. Review the output\. If the email was successfully sent, the console displays "Email sent\!" Otherwise, it displays an error message\.

1. Sign in to the email client of the recipient address\. You will find the message that you sent\.
