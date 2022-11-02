# Virtual Deliverability Manager settings<a name="vdm-settings"></a>

You can view or change Virtual Deliverability Manager settings in your account at any time\. You can enable or disable Virtual Deliverability Manager, and can specify an on or off mode for engagement tracking and optimized shared delivery at the Virtual Deliverability Manager account level through the Amazon SES console or the AWS CLI

Virtual Deliverability Manager options are also provided at the configuration set level so you can define custom settings for how a configuration set will use engagement tracking and optimized shared delivery by overriding how they’ve been defined in Virtual Deliverability Manager\. This gives you the flexibility to tailor your email sending for specific email campaigns\. For example, you can enable engagement tracking and optimized shared delivery for your marketing email and disable them for your transactional email\.

## Changing your Virtual Deliverability Manager account settings using the Amazon SES console<a name="vdm-settings-console"></a>

The following procedure shows you how to change your Virtual Deliverability Manager account settings using the Amazon SES console\.

**To change your Virtual Deliverability Manager account settings using the Amazon SES console**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the left navigation pane, choose **Settings** under **Virtual Deliverability Manager**\.

   The **Virtual Deliverability Manager settings** page opens\. The **Subscription overview** panel indicates the status of Virtual Deliverability Manager and the **Additional settings** panel indicates the status of **Engagement tracking** and **Optimized shared delivery**\.

1. To change engagement tracking or optimized shared delivery settings:

   1. In the **Additional settings** panel, choose **Edit**\.

   1. Select the corresponding radio button to turn either feature on or off, and then choose **Submit settings**\.

      The **Virtual Deliverability Manager settings** page shows a summary of your changes in the **Additional settings** panel\.

1. \(Optional\) To define custom settings for how a configuration set uses engagement tracking and optimized shared delivery by overriding how they’re defined in Virtual Deliverability Manager, reference [Virtual Deliverability Manager options](creating-configuration-sets.md#vdm-create-config-overrides) while creating or editing a configuration set\.

1. To disable Virtual Deliverability Manager:

   1. In the **Subscription overview** panel, choose **Disable Virtual Deliverability Manager**\.

   1. In the **Disable Virtual Deliverability Manager?** pop\-up window, enter `Disable` in the confirmation field, and then choose **Disable Virtual Deliverability Manager**\.

   1. A banner appears, confirming that you've disabled Virtual Deliverability Manager\.

1. To reenable Virtual Deliverability Manager, see [Getting started with Virtual Deliverability Manager](vdm-get-started.md)\.

## Changing your Virtual Deliverability Manager account settings using the AWS CLI<a name="vdm-settings-cli"></a>

You can change your Virtual Deliverability Manager account settings using the AWS CLI\.

**To change your Virtual Deliverability Manager account settings using the AWS CLI**  
You can use the [https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_PutAccountVdmAttributes.html](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_PutAccountVdmAttributes.html) and [https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_PutConfigurationSetVdmOptions.html](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_PutConfigurationSetVdmOptions.html) operations in the Amazon SES API v2 to change your Virtual Deliverability Manager settings\. You can call this operation from the AWS CLI, as shown in the following examples\.
+ Enable or disable engagement tracking, optimized shared delivery, or both using an input file:

  ```
  aws --region us-east-1 sesv2 put-account-vdm-attributes --cli-input-json file://attributes.json
  ```

  In this example, where engagement tracking is `ENABLED` and optimized shared delivery is `DISABLED`, the input file looks similar to this:

  ```
  {
      "VdmAttributes": {
          "VdmEnabled": "ENABLED",
          "DashboardAttributes": {
              "EngagementMetrics": "ENABLED"
          },
          "GuardianAttributes": {
          }
              "OptimizedSharedDelivery": "DISABLED"
  }
  ```

  You can find more information about parameter values and related data typesby linking from the [https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_VdmAttributes.html](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_VdmAttributes.html) data type in the Amazon SES API v2 reference\.
+ Define custom settings for how a configuration set will use engagement tracking and optimized shared delivery by overriding how they’ve been defined in Virtual Deliverability Manager:

  ```
  aws --region us-east-1 sesv2 put-configuration-set-vdm-options --cli-input-json file://config-set.json
  ```

  In this example, where a configuration set named *example* has both engagement tracking and optimized shared delivery enabled, the input file looks similar to this:

  ```
  {
      "ConfigurationSetName": "example",
      "VdmOptions": {
          "DashboardOptions": {
              "EngagementMetrics": "ENABLED"
          },
          "GuardianOptions": {
              "OptimizedSharedDelivery": "ENABLED"
          }
      }
  }
  ```

  For more information about parameter values and related data types, see the [https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_VdmOptions.html](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_VdmOptions.html) data type in the Amazon SES API v2 reference\.
+ To verify the outcome:

  ```
  aws —region us-east-1 sesv2 get-configuration-set —configuration-set-name example
  ```
+ Not specifying [https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_DashboardOptions.html](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_DashboardOptions.html) or [https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_GuardianOptions.html](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_GuardianOptions.html) options at the configuration set level results in your Virtual Deliverability Manager account\-level settings applying to traffic sent through that configuration set\.