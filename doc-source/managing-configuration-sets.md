# Managing configuration sets in Amazon SES<a name="managing-configuration-sets"></a>

After creating a configuration set, you can manage it with the view, edit, and delete options using the SES console, the Amazon SES API v2, and the Amazon SES CLI v2\. Configuration sets can also be assigned to a verified identity as its default configuration set that is applied every time email is sent from the identity\.

**Topics**
+ [View, edit, & delete configuration set \(console\)](#console-detail-configuration-sets)
+ [List configuration sets \(AWS CLI\)](#cli-list-configuration-sets)
+ [Get configuration set details \(AWS CLI\)](#cli-get-configuration-set)
+ [Delete a configuration set \(AWS CLI\)](#cli-delete-configuration-set)
+ [Stop sending email from a configuration set \(AWS CLI\)](#cli-configuration-set-stop-sending)
+ [Understanding default configuration sets](#default-config-sets)
+ [Managing Amazon SES event destinations](event-destinations-manage.md)
+ [Manage IP pools in Amazon SES](managing-ip-pools.md)
+ [Configuring custom domains to handle open and click tracking](configure-custom-open-click-domains.md)

## View, edit, & delete configuration set \(console\)<a name="console-detail-configuration-sets"></a><a name="proc-access-exist-config-set"></a>

**Access an existing configuration set's detail page**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane, under **Configuration**, choose **Configuration sets**\.

1. To see details for a configuration set, choose the **Name** from the configuration set list\. This takes you to the details page\.

The **Configuration sets** detail page has two tabs for configuration set details with panels in each tab where you can view, edit, or delete as follows:
+ 

****Overview** tab**
  + **General details** – this panel shows general details for the configuration set:
    + **Sending status** \(whether it's currently enabled\)
    + **Configuration set name**
    + **Sending IP pool**
    + **Transport Layer Security \(TLS\)**
    + **Custom redirect domain**
  + **Reputation options** – this panel shows details related to your sending reputation:
    + **Reputation metrics** \(indicates if you're tracking metrics\)
    + **Last fresh start** \(the date and time at which the reputation metrics for the configuration set were last reset\)\.
    + **Suppression settings**
    + **Suppression reasons**
  + **Tags** – this panel shows all of the tags you've attached to the configuration set\.
    + **Key**
    + **Value**

  

  You can perform the following actions from these panels:
  + Choose the **Edit** button, or in the case of the Tags panel, the **Manage tags** button to edit the respective details of each panel\.
  + For more information about the fields, see the related section in the [Create a configuration set \(console\)](creating-configuration-sets.md#config-sets-create-console) steps\.
**Tip**  
Remember to **Save changes** when you are done editing\. Choose **Cancel** to go back to the configuration set detail page without saving\.
+ 

****Event destinations** tab**
  + **All destinations \(*count of event destinations*\)** – this panel lists all of the event destinations that you have entered for your configuration set\. For each destination, you can see:
    + **Name**
    + **Destination**
    + **Event types**
    + **Event publishing**

  

  You can perform the following actions from this panel:
  + Add a new event destination by choosing the **Add destination** button\. For more information about adding an event destination, see [Add an event destination \(console\)](event-destinations-manage.md#event-destination-add)\.
  + Modify an existing event destination by selecting its name which will open the edit screen\.
  + Delete an existing event destination by selecting the check box next to its name then choosing the **Delete** button\.

At the top of each configuration set's details page, and visible from either the **Overview** or **Events destination** tab, are the following options:
+ **Delete** – this button will delete your configuration set\.
+ **Disable sending** – this button will stop sending emails from your configuration set\.

## List configuration sets \(AWS CLI\)<a name="cli-list-configuration-sets"></a>

You can use the list\-configuration\-sets command in the AWS CLI to generate a list of all the configuration sets associated with your account in the current Region, as follows:

```
aws sesv2 list-configuration-sets
```

## Get configuration set details \(AWS CLI\)<a name="cli-get-configuration-set"></a>

You can use the get\-configuration\-set command in the AWS CLI to get details for a specific configuration set, as follows:

```
aws sesv2 get-configuration-set --configuration-set-name name
```

## Delete a configuration set \(AWS CLI\)<a name="cli-delete-configuration-set"></a>

You can use the delete\-configuration\-set command in the AWS CLI to delete a specific configuration set, as follows:

```
aws sesv2 delete-configuration-set --configuration-set-name name
```

## Stop sending email from a configuration set \(AWS CLI\)<a name="cli-configuration-set-stop-sending"></a>

You can use the put\-configuration\-set\-sending\-options command in the AWS CLI to stop sending email from a specific configuration set, as follows:

```
aws sesv2 put-configuration-set-sending-options --configuration-set-name name --no-sending-enabled
```

To start sending again, run the same command with the `--sending-enabled` option instead, as follows:

```
aws sesv2 put-configuration-set-sending-options --configuration-set-name name --sending-enabled
```

## Understanding default configuration sets<a name="default-config-sets"></a>

The concept of assigning a configuration set as the default to be used by a verified identity is explained in this section to help understand the benefits and use case\.

A default configuration set automatically applies its rules to all messages that you send from the email identity associated with that configuration set\. You can apply default configuration sets to both email address and domain identities during the creation of the identity or after the fact as an edit function of an existing identity\.

**Default configuration set considerations**
+ The configuration set must be created first before associating it with an identity\.
+ Default configuration sets will only be applied if the identity is verified\.
+ An email identity can be associated with only one configuration set at a time\. However, you can apply the same configuration set to multiple identities\.
+ A default configuration set at the email address level overrides a default configuration set at the domain level\. For example, a default configuration set associated with *joe@example\.com* overrides the configuration set for the domain of *example\.com*\.
+ A default configuration set at the domain level applies to all email addresses for that domain \(unless you verify specific addresses for the domain\)\.
+ If you delete a configuration set that's designated as the default configuration set for an identity, and then attempt to send email through that identity, your call to Amazon SES fails with a "bad request" error\.
+ How to specify an existing configuration set to be used as the identity's default configuration set is actually a function of verified identities, so instructions are given in the identity workflows accordingly:
  + **Specify a default configuration set during identity creation** – follow the instructions given in the optional Step 6 for either [Domain identity default configuration set](creating-identities.md#verified-domain-identity-default-config-set) or [Email identity default configuration set](creating-identities.md#verified-email-identity-default-config-set) located in the [Creating and verifying identities in Amazon SES](creating-identities.md) chapter\.
  + **Specify a default configuration set for an existing identity** – follow the steps in [Editing an identity using the console](managing-identities.md#edit-verified-domain) along with these details for Step 5:

    1. Choose the **Configuration set** tab\.

    1. Choose **Edit** in the **Default configuration set** container\.

    1. Select the list box and choose an existing configuration set to be used as the default\.

    1. Continue with the remaining steps in [Editing an identity using the console](managing-identities.md#edit-verified-domain)\.