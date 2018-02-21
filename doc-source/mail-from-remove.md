# Removing a MAIL FROM Domain with Amazon SES<a name="mail-from-remove"></a>

If you want to use the default Amazon SES MAIL FROM domain, you can remove the custom MAIL FROM domain configuration from the verified identity\.

The following procedures show how to use the Amazon SES console to remove a custom MAIL FROM domain from the configuration of a verified email address or domain\. If you want to use the Amazon SES API instead, see the `SetIdentityMailFromDomain` API in the [Amazon Simple Email Service API Reference](http://docs.aws.amazon.com/ses/latest/APIReference/)\.

**To remove a custom MAIL FROM domain from the configuration of a verified email address**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane, under **Identity Management**, choose **Email Addresses**\.

1. In the verified email address list, choose the verified email address for which you want to remove the custom MAIL FROM domain\.

1. In the details pane of the verified email address, expand **MAIL FROM Domain**\.

1. Choose **Remove MAIL FROM Domain**\.

1. Choose **Yes, Remove MAIL FROM Domain**\.

1. \(Optional\) Log in to your DNS service and remove the MX record that you published when you set up the MAIL FROM domain with Amazon SES\.

1. \(Optional\) Remove the SPF record that you published when you set up the custom MAIL FROM domain with Amazon SES\.

**To remove a custom MAIL FROM domain from the configuration of a verified domain**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane, under **Identity Management**, choose **Domains**\.

1. In the verified domain list, choose the verified domain for which you want to remove the custom MAIL FROM domain\.

1. In the details pane of the verified domain, expand **MAIL FROM Domain**\.

1. Choose **Remove MAIL FROM Domain**\.

1. Choose **Yes, Remove MAIL FROM Domain**\.

1. \(Optional\) Log in to your DNS service and remove the MX record that you published when you set up the MAIL FROM domain with Amazon SES\.

1. \(Optional\) Remove the SPF record that you published when you set up the custom MAIL FROM domain with Amazon SES\.