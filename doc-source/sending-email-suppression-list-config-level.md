# Using the configuration set\-level suppression list<a name="sending-email-suppression-list-config-level"></a>

You can enable the account\-level suppression list for your entire account, and at the same time, you can configure it separately for different configuration sets \- this is referred to as a configuration set\-level suppression list\. For example, you could have an account\-level suppression list enabled for your entire account whenever bounces and complaints occur, while also using a configuration set\-level suppression list to override your account\-level suppression list so that email addresses are added to your suppression list for just complaints \(not bounces and complaints like is set up in your account\-level suppression list\)\.

With the configuration set\-level suppression list, there are different levels of overriding your account\-level suppression, including not using any suppression at all\. To help understand these various levels of suppression that can be set in the following console procedures, the following relationship map models the decision set of choices made for the enabling or disabling of various levels of overrides, that depending on their combination, can be used to implement three different levels of suppression\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/dg/images/config_set_suppression_logic.png)

## Enabling configuration set\-level suppression<a name="create-config-level-suppression-console"></a>

**To enable configuration set\-level suppression using the Amazon SES new console:**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane, under **Configuration**, choose **Configuration sets**\.

1. In **Configuration sets**, choose the name of the configuration set you want to configure with customized suppression\. 

1. In the **Suppression list options** pane, choose **Edit**\.

1. <a name="suppression-list-config-set-level"></a>The **Suppression list options** section provides a decision set to define customized suppression starting with the option to use this configuration set to override your account\-level suppression\. The [configuration set\-level suppression logic map](#sending-email-suppression-list-config-level) will help you understand the effects of the override combinations\. These multitiered selections of overrides can be combined to implement three different levels of suppression:

   1. **Use account\-level suppression:** Do not override your account\-level suppression and do not implement any configuration set\-level suppression \- basically, any email sent using this configuration set will just use your account\-level suppression\. To do this:

      1. In **Suppression list settings**, uncheck the **Override account level settings** box\.

   1. **Do not use any suppression:** Override your account\-level suppression without enabling any configuration set\-level suppression \- this means any email sent using this configuration set will not use any of your account\-level suppression; in other words, all suppression is cancelled\. To do this:

      1. In **Suppression list settings**, check the **Override account level settings** box\.

      1. In **Suppression list**, uncheck the **Enabled** box\.

   1. **Use configuration set\-level suppression:** Override your account\-level suppression with custom suppression list settings defined in this configuration set \- this means any email sent using this configuration set will only use its own suppression settings and ignore any account\-level suppression settings\. To do this:

      1. In **Suppression list settings**, check the **Override account level settings** box\. 

      1. In **Suppression list**, check **Enabled**\. 

      1. In **Specify the reason\(s\)\.\.\.**, select one of the suppression reasons for this configuration set to use\.

1. Choose **Save changes**\.