# Managing IP address filters for Amazon SES email receiving<a name="receiving-email-managing-ip-filters"></a>

In addition to creating IP address filters as explained in [Creating IP address filters](receiving-email-ip-filters.md), you can view and delete them, as described in the following sections\.

## Viewing IP address filters<a name="receiving-email-managing-ip-filters-view"></a>

You can use the Amazon SES console or the `ListReceiptFilters` API to get a list of your IP address filters\.

**To view your IP address filters \(console\)**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the left navigation pane, under **Email Receiving**, choose **IP Address Filters**\. You will see a list of your IP address filters\.

For information about how to use the `ListReceiptFilters` API to get a list of your IP address filters, see the [Amazon Simple Email Service API Reference](https://docs.aws.amazon.com/ses/latest/APIReference/API_ListReceiptFilters.html)\.

## Deleting an IP address filter<a name="receiving-email-managing-ip-filters-delete"></a>

You can use the Amazon SES console or the `DeleteReceiptFilter` API to delete an IP address filter\.

**To delete an IP address filter \(console\)**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the left navigation pane, under **Email Receiving**, choose **IP Address Filters**\.

1. In the details pane, select the IP address filter\.

1. Choose **Delete**, and then confirm that you want to delete the IP address filter\.

For information about how to use the `DeleteReceiptFilter` API to delete an IP address filter, see the [Amazon Simple Email Service API Reference](https://docs.aws.amazon.com/ses/latest/APIReference/API_DeleteReceiptFilter.html)\.
