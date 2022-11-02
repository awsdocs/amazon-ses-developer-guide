# Virtual Deliverability Manager dashboard<a name="vdm-dashboard"></a>

The dashboard offers high level views of your account’s deliverability program, such as easy to read graphic cards that show deliverability and reputation through open/click and delivery rates and bounce/complaint stats\. The dashboard also offers a more detailed view, enabling you to drill down to more detailed specific table data when there’s an issue that's tied to a particular ISP, sending identity, or configuration set that's associated with an email campaign\.

Being able to see things from a high overall level with the ability to also view the specific details allows you to focus on the problematic areas of your deliverability rather than needing to review your email program as a whole\. This level of insight also gives you the ability to catch trends and possible problems before they turn into larger deliverability problems, like deferrals or blocks\. 

An account overview in the Virtual Deliverability Manager dashboard with the *Account statistics* table selected\.

![\[An account overview in the Virtual Deliverability Manager dashboard with the Account statistics table selected.\]](http://docs.aws.amazon.com/ses/latest/dg/images/vdm_db_overview.png)

Granular data provided by the dashboard can help you to improve your sender reputation and calculate ideal times and dates for better engagement and conversions for your email program with the ability to drill\-down to specific data sets:
+ <a name="isp-data"></a>**ISP data** – Valuable when you have a deliverability issue to a specific ISP or mailbox provider—instead of trying to adjust your entire account, which may otherwise be doing well, you can focus on the problematic endpoint and align with its best practices to improve sender reputation to that ISP and restore good inbox deliverability to reach your recipients\. It's also important to understand your ISP distribution—as you may send more heavily to one ISP or mailbox provider than to others\. You need to ensure that traffic is always being delivered and engaged by the end recipients to have a positive impact your email conversion\. 
+ <a name="send-ident-config-set"></a>**Sending identity & configuration set data** – Useful in helping you to identify sending identities and configuration sets that are contributing to your overall account deliverability issue\. You can focus on those specifically, adjust your configurations, and possibly reduce sending with a particular identity until the issue is resolved\. For example, a sending identity accidentally sent to a suppression list, resulting in all traffic going through that identity\. That identity is associated with a configuration set, causing deliverability issues\. It’s valuable in such cases to be able to identify the sending identity or configuration set so that you can focus on rectifying that problem specifically, rather than combing through your entire account to try to identify the root cause of the deliverability issue\. 

Drill\-down data displayed in the Virtual Deliverability Manager dashboard for the selected sending identity, *example\.com*—graphic cards display deliverability and reputation metrics\. The table displays all of the ISPs that the sending identity sent mail to with metric rates for each ISP within the date range entered\.

![\[Drill-down data displayed in the Virtual Deliverability Manager dashboard for the selected sending identity, example.com—graphic cards display deliverability and reputation metrics. The table displays all of the ISPs that the sending identity sent mail to with metric rates for each ISP within the date range entered.\]](http://docs.aws.amazon.com/ses/latest/dg/images/vdm_db_ident_drill.png)

## Using the Virtual Deliverability Manager dashboard in the Amazon SES console<a name="vdm-dashboard-console"></a>

The following procedure shows you how to use the Virtual Deliverability Manager dashboard in the Amazon SES console to view your overall deliverability and reputation statistics and to drill\-down into problematic areas\.

**To use the Virtual Deliverability Manager dashboard to view high level and more detailed data of your account’s deliverability metrics**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the left navigation pane, choose **Dashboard** under **Virtual Deliverability Manager**\.
**Note**  
**Dashboard** will not be visible if you haven't enabled Virtual Deliverability Manager for your account\. For more information, see [Getting started with Virtual Deliverability Manager](vdm-get-started.md)\.

1. <a name="full-acct-overview"></a>In the **Full account overview** panel, choose a date range to be used for all metrics in the graphic cards and drill\-down tables\.

   1. In the **Date range** field, choose **Relative range** \(default\) or **Absolute range**\.
     + **Relative range** – Select the radio button that corresponds with the number of days desired\. *Custom range* – Enter a range in either days \(up to 30\), weeks \(up to 4\), or 1 month\.
     + **Absolute range** – The first date you choose will be the *Start date*, the second date will be the *End date*, not to exceed 30 days total\. To specify a single day, choose it for both the *Start* and *End date*\.
**Note**  
The following applies to all date ranges in the dashboard:  
All dates & times are UTC\.
For **Relative range** dates, the last day ends on its UTC midnight timestamp\. For example, if you choose *Last 7 days*, the seventh day would be yesterday, ending at midnight\.

1. <a name="acct-stats"></a>The graphic cards and all of the drill\-down tables \(**Accounts statistics**, **ISP**, **Sending identities**, and **Configuration sets**\) display metric totals calculated from the date range entered, and use the metric math described below:
   + **% Rate** – The metric total divided by either the sent or delivered total, depending on the metric\. For example, Delivered = *Delivered volume / Sent volume \* 100*, Open rate = *Open volume / Delivered volume \* 100*\.
     + The *Total send volume* metric doesn’t have a *% Rate*\. It’s always 100% because it’s everything you’ve sent\.
**Note**  
The following applies to all metrics in the dashboard:  
Click rate is only calculated for messages that have engagement tracking turned on and contain at least one link\.
Open rate is only calculated for messages that have engagement tracking turned on\.
Complaint rate is only calculated for messages that are sent to ISPs that SES has a [feedback loop \(FBL\)](faqs-enforcement.md#cm-feedback-loop) set up with\.
Messages sent to addresses on your account\-level suppression list won’t be counted in the bounce or complaint rates\.
Messages sent to the *SES mailbox simulator* are not counted in any of the metrics\.
**Important**  
Apple Mail's Privacy Protection and its impact to engagement rates: As a result of Apple implementing their Mail Privacy Protection \(MPP\) feature for Apple devices as of iOS15, engagement numbers have become inflated as MPP triggers opens as the Apple Mail app is initiated, not necessarily when a recipient opens and/or clicks a message\. This causes engagement data to look much higher than it typically would be and this is something email marketers will have to take into account when reviewing engagement\. There are several other ways of identifying engagement, such as web activity, app/portal usage and also using proxy data from non\-Apple devices to build an aggregate metric\. The important thing to focus on is the trends of engagement as that can indicate if there's a problem with your email sending\. For more information, see [Apple Mail's Privacy Protection](https://aws.amazon.com/blogs/messaging-and-targeting/apple-mails-ios15-privacy-protection-impact-to-senders-2/)\.
   + **% Difference** – The difference in the metric total as compared to the previous metric total for the given date range\. For example, if *Last 7 days* is the specified date range, Complaints = *Complaint rate of last 7 days \- Complaint rate of previous 7 days*\.
     + *Total send difference* is calculated differently\. For example, *\(Send volume of last 7 days \- Send volume of previous 7 days\) / Send volume of previous 7 days \* 100*\.
   + **Volume** – Total of each metric\.

1. Choose the **Accounts** tab to display the **Accounts statistics** table\.
   + This table gives an overview of your deliverability and reputation metrics, showing the total **Volume**, **% Rate**, and **% Difference** for *Sent*, *Delivered*, *Complaints*, *Transient & Permanent bounces*, *Opens & Clicks* as calculated from the date range entered\.

1. Choose the **ISP** tab to display the **ISP** table\.
   + This table displays metrics for *Send volume*, *Delivered*, *Transient & Permanent bounces*, *Complaints*, *Opens & Clicks* for each ISP you’ve sent to as calculated from the date range entered\.
   + To filter specific ISPs, inside the *Choose ISPs* search box, choose the corresponding check box for each ISP to include\.

1. Choose the **Sending identities** tab to display the **Sending identities** table\.
   + This table displays metrics for *Send volume*, *Delivered*, *Transient & Permanent bounces*, *Complaints*, *Opens & Clicks* for each sending identity you’ve used as calculated from the date range entered\.
   + To filter specific sending identities, inside the *Choose identities* search box, choose the corresponding check box for each identity to include\.
   + To drill\-down on a specific sending identity, choose its name in the **Sending identity** column\.
     + Graphic cards will appear displaying *Delivery rate*, *Complaints*, *Transient & Permanent bounces*, *Open & Click rates* for the selected sending identity as calculated from the date range entered\.
     + An ISP table will be displayed listing all the ISPs the sending identity sent mail to with metrics given for each ISP as calculated from the date range entered\.

1. Choose the **Configuration sets** tab to display the **Configuration sets** table\.
   + This table displays metrics for *Send volume*, *Delivered*, *Transient & Permanent bounces*, *Complaints*, *Opens & Clicks* for each configuration set that’s been used to send mail as calculated from the date range entered\.
   + To filter specific configuration sets, inside the *Choose configuration sets* search box, choose the corresponding check box for each configuration set to include\.
   + To drill down on a specific configuration set, choose its name in the **Configuration set** column\.
     + Graphic cards will appear displaying *Delivery rate*, *Complaints*, *Transient & Permanent bounces*, *Open & Click rates* for the selected configuration set as calculated from the date range entered\.
     + An ISP table will be displayed listing all the ISPs the configuration set was used to send mail to with metrics given for each ISP as calculated from the date range entered\.

## Accessing your Virtual Deliverability Manager metric data using the AWS CLI<a name="vdm-dashboard-cli"></a>

The following example shows you how to access your Virtual Deliverability Manager metric data using the AWS CLI\. This is the same data used in the Virtual Deliverability Manager dashboard in the console\.

**To access your deliverability metric data using the AWS CLI**  
You can use the [https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_BatchGetMetricData.html](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_BatchGetMetricData.html) operation in the Amazon SES API v2 to access your deliverability metric data\. You can call this operation from the AWS CLI as shown in the following examples\.
+ Access your deliverability metric data:

  ```
  aws --region us-east-1 sesv2 batch-get-metric-data --cli-input-json file://sends.json
  ```
+ The input file looks similar to this:

  ```
  {
   "Queries": [
     {
       "Id": "Retrieve-Account-Sends",
       "Namespace": "VDM",
       "Metric": "SEND",
       "StartDate": "2022-11-04T00:00:00",
       "EndDate": "2022-11-05T00:00:00"
      }
   ]
  }
  ```

  More information about parameter values and related data types can be found by linking from the [https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_BatchGetMetricDataQuery.html](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_BatchGetMetricDataQuery.html) data type in the Amazon SES API v2 reference\.