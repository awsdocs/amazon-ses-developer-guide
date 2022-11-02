# Getting started with Virtual Deliverability Manager<a name="vdm-get-started"></a>

To start using Virtual Deliverability Manager with your account, you must enable it using the onboarding wizard in the Amazon SES console, where you'll set up *engagement tracking* and *optimized shared delivery*\. Virtual Deliverability Manager uses engagement tracking and optimized shared delivery to monitor your sending and to help you make improvements to your deliverability and reputation\.
+ **Engagement tracking** – The ability to monitor recipient engagement behavior through open and click events by using a tracking pixel within a wrapped link\. When triggered, the tracking pixel provides a timestamp of when a message was opened, and indicates which links were  clicked by the recipient\. *Turning this on alters your URLs and links to include Amazon SES engagement tracking wrappers\.*
+ **Optimized shared delivery** – Automatically chooses the optimal IP to use when sending emails, improving end\-point delivery of messages to the target email recipients\. *This does not apply to dedicated IP addresses\.*

While both engagement tracking and optimized shared delivery are turned on by default in the onboarding wizard, you have the option to turn them off\. We highly recommend that you keeping both features enabled to get the most out of Virtual Deliverability Manager\.

## Getting started with Virtual Deliverability Manager using the Amazon SES console<a name="vdm-get-started-console"></a>

The following procedure shows you how to get started with Virtual Deliverability Manager using the Amazon SES console\.

**To get started with Virtual Deliverability Manager using the Amazon SES console**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the left navigation pane, choose **Virtual Deliverability Manager**\.

1. Choose any of the **Get started with Virtual Deliverability Manager** buttons on the **Virtual Deliverability Manager overview** page\.

1. On the **Select Engagement tracking** page, accept the default or choose **Turn off engagement tracking**, then choose **Next**\.
**Note**  
Turning on engagement tracking alters your URLs and links to include Amazon SES engagement tracking wrappers\.

1. On the **Select Optimized shared delivery** page, accept the default or choose **Turn off optimized shared delivery**, then choose **Next**\.
**Important**  
Optimized shared delivery might result in preemptive delays to your emails being sent in an attempt to protect your sending reputation\. If you have a critical workload that must be sent without delay, we recommend that you don't enable this setting\. Instead, use configuration sets for sending, and only enable optimized shared delivery for those configuration sets where you can afford delays\.

1. Review your choices for engagement tracking and optimized shared delivery on the **Review and enable** page\. Choose **Previous** if you want to go back and make changes; otherwise, choose **Enable Virtual Deliverability Manager**\.

   The **Virtual Deliverability Manager settings** page opens\. The **Subscription overview** panel indicates the status of Virtual Deliverability Manager and the **Additional settings** panel indicates the status of **Engagement tracking** and **Optimized shared delivery**\.

Once you've enabled Virtual Deliverability Manager for your account, you can define custom settings for how a configuration set will use engagement tracking and optimized shared delivery by overriding how they’ve been defined in Virtual Deliverability Manager\. This gives you the flexibility to tailor your email sending for specific email campaigns\. For example, you can enable engagement tracking and optimized shared delivery for your marketing email and disable them for your transactional email\. See [Virtual Deliverability Manager options](creating-configuration-sets.md#vdm-create-config-overrides) while creating or editing a configuration set\.

## Getting started with Virtual Deliverability Manager using the AWS CLI<a name="vdm-get-started-cli"></a>

The following examples show you how to get started with Virtual Deliverability Manager using the AWS CLI\.

**To get started with Virtual Deliverability Manager using the AWS CLI**  
You can use the [https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_PutAccountVdmAttributes.html](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_PutAccountVdmAttributes.html) operation in the Amazon SES API v2 to get started with Virtual Deliverability Manager\. You can call this operation from the AWS CLI, as shown in the following examples\.
+ Enable Virtual Deliverability Manager in your account:

  ```
  aws --region us-east-1 sesv2 put-account-vdm-attributes --vdm-attributes VdmEnabled=ENABLED
  ```
+ Enable both engagement tracking and optimized shared delivery using an input file:

  ```
  aws --region us-east-1 sesv2 put-account-vdm-attributes --cli-input-json file://attributes.json
  ```

  The input file looks similar to this:

  ```
  {
      "VdmAttributes": {
          "VdmEnabled": "ENABLED",
          "DashboardAttributes": {
              "EngagementMetrics": "ENABLED"
          },
          "GuardianAttributes": {
          }
              "OptimizedSharedDelivery": "ENABLED"
  }
  ```

  Parameter values and related data types can be found by linking from the [https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_VdmAttributes.html](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_VdmAttributes.html) data type in the Amazon SES API v2 reference\.
**Note**  
Turning on engagement tracking alters your URLs and links to include Amazon SES engagement tracking wrappers\.
**Important**  
Optimized shared delivery might result in preemptive delays to your emails being sent in an attempt to protect your sending reputation\. If you have a critical workload that must be sent without delay, we recommend that you don't enable this setting\. Instead, use configuration sets for sending, and only enable optimized shared delivery for those configuration sets where you can afford delays\.
+ To verify the outcome:

  ```
  aws --region us-east-1 sesv2 get-account
  ```
+ To define custom settings for how a configuration set will use engagement tracking and optimized shared delivery by overriding how they’ve been defined in Virtual Deliverability Manager, see the AWS CLI example in [Virtual Deliverability Manager settings](vdm-settings.md)\.