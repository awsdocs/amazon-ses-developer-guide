# Using the Reputation Dashboard to Track Bounce and Complaint Rates<a name="reputation-dashboard-dg"></a>

The reputation dashboard contains the same information that the Amazon SES team sees when determining the health of individual accounts\.

**To view the reputation dashboard**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane on the left side of the screen, choose **Reputation Dashboard**\.

   The dashboard displays the following information:
   + **Account status** – A brief description of the health of your account\. Possible values include:
     + **Healthy** – There are no issues currently impacting your account\.
     + **Probation** – Your account is on probation\. If the issues that caused your account to be put on probation are not resolved by the end of the probation period, your account may be suspended\.
     + **Pending end of probation decision** – Your account is on probation\. Because of the nature of the issues that led to this probation, a member of the Amazon SES team must review your account before any further action is taken\.
     + **Shutdown** – Your account has been shutdown\. While your account is shutdown, you will not be able to send email using Amazon SES\. You can appeal this decision; see [Amazon SES Enforcement FAQs](e-faq.md) for more information about submitting an appeal\.
     + **Pending shutdown** – Your account is on probation\. The issues that caused this probation have not been resolved\. These issues may lead to your account being shut down\. However, because of the nature of your account, a member of the Amazon SES team will review your account before any further action is taken\.
   + **Bounce Rate** – The percentage of emails sent from your account that resulted in a hard bounce\.
   + **Complaint Rate** – The percentage of emails sent from your account that resulted in recipients reporting them as spam\.
**Note**  
The **Bounce Rate** and **Complaint Rate** sections also include status messages for their respective metrics\. The following is a list of status messages that may be displayed for these metrics:  
**Healthy** – The metric is below the threshold that could cause your account to be placed on probation\.
**Almost healed** – The metric caused your account to be placed on probation\. Since the probation began, the metric has stayed below the maximum rate\. If the metric remains below the maximum rate, its status may change to Healthy before the probation period ends\.
**Probation** – The metric caused your account to be placed on probation, and is still above the maximum rate\. If the issue that caused the metric to exceed the maximum rate is not resolved by the end of the probation period, your account may be suspended\.
**Shutdown** – The metric caused your account to be suspended\. While your account is shut down, you cannot send email using Amazon SES\. You can appeal this decision\. For information about submitting an appeal, see [Amazon SES Enforcement FAQs](e-faq.md)\.
**Pending shutdown** – The metric caused your account to be placed on probation\. The issues that caused this probation have not been resolved\. These issues may lead to your account being shut down\. A member of the Amazon SES team will review your account before any further action is taken\.
   + *Other Notifications* – If your account is experiencing reputation\-related issues that are not related to bounces or complaints, a brief message will be shown here\. For more information about the notifications that can be shown in this area, see [Reputation Dashboard Messages](reputationdashboardmessages.md)\.