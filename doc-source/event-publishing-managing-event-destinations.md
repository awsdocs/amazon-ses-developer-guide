# Managing Amazon SES Event Destinations<a name="event-publishing-managing-event-destinations"></a>

Event destinations allow you to publish email sending metrics—including the numbers of sends, deliveries, opens, clicks, bounces, and complaints—to other AWS products\. To learn more about setting up event publishing, see [Monitoring Using Amazon SES Event Publishing](monitor-using-event-publishing.md)\. 

## Updating an Event Destination<a name="event-publishing-managing-event-destinations-updating"></a>

You can use the Amazon SES console or the `UpdateConfigurationSetEventDestination` API to update an event destination\. 

**To update an event destination \(console\)**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the left navigation pane, choose **Configuration Sets**\.

1. In the configuration set list, choose the configuration set that contains the event destination that you want to update\.

1. In the **Destination** list, to the right of the destination you want to edit, choose the **edit** icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/edit_icon.png)\)\.

1. Edit the event destination details, and then choose **Save**\.

1. To exit the **Edit Configuration Set** page, use the back button of your browser\.

For information about how to use the `UpdateConfigurationSetEventDestination` API to update an event destination, see the [Amazon Simple Email Service API Reference](https://docs.aws.amazon.com/ses/latest/APIReference/API_UpdateConfigurationSetEventDestination.html)\.

## Deleting an Event Destination<a name="event-publishing-managing-event-destinations-deleting"></a>

You can use the Amazon SES console or the `DeleteConfigurationSetEventDestination` API to delete an event destination\. 

**To delete an event destination \(console\)**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the left navigation pane, choose **Configuration Sets**\.

1. In the configuration set list, choose the configuration set that contains the event destination that you want to delete\.

1. In the **Destination** list, choose the **delete** icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/delete_icon.png)\)\. 

1. Confirm that you want to delete the configuration set\.

1. To exit the **Edit Configuration Set** page, use the back button of your browser\.

For information about how to use the `DeleteConfigurationSetEventDestination` API to delete an event destination, see the [Amazon Simple Email Service API Reference](https://docs.aws.amazon.com/ses/latest/APIReference/API_DeleteConfigurationSetEventDestination.html)\.

## Enabling or Disabling an Event Destination<a name="event-publishing-managing-event-destinations-enabling-disabling"></a>

You can use the Amazon SES console or the `UpdateConfigurationSetEventDestination` API to enable or disable an event destination\. 

**To enable or disable an event destination \(console\)**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the left navigation pane, choose **Configuration Sets**\.

1. In the configuration set list, choose the configuration set that contains the event destination that you want to enable or disable\.

1. In the **Destination** list, to the right of the destination you want to edit, choose the edit icon \(the pencil\)\.

1. Select or deselect **Enabled**, and then choose **Save**\.

1. To exit the **Edit Configuration Set** page, use the back button of your browser\.

For information about how to use the `UpdateConfigurationSetEventDestination` API to enable or disable an event destination, see the [Amazon Simple Email Service API Reference](https://docs.aws.amazon.com/ses/latest/APIReference/API_UpdateConfigurationSetEventDestination.html)\.