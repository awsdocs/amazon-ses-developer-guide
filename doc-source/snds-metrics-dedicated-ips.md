# SNDS metrics for dedicated IPs<a name="snds-metrics-dedicated-ips"></a>

You can view Smart Network Data Services \(SNDS\) data for leased dedicated IP addresses in each AWS Region where you use Amazon SES\. This SNDS data is available through the Amazon CloudWatch console\.

SNDS is an Outlook program that allows IP owners to help prevent spam within their IP space\. Amazon SES provides this important data for those who lease dedicated IPs\. The SNDS data provides insight into the IP’s mail sending behavior and calls out areas of concern for your sender reputation\.

**Note**  
When referring to Outlook, this covers all the domains they track\. For example, this can cover Hotmail\.com, Outlook\.com, and Live\.com\.

**To view SNDS data for your dedicated IP addresses**

1. Sign in to the Amazon CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Metrics**\.

1. On the **All metrics** tab in the AWS Region of your choice, choose **SES** under **AWS Namespaces**\.

1. Choose **IP Metrics**, which will show you all of your dedicated IPs tracked by SNDS\.

1. View all of your dedicated IPs tracked by SNDS in this list, or select an individual IP address to view only its metrics\.

The following metrics are provided for each dedicated IP address and defined by Outlook\. For more information, see Outlook’s SNDS [FAQs](https://sendersupport.olc.protection.outlook.com/snds/FAQ.aspx#DataProvided)\.

**Note**  
These metrics represent an activity period that provides updated data once a day\. The metrics also have a corresponding timestamp, which reflects a 24\-hour period\.
+ **SNDS\.RCPTCommands** \- This is the number of RCPT commands perceived by SNDS for the specific IP address during the activity period\. RCPT commands are part of the SMTP protocol used to send mail, which specifies the recipient address to which you are trying to deliver email\.
+ **SNDS\.DATACommands** \- The number of DATA commands perceived by SNDS for the specific IP address during the activity period\. DATA commands are part of the SMTP protocol used to send mail, specifically that part which actually transmits the message to the previously established intended recipient\(s\)\.
+ **SNDS\.MessageRecipients** \- The number of recipients on messages perceived by SNDS for the specific IP address during the activity period\.
+ **SNDS\.SpamRate** \- Displays the aggregate results of the spam filtering applied to all messages sent by the IP address during the given activity period\. 
  + A SpamRate of 0 means the IP address has less than 10% spam\.
  + A SpamRate of 0\.5 means that between 10% and 90% spam is generated from the IP address\.
  + A SpamRate of 1 means 90% or more spam is generated from the IP address\.
+ **SNDS\.ComplaintRate** \- This is the fraction of the time that a message received from the IP is complained about by an Outlook user during the activity period\.
  + A ComplaintRate of 1 means a 100% complaint rate\.
  + A ComplaintRate of 0\.05 would mean a 5% complaint rate, for example\.
  + A ComplaintRate of 0 means the rate is less than 0\.1%\.
+ **SNDS\.TrapHits** \- Displays the number of messages sent to "trap accounts\." Trap accounts are accounts maintained by Outlook that don't solicit any mail\. Thus, any messages sent to trap accounts are very likely to be spam\.

## Troubleshooting questions<a name="troubleshooting-questions-snds"></a>

**Q1\. Why does data not populate every day? Either of the following scenarios could apply:**
+ SNDS data is dependent on Outlook’s SNDS program\.
+ There is a minimum threshold of emails SNDS needs to receive to calculate a value\. Data may not be available at times where email volume on an IP was low\.

**Q2\. Why are the SNDS\.SpamRate and SNDS\.ComplaintRate metrics changing, and what do I do if the rate changes to a value of 1?**

This is an indicator that something in your sending behavior has triggered a negative response from the Outlook SNDS program\. In this case, you want to check other Internet Service Providers \(ISPs\) as well as your engagement numbers to make sure it isn’t a global problem\. If it is a global problem, you may see issues with multiple ISPs, which would suggest a list, content, distribution, or permissions problem\. If it is specific to Outlook, review [how to best deliver to Outlook](https://sendersupport.olc.protection.outlook.com/pm/)\. For more information about best practices, see [Best practices for sending email using Amazon SES](best-practices.md)\.

**Q3\. What actions will AWS Support take if my SNDS\.SpamRate changes from a value of 0 \(or 0\.5\) to 1?**

AWS does not have any control over SNDS and therefore has no influence over SNDS\. All mitigation requests need to filed directly with Outlook via their [New support request form](https://support.microsoft.com/en-us/supportrequestform/8ad563e3-288e-2a61-8122-3ba03d6b8d75)\.