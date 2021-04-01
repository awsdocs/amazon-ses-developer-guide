# Send an email by accessing the Amazon SES SMTP interface programmatically<a name="send-using-smtp-programmatically"></a>

You can access the Amazon SES SMTP interface by using an SMTP\-enabled programming language\. You provide the Amazon SES SMTP hostname and port number along with your SMTP credentials and then use the programming language's generic SMTP functions to send the email\.

**Note**  
Amazon Elastic Compute Cloud \(Amazon EC2\) restricts email traffic over port 25 by default\. To avoid timeouts when sending email through the SMTP endpoint from Amazon EC2, you can request that these restrictions be removed\. For more information, see [How do I remove the restriction on port 25 from my Amazon EC2 instance or AWS Lambda function?](https://aws.amazon.com/premiumsupport/knowledge-center/ec2-port-25-throttle/) in the AWS Knowledge Center\.  
The code examples in this section use port 587 to avoid this issue\.

**Topics**
+ [Send an email using SMTP with C\#](send-using-smtp-net.md)
+ [Send an email using SMTP with Java](send-using-smtp-java.md)
+ [Send an email using SMTP with PHP](send-using-smtp-php.md)
