# Send an Email Through Amazon SES Using SMTP<a name="send-an-email-using-smtp"></a>

To send an email using the Amazon SES SMTP interface, you can use an SMTP\-enabled programming language, email server, or application\. Before you start, review the instructions in [Before You Begin with Amazon SES](before-you-begin.md)\. You also need to get the following additional information: 
+ Your Amazon SES SMTP username and password, which enable you to connect to the Amazon SES SMTP endpoint\. To get your Amazon SES SMTP username and password, see [Obtaining Your Amazon SES SMTP Credentials](smtp-credentials.md)\. 
**Important**  
Your SMTP credentials are different from your AWS credentials\. For more information about credentials, see [Using Credentials With Amazon SES](using-credentials.md)\.
+ The Amazon SES SMTP hostname, which is *email\-smtp\.us\-east\-1\.amazonaws\.com* \(for Region us\-east\-1\), *email\-smtp\.us\-west\-2\.amazonaws\.com* \(for Region us\-west\-2\), or *email\-smtp\.eu\-west\-1\.amazonaws\.com* \(for Region eu\-west\-1\)\.
+ The Amazon SES SMTP interface port number, which depends on the connection method\. For more information, see [Connecting to the Amazon SES SMTP Endpoint](smtp-connect.md)\.

After you get your SMTP credentials, you can connect to the Amazon SES SMTP endpoint and send email\. This getting started tutorial shows you how to send email through the Amazon SES SMTP interface by using the following methods:
+ [Send an Email by Accessing the Amazon SES SMTP Interface Programmatically](send-using-smtp-programmatically.md)
+ [Configuring Your Existing Email Server or SMTP\-Enabled Application to Send Email Through Amazon SES](send-using-smtp-integrate.md)

For more information about the Amazon SES SMTP interface, see [Using the Amazon SES SMTP Interface to Send Email](send-email-smtp.md)\. 