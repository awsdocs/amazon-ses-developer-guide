# Creating configuration sets in Amazon SES<a name="creating-configuration-sets"></a>

You can use the Amazon SES console, the `CreateConfigurationSet` action in the Amazon SES API v2, or the aws sesv2 create\-configuration\-set command in the Amazon SES CLI v2 to create a new configuration set\. This section shows how to create configuration sets using the Amazon SES console and the Amazon SES CLI v2\.

## Create a configuration set \(console\)<a name="config-sets-create-console"></a>

To create a configuration set using the Amazon SES console, follow these steps:

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane, under **Configuration**, choose **Configuration sets**\.

1. Choose **Create set**\.

1. <a name="create-config-set-step-4"></a>Enter the following details in the **General details** section:
   + **Configuration set name** – the name for your configuration set\. The name can contain up to 64 alphanumeric characters, including letters, numbers, hyphens \(\-\) and underscores \(\_\) only\.
   + **Sending IP pool** – when you send email using this configuration set, messages are sent from the dedicated IP addresses in the assigned pool\. Select an IP pool from the list\.
**Note**  
The **default** \(ses\-default\-dedicated\-pool\) contains dedicated IP addresses that haven't been assigned to any other pool\. To learn more about managing IP pools, see [Assign IP pools](managing-ip-pools.md)\.
   + **Tracking options** – select the **Use a custom redirect domain** check box to use a custom redirect domain to handle open and click tracking for this configuration set, instead of using one of the Amazon SES domains\.
     + **Custom redirect domain** – with a custom redirect domain, you can enter a custom subdomain in the box \(optional\), or select a verified domain from the list\.
**Note**  
Custom redirect domains can be specified as follows:  
Redirect domains must be set up prior to choosing this option\. For instructions on selecting a custom domain for handling open and click tracking, see [Configuring custom domains to handle open and click tracking](configure-custom-open-click-domains.md)\.
Then, to choose to use a custom redirect domain, you must indicate it while creating your configuration set, or at a later time by editing your tracking options for the configuration set\.
   + **Advanced delivery options** – choose the arrow on the left to expand the advanced delivery options section\.
     + **Transport Layer Security \(TLS\)** – to require Amazon SES to establish a secure connection with the receiving mail server, and send emails using the TLS protocol, select the **Required** check box\.
**Note**  
Amazon SES supports TLS 1\.2, TLS 1\.1, and TLS 1\.0\. To learn more, see [Security in Amazon Simple Email Service](security.md)\.

1. Enter the following details in the **Reputation options** section:
   + **Reputation metrics** – used to track bounce and complaint metrics in CloudWatch for emails sent using this configuration set\. Additional charges apply, see [Amazon CloudWatch pricing](http://aws.amazon.com/cloudwatch/pricing) for more information\.
     + **Enabled** – select this check box to enable reputation metrics for the configuration set\.

1. <a name="suppression-list-config-set-level"></a>The **Suppression list options** section provides a decision set to define customized suppression starting with the option to use this configuration set to override your account\-level suppression\. The [configuration set\-level suppression logic map](sending-email-suppression-list-config-level.md) will help you understand the effects of the override combinations\. These multitiered selections of overrides can be combined to implement three different levels of suppression:

   1. **Use account\-level suppression:** Do not override your account\-level suppression and do not implement any configuration set\-level suppression \- basically, any email sent using this configuration set will just use your account\-level suppression\. To do this:

      1. In **Suppression list settings**, uncheck the **Override account level settings** box\.

   1. **Do not use any suppression:** Override your account\-level suppression without enabling any configuration set\-level suppression \- this means any email sent using this configuration set will not use any of your account\-level suppression; in other words, all suppression is cancelled\. To do this:

      1. In **Suppression list settings**, check the **Override account level settings** box\.

      1. In **Suppression list**, uncheck the **Enabled** box\.

   1. **Use configuration set\-level suppression:** Override your account\-level suppression with custom suppression list settings defined in this configuration set \- this means any email sent using this configuration set will only use its own suppression settings and ignore any account\-level suppression settings\. To do this:

      1. In **Suppression list settings**, check the **Override account level settings** box\. 

      1. In **Suppression list**, check **Enabled**\. 

      1. In **Specify the reason\(s\)\.\.\.**, select one of the suppression reasons for this configuration set to use\.

1. You can optionally add one or more tags in the **Tags** section\. Repeat the following steps for each tag you want to add to your configuration set\.

   1. Choose **Add new tag**\.

   1. Enter the tag **Key**\.

   1. Enter the tag **Value** \(optional\)\.

   To remove a tag you've entered, choose **Remove** for that tag\. You can enter a maximum of 50 tags\.

1. Choose **Create set** to create your configuration set\.

Now that you’ve created your configuration set, you have the option to define event destinations for your configuration set which enables event publishing that is triggered on the event types you specify for the event destination\. A configuration set can have multiple event destinations with multiple event types defined\. See [Creating Amazon SES event destinations](event-destinations-manage.md)\.

## Create a configuration set \(AWS CLI\)<a name="config-sets-create-cli"></a>

You can create a configuration set using a JSON file as input to the ses create\-configuration\-set command in the AWS CLI\.

1. 

**Create a CLI input JSON file**

   Use your favorite file editing tool to create a JSON file with the following keys, plus values that are valid for your environment, or use the ses create\-configuration\-set command with the `--generate-cli-skeleton` option with no value specified to print a sample JSON structure to standard output\.

   This example uses a file named `create-configuration-set.json`:

   ```
   {
       "configuration-set-name": "sample-configuration-set",
       "tracking-options": {
           "CustomRedirectDomain": "some.domain.com"
       },
       "delivery-options": {
           "TlsPolicy": "REQUIRE",
           "SendingPoolName": "sending pool"
       },
       "reputation-options": {
           "ReputationMetricsEnabled": true,
           "LastFreshStart": timestamp
       },
       "sending-options": {
           "SendingEnabled": true
       },
       "tags": [
           {
               "Key": "tag key",
               "Value": "tag value"
           }
       ],
       "suppression-options": {
           "SuppressedReasons": ["BOUNCE","COMPLAINT"]
       }
   }
   ```
**Note**  
You must include the `file://` notation at the beginning of the JSON file path\.
The path for the JSON file should follow the appropriate convention for the base operating system where you are running the command\. For example, Windows uses the backslash \(\\\) to refer to the directory path, and Linux uses the forward slash \(/\)\.

1. Run the following command, using the file you created as input\.

   ```
   aws sesv2 create-configuration-set --cli-input-json file://create-configuration-set.json
   ```

**Note**  
To review the AWS CLI reference for this command, see [create\-configuration\-set](https://docs.aws.amazon.com/cli/latest/reference/sesv2/create-configuration-set.html)\.