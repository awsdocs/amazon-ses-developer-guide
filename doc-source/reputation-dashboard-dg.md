# Using the Reputation Dashboard to Track Bounce and Complaint Rates<a name="reputation-dashboard-dg"></a>

The reputation dashboard contains the same information that the Amazon SES team sees when determining the health of individual accounts\.

**To view the reputation dashboard**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane on the left side of the screen, choose **Reputation Dashboard**\.

   The dashboard displays the following information:
   + **Account status** – A brief description of the health of your account\. Possible values include:
     + **Healthy** – There are no issues currently impacting your account\.
     + **Under review** – Your account is under review\. If the issues that caused us to place your account under review aren't resolved by the end of the review period, we might pause your account's ability to send email\.
     + **Pending end of review decision** – Your account is under review\. Because of the nature of the issues that caused us to place your account under review, we need to perform a manual review of your account before we take any further action\.
     + **Sending paused** – We've paused your account's ability to send email\. While your account's ability to send email is paused, you won't be able to send email using Amazon SES\. You can request that we review this decision\. To learn more about requesting a review, see [Amazon SES Sending Review Process FAQs](faqs-enforcement.md)\.
     + **Pending sending pause** – Your account is under review\. The issues that caused us to place your account under review haven't been resolved\. In this situation, we typically pause your account's ability to send email\. However, because of the nature of your account, we need to review your account before any further action is taken\.
   + **Bounce Rate** – The percentage of emails sent from your account that resulted in a hard bounce\.
   + **Complaint Rate** – The percentage of emails sent from your account that resulted in recipients reporting them as spam\.
**Note**  
The **Bounce Rate** and **Complaint Rate** sections also include status messages for their respective metrics\. The following is a list of status messages that may be displayed for these metrics:  
**Healthy** – The metric is within normal levels\.
**Almost healed** – The metric caused your account to be placed under review\. Since the review period began, the metric has stayed below the maximum rate\. If the metric remains below the maximum rate, the status of this metric changes to **Healthy** before the review period ends\.
**Under review** – The metric caused your account to be placed under review, and is still above the maximum rate\. If the issue that caused the metric to exceed the maximum rate is not resolved by the end of the review period, we might pause your account's ability to send email\.
**Sending pause** – The metric caused us to pause your account's ability to send email\. While your account's ability to send email is paused, you can't send email using Amazon SES\. You can request that we review this decision\. To learn more about submitting a request for review, see [Amazon SES Sending Review Process FAQs](faqs-enforcement.md)\.
**Pending sending pause** – The metric caused us to place your account under review\. The issues that caused this review period haven't been resolved\. These issues might cause us to pause your account's ability to send email\. A member of the Amazon SES team has to review your account before we take any further action\.
   + *Other Notifications* – If your account is experiencing reputation\-related issues that are not related to bounces or complaints, a brief message will be shown here\. For more information about the notifications that can be shown in this area, see [Reputation Dashboard Messages](reputationdashboardmessages.md)\.

**Note**  
The reputation dashboard is available to all users who have access to the AWS console\. You can't use IAM policies to restrict access to the reputation dashboard\.