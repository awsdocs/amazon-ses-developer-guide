# Configuring Email Clients to Send Through Amazon SES<a name="configure-email-client"></a>

After you obtain your [SMTP user name and password](smtp-credentials.md), you can use the Amazon SES SMTP interface to send email\.

The following procedures show how to configure [Mozilla Thunderbird](https://www.mozilla.org/thunderbird/) to send email by using Amazon SES\. You might be able to configure other email clients to send email through Amazon SES\. See the documentation for your email client for more information\.

**Note**  
These procedures show you how to set up Mozilla Thunderbird to *send* email using Amazon SES\. However, you can't use Thunderbird to *receive* email that is sent to your Amazon SES email addresses\. For more information about receiving email with Amazon SES, see [Receiving email with Amazon SES](receiving-email.md)\.

## Configuring Mozilla Thunderbird to Send Email Using Amazon SES<a name="configure-email-client-thunderbird"></a>

The procedures in this section show you how to configure [Mozilla Thunderbird](https://www.mozilla.org/thunderbird/) to send email through Amazon SES\. These procedures were tested using Mozilla Thunderbird version 52\.5 on Windows, macOS, and Linux\. The procedures might differ slightly for other versions of Thunderbird\.

### Part 1: Create Local Folders<a name="configure-email-client-thunderbird-part-1"></a>

Amazon SES doesn't include server\-based folders for saving items such as drafts and sent mail\. For this reason, you have to create these folders on your computer\. You configure Thunderbird to save mail to these folders in a later section\.

**To create the Sent Mail and Drafts folders**

1. In the bottom left corner of the Thunderbird window, click the **Offline Mode** \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/thunderbird_offline_mode_icon.png)\) icon to enable offline mode\. If Thunderbird asks if you want to download messages for offline use, choose **Later**\.

1. In the navigation pane on the left side of the Thunderbird window, right\-click a blank area, and then choose **New Folder**\.

1. On the **New Folder** window, complete the following sections:
   + For **Name**, type **Sent Mail**\.
   + For **Create as a subfolder of**, choose **Local Folders**\.

1. Repeat steps 2 and 3 to create an additional folder, but this time, name the folder **Drafts**\.

### Part 2: Configure the SMTP Server<a name="configure-email-client-thunderbird-part-2"></a>

Before you can send email through Amazon SES, you have to configure Thunderbird to connect to the Amazon SES SMTP endpoint\.

**To configure the SMTP server**

1. In Thunderbird, complete one of the following steps:
   + If you use Windows: choose the **Menu** \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/thunderbird_menu_icon.png)\) icon, point to **Options**, and then choose **Account Settings**\.
   + If you use Linux or macOS: choose the **Menu** \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/thunderbird_menu_icon.png)\) icon, point to **Preferences**, and then choose **Account Settings**\.

1. On the **Account Settings** window, in the column on the left, choose **Outgoing Server \(SMTP\)**\.

1. Choose **Add**\.

1. On the **SMTP Server** window, complete the following sections:
   + For **Description**, type **Amazon SES**\.
   + For **Server Name**, enter the SMTP endpoint for the AWS Region in which you use Amazon SES\. For a list of endpoints, see [Amazon SES Regions and Endpoints](regions.md#region-endpoints)\.
   + For **Port**, type **587**\.
   + For **Connection security**, choose **STARTTLS**\.
   + For **Authentication method**, choose **Normal password**\.
   + For **User Name**, type your SMTP user name\.
**Note**  
Your SMTP user name isn't the same as your AWS access key ID\. Additionally, you have to use SMTP credentials that are specific to the AWS Region that you're using\. For more information, see [Obtaining Your Amazon SES SMTP Credentials](smtp-credentials.md)\.

   When you finish, choose **OK**\.

1. On the **Account Settings** window, choose **Account Actions**, and then choose **Add Mail Account**\. 

1. On the **Mail Account Setup** window, complete the following sections:
   + For **Your name**, type the name you want to appear on messages sent from this account\.
   + For **Email address**, type the email address you use to send email with Amazon SES\.
   + Leave the **Password** field blank, and clear the check box next to **Remember password**\.

   When you finish, choose **Advanced config**\. You return to the **Account Settings** window\.
**Note**  
You can only complete this step if Thunderbird is in Offline Mode\.

1. On the **Account Settings** window, in the column on the left, choose the account you created in the previous step\.

1. For **Outgoing Server \(SMTP\)**, choose the SMTP server you created in step 4\.

### Part 3: Configure Thunderbird to Save Sent Mail and Drafts on Your Computer<a name="configure-email-client-thunderbird-part-3"></a>

This section contains procedures for saving sent mail and drafts to your computer\.

**To configure Thunderbird to save sent mail and drafts to your computer**

1. On the **Account Settings** window, under your account, choose **Server Settings**\.

1. Under **Server Settings**, clear the check boxes next to the following items:
   + **Check for new messages at startup**
   + **Check for new messages every 10 minutes**
   + **Allow immediate server notifications when new messages arrive** 

1. On the **Account Settings** window, under your account, choose **Copies & Folders**\.

1. Under **When sending messages, automatically**, choose **Other**, and then choose the Sent Mail folder you created in step 3\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/thunderbird_local_sent_folder.png)

1. Under **Message Archives**, clear the check box next to **Keep message archives in**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/thunderbird_archives_folder.png)

1. Under **Drafts and Templates**, choose **Other**, and then choose the Drafts folder you created in step 4\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/thunderbird_local_draft_folder.png)

### Part 4: Test Email Sending Functionality<a name="configure-email-client-thunderbird-part-4"></a>

Complete the procedures in this section to ensure that Thunderbird is properly configured to send email through Amazon SES\.

**To test email sending in Thunderbird**

1. In the bottom left corner of the screen, choose the **Offline mode** \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/thunderbird_offline_mode_off_icon.png)\) icon to disable offline mode\.

1. On the Thunderbird window, choose **Write**\. Send a test message to yourself, or to another email address that has been verified with Amazon SES\.\.

   When you send the email, you might be prompted to enter a password\. Enter your Amazon SES SMTP password, and then select the box next to **Use Password Manager to remember this password**\.
**Note**  
Your SMTP password isn't the same as your AWS access key ID\. For more information, see [Obtaining Your Amazon SES SMTP Credentials](smtp-credentials.md)\.

   The first time you send an email using this configuration, Thunderbird might display a message stating that it was unable to connect to the server\. If this message appears, click **Retry**\.

### \(Optional\) Part 5: Specify a Configuration Set When Sending Email<a name="configure-email-client-thunderbird-part-5"></a>

You can configure Thunderbird so that it allows you to specify a configuration set when you send a new message\.

**Warning**  
You modify the hidden configuration settings in Thunderbird during this procedure\. Changing these settings might render Thunderbird unusable\. Proceed with caution\.

**To add a configuration set header**

1. In Thunderbird, choose the **Menu** \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/thunderbird_menu_icon.png)\) icon, point to **Options**, and then choose **Options**\.

1. On the **Options** window, choose **Advanced**\. On the **General** tab, choose **Config Editor**\.

1. On the **about:config** window, right\-click anywhere in the list of configuration settings, point to **New**, and then choose **String**\.

1. For **Enter the preference name**, type **mail\.compose\.other\.header**, and then choose **OK**\.

1. For **mail\.compose\.other\.header**, type **X\-SES\-CONFIGURATION\-SET**, and then choose **OK**\.

1. Close the **about:config** window, and then close the **Options** window\.

1. When you send an email, type the recipient's address in the **To** field\. Click the blank line below the recipient's email address\. Click the **To** menu, and then choose **X\-SES\-CONFIGURATION\-SET**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/thunderbird_configuration_set.png)

1. On the **X\-SES\-CONFIGURATION\-SET** line, type the name of the configuration set you want to use\. Complete the remaining sections of the email as you normally would\.