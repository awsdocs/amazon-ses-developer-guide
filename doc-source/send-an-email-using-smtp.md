# Send an email through Amazon SES using SMTP<a name="send-an-email-using-smtp"></a>

To send an email using the Amazon SES SMTP interface, you can use an SMTP\-enabled programming language, email server, or application\. Before you start, review the instructions in [Before you begin with Amazon SES](send-email-getting-started-prerequisites.md)\. You also need to get the following additional information: 
+ Your Amazon SES SMTP username and password, which enable you to connect to the Amazon SES SMTP endpoint\. To get your Amazon SES SMTP username and password, see [Obtaining your Amazon SES SMTP credentials](smtp-credentials.md)\. 
**Important**  
Your SMTP credentials are different from your AWS credentials\. For more information about credentials, see [Types of Amazon SES credentials](send-email-concepts-credentials.md)\.
+ The SMTP endpoint address\. For a list of Amazon SES SMTP endpoints, see [Connecting to an Amazon SES SMTP endpoint](smtp-connect.md)\.
+ The Amazon SES SMTP interface port number, which depends on the connection method\. For more information, see [Connecting to an Amazon SES SMTP endpoint](smtp-connect.md)\.

After you get your SMTP credentials, you can connect to the Amazon SES SMTP endpoint and send email\. This getting started tutorial shows you how to send email through the Amazon SES SMTP interface by using the following methods:
+ [Send an email by accessing the Amazon SES SMTP interface programmatically](send-using-smtp-programmatically.md)
+ [Configuring Your Existing Email Server or SMTP\-Enabled Application to Send Email Through Amazon SES](send-using-smtp-integrate.md)

For more information about the Amazon SES SMTP interface, see [Using the Amazon SES SMTP interface to send email](send-email-smtp.md)\. 