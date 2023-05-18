# Configuring custom domains to handle open and click tracking<a name="configure-custom-open-click-domains"></a>

When you use [event publishing](monitor-using-event-publishing.md) to capture open and click events, Amazon SES makes minor changes to the emails you send\. To capture open events, SES adds a 1 pixel by 1 pixel transparent GIF image in each email sent through SES which includes a unique file name for each email, and is hosted on a server operated by SES; when the image is downloaded, SES can tell exactly which message was opened and by whom\.

By default, this pixel is inserted at the bottom of the email; however, some email providers’ applications truncate the preview of an email when it exceeds a certain size and may provide a link to view the remainder of the message\. In this scenario, the SES pixel tracking image does not load and will throw off the open rates you’re trying to track\. To get around this, you can optionally place the pixel at the beginning of the email, or anywhere else, by inserting the `{{ses:openTracker}}` placeholder into the email body\. Once SES receives the message with the placeholder, it will be replaced with open tracking pixel image\. Just add one placeholder, as only the first occurrence will be replaced, any remaining will be omitted\.

To capture link click events, Amazon SES replaces the links in your emails with links to a server operated by SES\. This immediately redirects the recipient to their intended destination\.

You also have the option to use your own domains, rather than domains owned and operated by Amazon SES, to create a more cohesive experience for your recipients, meaning all SES indicators are removed\. You can configure multiple custom domains to handle open and click tracking events\. These custom domains are associated with configuration sets\. When you send an email using a configuration set, if that configuration set is configured to use a custom domain, then the open and click links in that email automatically use the custom domain specified in that configuration set\.

This section contains procedures for setting up a subdomain on a server you own to automatically redirect users to the open and click tracking servers operated by Amazon SES\. There are three steps involved in setting up these domains\. First, you configure the subdomain itself, then set a configuration set to use the custom domain, and then set its event destination to publish open and click events\. This topic contains procedures for completing all of these steps\.

However, if you simply want to enable open or click tracking without setting up a custom domain, you can proceed directly to defining event destinations for your configuration set which enables event publishing that is triggered on the event types you specify, including open and click events\. A configuration set can have multiple event destinations with multiple event types defined\. See [Creating Amazon SES event destinations](event-destinations-manage.md)\.

## Part 1: Setting up a domain for handling open and click link redirects<a name="configure-custom-open-click-domain"></a>

The specific procedures for setting up a redirect domain vary depending on your web hosting provider \(and your Content Delivery Network, if you use an HTTPS server\)\. The procedures in the following sections provide general guidance rather than specific steps\.

### Option 1: Configuring an HTTP domain<a name="configure-custom-open-click-domain-http"></a>

If you plan to use an HTTP domain to handle open and click links \(as opposed to an HTTPS domain\), the process for configuring the subdomain involves only a few steps\.

**Note**  
If you set up a custom domain that uses the HTTP protocol, and you send an email that contains links that use the HTTPS protocol, your customers may see a warning message when they click the links in your email\. If you plan to send emails that contain links that use the HTTPS protocol, you should use an HTTPS domain for handling click tracking events\.

**To set up an HTTP subdomain for handling open and click links**

1. If you have not already done so, create a subdomain to use for open and click tracking links\. We recommend that you create a subdomain that is specifically dedicated to handling these links\.

1. Verify the subdomain for use with Amazon SES\. For more information, see [Creating a domain identity](creating-identities.md#verify-domain-procedure)\.

1. Modify the DNS record for the subdomain\. In the DNS record, add a new CNAME record that redirects requests to the Amazon SES tracking domain\. The address that you redirect to depends on the AWS Region that you use Amazon SES in\. The following table contains a list of tracking domains for the AWS Regions where Amazon SES is available\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/ses/latest/dg/configure-custom-open-click-domains.html)
**Note**  
Depending on your web hosting provider, it may take several minutes for the changes you make to the subdomain's DNS record to take effect\. Your web hosting provider or IT organization can provide additional information about these delays\.

### Option 2: Configuring an HTTPS domain<a name="configure-custom-open-click-domain-https"></a>

You can only use an HTTPS domain for tracking link clicks\. To set up an HTTPS domain for tracking link clicks, you have to perform some additional steps, beyond those required for [setting up an HTTP domain](#configure-custom-open-click-domain-http)\.

**Note**  
You can only use an HTTPS domain for tracking link clicks\. Amazon SES only supports open tracking over HTTP domains when using a custom domain; otherwise, SES supports open tracking over HTTPS when a custom domain is not defined which will implicitly use domains owned and operated by SES\.

**To set up an HTTPS subdomain for handling click links**

1. Create a subdomain to use for click tracking links\. We recommend that you create a subdomain that is specifically dedicated to handling these links\. 

1. Verify the subdomain for use with Amazon SES\. For more information, see [Creating a domain identity](creating-identities.md#verify-domain-procedure)\.

1. Create a new account with a Content Delivery Network \(CDN\), such as [Amazon CloudFront](https://aws.amazon.com/cloudfront)\.

1. Configure the CDN to forward requests to the Amazon SES tracking domain\. The address that you redirect to depends on the AWS Region that you use Amazon SES in\. The following table contains a list of tracking domains for the AWS Regions where Amazon SES is available\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/ses/latest/dg/configure-custom-open-click-domains.html)

1. If you use Amazon CloudFront as your CDN, complete the following procedures:

   1.  On the **CloudFront Distributions** page, choose the distribution that corresponds with your CDN\.

   1. On the **Behaviors** tab, choose the default behavior, and then choose **Edit**\.

   1. For **Cache Based on Selected Request Headers**, choose **All**\.

   1. For **Query String Forwarding and Caching**, choose **Forward all, cache based on all**\.

   1. Add an alternate domain name to your distribution\. The subdomain that you use has to be verified in Amazon SES\. For more information, see [Configuring Alternate Domain Names and HTTPS](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/cnames-and-https-procedures.html) in the *Amazon CloudFront Developer Guide*\.

   If you use a CDN other than CloudFront, you might need to complete similar steps\. For more information, refer to the documentation for your CDN\.

1. If you use Route 53 to manage the DNS configuration for your domain and CloudFront as your CDN, create an Alias record in Route 53 that refers to your CloudFront distribution \(such as *d111111abcdef8\.cloudfront\.net*\)\. For more information, see [Creating Records by Using the Amazon Route 53 Console](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resource-record-sets-creating.html) in the *Amazon Route 53 Developer Guide*\.

   Otherwise, in the DNS configuration for your subdomain, add a CNAME record that refers to the address of your CDN\.

1. Acquire an SSL certificate from a trusted Certificate Authority\. The certificate should cover both the subdomain you created in step 1 as well as the CDN you configured in steps 3–5\. Upload the certificate to the CDN\.

## Part 2: Setting up a configuration set to refer to a custom open and click tracking domain<a name="configure-custom-open-click-domain-config-set"></a>

After you configure your domain to handle open and click tracking redirects, you must specify your custom domain in the configuration set\.  You can complete this using the Amazon SES console or the `CreateConfigurationSetTrackingOptions` API operation\.

This section references the procedures for completing these tasks using the Amazon SES console\. For information about using the API, see [CreateConfigurationSetTrackingOptions](https://docs.aws.amazon.com/ses/latest/APIReference/API_CreateConfigurationSetTrackingOptions.html) in the [Amazon Simple Email Service API Reference](https://docs.aws.amazon.com/ses/latest/APIReference/)\.
+ **To specify a custom redirect domain using the console**\.\.\.
  + **while creating a new configuration set** – see [Tracking options](creating-configuration-sets.md#create-config-set-step-4) in Step 4 of [Create configuration sets](creating-configuration-sets.md)
  + **while modifying an existing configuration set** – select the **Edit** button in the **General details** panel of the selected configuration set, and follow the directions for [Tracking options](creating-configuration-sets.md#create-config-set-step-4) in Step 4 of [Create configuration sets](creating-configuration-sets.md)

## Part 3: Selecting open and click event types in your configuration set's event destinations<a name="configure-open-click-event-types"></a>

After specifying your custom domain in the configuration set, you must select open and/or click event types in an event destination added to your configuration set\. You can complete this using the Amazon SES console or the [https://docs.aws.amazon.com/ses/latest/APIReference/API_CreateConfigurationSetEventDestination.html](https://docs.aws.amazon.com/ses/latest/APIReference/API_CreateConfigurationSetEventDestination.html) API operation\.
+ **To select open and/or click event types using the console**\.\.\.
  + **while creating a new event destination** – see [Open and click tracking](event-destinations-manage.md#select-event-types-step) in Step 6 of [Creating an event destination](event-destinations-manage.md#event-destination-add)\. 
  + **while modifying an existing event destination** – select the **Edit** button in the **Event types** panel of the selected event destination in Step 6 of  [Editing, disabling/enabling, or deleting an event destination](event-destinations-manage.md#event-destination-edit)