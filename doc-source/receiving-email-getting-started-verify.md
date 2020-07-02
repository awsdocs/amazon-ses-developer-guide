# Step 2: Verify your domain<a name="receiving-email-getting-started-verify"></a>

Before you can configure Amazon SES to receive email for your domain, you must prove that you own the domain\. You can verify any domain that you own, but it is easier to verify domains that you registered using Route 53\.

**Note**  
If your account is still in the Amazon SES sandbox, complete the procedure in [Moving out of the Amazon SES sandbox](request-production-access.md) before you complete the procedure in this section\.

**To verify a domain with Amazon SES**

1. Open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.
**Note**  
To complete the procedure in this section, sign in to the AWS Management Console using the same AWS account you used when you registered your domain with Route 53\.

1. In the navigation pane, under **Identity Management**, choose **Domains**\.

1. Choose **Verify a New Domain**\.

1. On the **Verify a New Domain** dialog box, for **Domain**, type the name of the domain that you registered using Route 53, and then choose **Verify This Domain**\.

1. On the **Verify a New Domain** dialog box, choose **Use Route 53**\.
**Note**  
If you don't see the **Use Route 53** button, your domain may not be registered with Route 53\. If you used another service to register your domain, you can verify the domain by completing the procedures in [Verifying a domain with Amazon SES](verify-domain-procedure.md)\.

1. On the **Use Route 53** dialog box, select **Domain Verification Record** and **Email Receiving Record**\. Then, under **Hosted Zones**, select the name of the Hosted Zone you want to use\. If you haven't made any changes to the domain you registered using Route 53, there should only be one option available in the **Hosted Zones** section\.
**Important**  
If you've already set up mail exchanger \(MX\) records for your domain, the next step will replace those records with new ones\.

1. Choose **Create Record Sets**\. You'll return to the list of domains\.

1. Wait five minutes, and then choose the **refresh** \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/refresh_icon.png)\) button\. Confirm that the value in the **Status** column is **verified**\. If the status is **pending verification**, wait a few more minutes, and then refresh the list again\. Repeat this process until the domain's status is **verified**\.

Next step: [Step 3: Set up a receipt rule](receiving-email-getting-started-receipt-rule.md)