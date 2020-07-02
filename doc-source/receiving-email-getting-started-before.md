# Step 1: Before you begin<a name="receiving-email-getting-started-before"></a>

Before you start this tutorial, sign up for an AWS account \(if you don't already have one\), and use [Amazon Route 53](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/Welcome.html) to register the domain you want to use to receive email\.

**Note**  
Amazon SES only supports email receiving in certain AWS Regions\. For a complete list of Regions where email receiving is supported, see [Amazon Simple Email Service Endpoints and Quotas](https://docs.aws.amazon.com/general/latest/gr/ses) in the AWS General Reference\.

## Sign up for AWS<a name="receiving-email-getting-started-sign-up"></a>

If you already have an AWS account, you can skip this section\.

**To create an AWS account**

1. Go to [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/), and then choose **Get Started with Amazon SES**\.

1. On the **Create an AWS Account** page, complete the required fields and follow the on\-screen instructions to create a new account\.

## Register a domain using Route 53<a name="receiving-email-getting-started-domain"></a>

This tutorial assumes that you're using a domain that you registered using Route 53\. You can also use a domain that you registered using another service, but the procedures for verifying your domain will differ from those shown in this tutorial\. For more information about using Route 53 to register a domain, see [Register a New Domain](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/domain-register.html) in the *Amazon Route 53 Developer Guide*\.

You can also transfer an existing domain to Route 53\. For more information about transferring domains to Route 53, see [Transferring Registration for a Domain to Route 53](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/domain-transfer-to-route-53.html) in the *Amazon Route 53 Developer Guide*\.

Next step: [Step 2: Verify your domain](receiving-email-getting-started-verify.md)

## Request production access<a name="receiving-email-getting-started-sandbox"></a>

You can't set up email receiving if your account is still in the Amazon SES sandbox\. To have your account removed from the sandbox, complete the procedure in [Moving out of the Amazon SES sandbox](request-production-access.md)\.