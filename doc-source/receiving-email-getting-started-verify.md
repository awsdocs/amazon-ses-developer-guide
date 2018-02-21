# Step 2: Verify Your Domain<a name="receiving-email-getting-started-verify"></a>

Before you can receive email for a domain using Amazon SES, you must prove that you own the domain by verifying it with Amazon SES\. Although Amazon SES enables you to verify single email addresses, you must verify a domain if you want to use Amazon SES for email receiving\. You can verify and receive email with Amazon SES for any domain that you own, but it is easier to set up a domain that you have registered with Route 53\.

**To verify a domain with Amazon SES**

1. Sign in to the AWS Management Console using the AWS account that you used to register your domain with Route 53, and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the left navigation pane, under **Identity Management**, choose **Domains**\. Then, in the content pane, choose **Verify a New Domain**\.   
![\[Verifying a domain\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/getting_started_receiving_verify_1.png)

1. Enter the name of the domain that is registered in Route 53, leave **Generate DKIM Settings** unselected \(it is for email sending\), and then choose **Verify This Domain**\.  
![\[Verifying a domain\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/getting_started_receiving_verify_2.png)

1. On the **Verify a New Domain** page, which displays the records you must add to your DNS server, choose **Use Route 53**\.
**Note**  
If you do not see **Use Route 53**, then your domain is not registered with Route 53, which is a prerequisite for this tutorial\.  
![\[Verifying a domain\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/getting_started_receiving_verify_3.png)

1. On the **Use Route 53** page, choose **Domain Verification Record**, choose **Email Receiving Record**, and choose the hosted zone you want to use\.
**Important**  
If you have already set up mail exchanger \(MX\) records for your domain, the next step will replace your old MX records with new records\. MX records specify the mail server that you want to accept emails on behalf of your domain\.   
![\[Verifying a domain\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/getting_started_receiving_verify_4.png)

1. Choose **Create Record Sets**\. You will go back to the **Domain Identities** list\.

1. Wait a few minutes, and then refresh the **Domain Identities** list by using the refresh button near the top right of the content pane\. Confirm that the status of the domain is **verified**\.  
![\[Verifying a domain\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/getting_started_receiving_verify_5.png)

Next step: [Step 3: Set up a Receipt Rule](receiving-email-getting-started-receipt-rule.md)