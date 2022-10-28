# Migrating to Amazon SES from another email\-sending solution<a name="send-email-getting-started-migrate"></a>

This topic provides an overview of the steps that you have to take if you want to move your email\-sending solution to Amazon SES from a solution that's hosted on\-premises, or from one hosted on an Amazon EC2 instance\.

**Topics**
+ [Step 1\. Verify your domain](#send-email-getting-started-migrate-verify-domain)
+ [Step 2\. Request production access](#send-email-getting-started-migrate-request-production-access)
+ [Step 3\. Configure domain authentication systems](#send-email-getting-started-migrate-configure-authentication)
+ [Step 4\. Generate your SMTP credentials](#send-email-getting-started-migrate-generate-credentials)
+ [Step 5\. Connect to an SMTP endpoint](#send-email-getting-started-migrate-connect-to-endpoint)
+ [Next steps](#send-email-getting-started-migrate-next-steps)

## Step 1\. Verify your domain<a name="send-email-getting-started-migrate-verify-domain"></a>

Before you can use Amazon SES to send email, you have to verify the identities that you plan to send email from\. In Amazon SES, an identity can be an email address or an entire domain\. When you verify a domain, you can use Amazon SES to send email from any address on that domain\. For more information about verifying a domain, see [Creating a domain identity](creating-identities.md#verify-domain-procedure)\.

## Step 2\. Request production access<a name="send-email-getting-started-migrate-request-production-access"></a>

When you first start using Amazon SES, your account is in a sandbox environment\. While your account is in the sandbox, you can only send email to addresses that you've verified\. Additionally, there are restrictions on the number of messages that you can send per day, and the number that you can send per second\. For more information about requesting production access, see [Moving out of the Amazon SES sandbox](request-production-access.md)\.

## Step 3\. Configure domain authentication systems<a name="send-email-getting-started-migrate-configure-authentication"></a>

You can configure your domain to use authentication systems such as DKIM and SPF\. This step is technically optional\. However, by setting up either DKIM or SPF \(or both\) for your domain, you can improve the deliverability of your emails, and increase the amount of trust that your customers have in you\. For more information about setting up SPF, see [Authenticating Email with SPF in Amazon SES](send-email-authentication-spf.md)\. For more information about setting up DKIM, see [Authenticating Email with DKIM in Amazon SES](send-email-authentication-dkim.md)\.

## Step 4\. Generate your SMTP credentials<a name="send-email-getting-started-migrate-generate-credentials"></a>

If you plan to send email using an application that uses SMTP, you have to generate SMTP credentials\. Your SMTP credentials are different from your regular AWS credentials\. These credentials are also unique in each AWS Region\. For more information about generating your SMTP credentials, see [Obtaining Amazon SES SMTP credentials](smtp-credentials.md)\. 

## Step 5\. Connect to an SMTP endpoint<a name="send-email-getting-started-migrate-connect-to-endpoint"></a>

If you use a message transfer agent such as postfix or sendmail, you have to update the configuration for that application to refer to an Amazon SES SMTP endpoint\. For a complete list of SMTP endpoints, see [Connecting to an Amazon SES SMTP endpoint](smtp-connect.md)\. Note that the SMTP credentials that you created in the previous step are associated with a specific AWS Region\. You have to connect to the SMTP endpoint in the region that you created the SMTP credentials in\.

## Next steps<a name="send-email-getting-started-migrate-next-steps"></a>

At this point, you're ready to start sending email using Amazon SES\. However, there are a few optional steps that you can take\.
+ You can create configuration sets, which are sets of rules that are applied to the emails that you send\. For example, you can use configuration sets to specify where notifications are sent when an email is delivered, when a recipient opens a message or clicks a link in it, when an email bounces, and when a recipient marks your email as spam\. For more information, see [Using configuration sets in Amazon SES](using-configuration-sets.md)\.
+ When you send email through Amazon SES, it's important to monitor the bounces and complaints for your account\. Amazon SES includes a reputation metrics console page that you can use to keep track of the bounces and complaints for your account\. For more information, see [Using reputation metrics to track bounce and complaint rates](reputation-dashboard-dg.md)\. You can also create CloudWatch alarms that alert you when these rates get too high\. For more information about creating CloudWatch alarms, see [Creating reputation monitoring alarms using CloudWatch](reputationdashboard-cloudwatch-alarm.md)\.
+ Customers who send a large volume of email, or those who simply want to have full control over the reputations of their IP addresses, can lease dedicated IP addresses for an additional monthly charge\. For more information, see [Dedicated IP addresses for Amazon SES](dedicated-ip.md)\.