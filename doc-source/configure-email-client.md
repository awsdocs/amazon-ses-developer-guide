# Configuring Email Clients to Send Through Amazon SES<a name="configure-email-client"></a>

After you obtain your [SMTP user name and password](smtp-credentials.md), you can send email through Amazon SES\. 

The following procedures show how to configure three popular email clients—[Mozilla Thunderbird](https://www.mozilla.org/thunderbird/), [Microsoft Outlook](https://products.office.com/outlook/), and macOS Mail—to send email through Amazon SES\. If you are using a different email client, the specific procedures are different, but the concepts and settings are the same\.

**Note**  
These procedures help you set up an email client to *send* email using Amazon SES\. However, you cannot use these clients to *receive* email sent to the email addresses you use with Amazon SES\.  
For more information about receiving email in Amazon SES, see [Receiving Email with Amazon SES](receiving-email.md)\.


+ [Configuring Mozilla Thunderbird to Send Email Using Amazon SES](#configure-email-client-thunderbird)
+ [Configuring Microsoft Outlook to Send Email Using Amazon SES](#configure-email-client-outlook)
+ [Configuring macOS Mail to Send Email Using Amazon SES](#configure-email-client-macos-mail)

## Configuring Mozilla Thunderbird to Send Email Using Amazon SES<a name="configure-email-client-thunderbird"></a>

The procedures in this section help you configure [Mozilla Thunderbird](https://www.mozilla.org/thunderbird/) to send email through Amazon SES\. These procedures were tested using Mozilla Thunderbird version 52\.5 on Windows, macOS and Linux\. The procedures may differ slightly for other versions of Thunderbird\.

### Part 1: Create Local Folders<a name="configure-email-client-thunderbird-part-1"></a>

Amazon SES does not include server\-based folders in which you can save items such as sent mail and drafts\. For this reason, you must create these folders on your computer\. You will configure Thunderbird to save mail to these folders in [Part 3: Configure Thunderbird to Save Sent Mail and Drafts on Your Computer](#configure-email-client-thunderbird-part-3)\.

**To create the Sent Mail and Drafts folders**

1. In the bottom left corner of the Thunderbird window, click the **Offline Mode** \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/thunderbird_offline_mode_icon.png)\) icon to enable offline mode\. If Thunderbird asks if you want to download messages for offline use, choose **Later**\.

1. In the navigation pane on the left side of the Thunderbird window, right\-click a blank area, and then choose **New Folder**\.

1. On the **New Folder** window, complete the following sections:

   + For **Name**, type **Sent Mail**\.

   + For **Create as a subfolder of**, choose **Local Folders**\.

1. Repeat steps 2 and 3 to create an additional folder, but this time, name the folder **Drafts**\.

### Part 2: Configure the SMTP Server<a name="configure-email-client-thunderbird-part-2"></a>

Before you can send email through Amazon SES, you must configure Thunderbird to connect to the Amazon SES SMTP endpoint\.

**To configure the SMTP server**

1. In Thunderbird, complete one of the following steps:

   + If you use Windows: choose the **Menu** \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/thunderbird_menu_icon.png)\) icon, point to **Options**, and then choose **Account Settings**\.

   + If you use Linux or macOS: choose the **Menu** \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/thunderbird_menu_icon.png)\) icon, point to **Preferences**, and then choose **Account Settings**\.

1. On the **Account Settings** window, in the column on the left, choose **Outgoing Server \(SMTP\)**\.

1. Choose **Add**\.

1. On the **SMTP Server** window, complete the following sections:

   + For **Description**, type **Amazon SES**\.

   + For **Server Name**, enter the SMTP endpoint for the AWS Region in which you use Amazon SES\. For a list of endpoints, see [Email Sending Endpoints](regions.md#region-endpoints-sending)\.

   + For **Port**, type **587**\.

   + For **Connection security**, choose **STARTTLS**\.

   + For **Authentication method**, choose **Normal password**\.

   + For **User Name**, type your SMTP user name\.
**Note**  
Your SMTP user name is not the same as your AWS access key ID\. For more information, see [Obtaining Your Amazon SES SMTP Credentials](smtp-credentials.md)\.

   When you finish, choose **OK**\.

1. On the **Account Settings** window, choose **Account Actions**, and then choose **Add Mail Account**\. 

1. On the **Mail Account Setup** window, complete the following sections:

   + For **Your name**, type the name you want to appear on messages sent from this account\.

   + For **Email address**, type the email address you use to send email with Amazon SES\.

   + Leave the **Password** field blank, and clear the check box next to **Remember password**\.

   When you finish, choose **Advanced config**\. You will return to the **Account Settings** window\.
**Note**  
You can only complete this step if Thunderbird is in Offline Mode\.

1. On the **Account Settings** window, in the column on the left, choose the account you created in the previous step\.

1. For **Outgoing Server \(SMTP\)**, choose the SMTP server you created in step 4\.

### Part 3: Configure Thunderbird to Save Sent Mail and Drafts on Your Computer<a name="configure-email-client-thunderbird-part-3"></a>

In this section, you will find procedures for saving sent mail and drafts to your computer\.

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

   When you send the email, you may be prompted to enter a password\. Enter your Amazon SES SMTP password, and then select the box next to **Use Password Manager to remember this password**\.
**Note**  
Your SMTP password is not the same as your AWS access key ID\. For more information, see [Obtaining Your Amazon SES SMTP Credentials](smtp-credentials.md)\.

   The first time you send an email using this configuration, Thunderbird may display a message stating that it was unable to connect to the server\. If this message appears, click **Retry**\.

### \(Optional\) Part 5: Specify a Configuration Set When Sending Email<a name="configure-email-client-thunderbird-part-5"></a>

You can configure Thunderbird so that it allows you to specify a configuration set when you send a new message\.

**Warning**  
You modify the hidden configuration settings in Thunderbird during this procedure\. Changing these settings may render Thunderbird unusable\. Proceed with caution\.

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

## Configuring Microsoft Outlook to Send Email Using Amazon SES<a name="configure-email-client-outlook"></a>

The procedures in this section will help you configure Microsoft Outlook for Windows to send email through Amazon SES\. These procedures were tested using Microsoft Outlook 2016, build 16\.0\.4549\.1000\. The procedures may differ slightly for other versions of Outlook\.

**Note**  
These procedures will not work on Microsoft Outlook for Mac\. Outlook for Mac requires an incoming \(IMAP\) server, and does not allow you to bypass the connection test in the same way that the Windows version does\. If you need a graphical client for sending email from a macOS computer, we recommend that you use [Thunderbird](#configure-email-client-thunderbird)\.

**To configure Microsoft Outlook 2016 to send email using Amazon SES**

1. In Microsoft Outlook, choose **File**, and then choose **Info**\.

1. Choose **Add Account**\.

1. On the **Add Account** window, choose **Manual setup or additional server types**, and then choose **Next**\.

1. Under **Choose Service**, choose **POP or IMAP**, and then choose **Next**\.

1. Under **POP and IMAP Account Settings**, fill in the following fields:

   1. ****Your Name**** – Your full name\.

   1. ****Email Address**** – The email address from which you will send emails\. This address must be verified in Amazon SES, and it must exactly match the address specified in Amazon SES\. 

   1. ****Account Type**** – Choose **POP3**\.

   1. ****Incoming mail server**** – Type **none**\. \(Even though you are setting up Amazon SES for outgoing email only, this field is required\.\)

   1. ****Outgoing mail server \(SMTP\)**** –Type the SMTP endpoint for the outgoing mail server\. For a list of Amazon SES SMTP endpoints, see [Connecting to the Amazon SES SMTP Endpoint](smtp-connect.md)\. For example, if you use the Amazon SES endpoint in the US West \(Oregon\) Region, the outgoing mail server is *email\-smtp\.us\-west\-2\.amazonaws\.com*\.

   1. ****User Name**** – Type **none**\. You will configure your credentials later in this procedure\.  
![\[Microsoft Outlook 2013 configuration\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/SMTP_outlook_01.png)

1. Choose **More Settings**\.

1. On the **Internet E\-mail Settings** window, choose the **Outgoing Server** tab, and then complete the following fields:

   1. ****My outgoing server \(SMTP\) requires authentication**** – \[Selected\]

   1. ****Log on using**** – \[Selected\]

   1. ****User Name**** – Your Amazon SES SMTP user name\.
**Important**  
Your SMTP password is not the same as your AWS access key ID\. For more information, see [Obtaining Your Amazon SES SMTP Credentials](smtp-credentials.md)\.

   1. ****Password**** – Your Amazon SES SMTP password\.
**Important**  
Your SMTP password is not the same as your AWS secret access key\. For more information, see [Obtaining Your Amazon SES SMTP Credentials](smtp-credentials.md)\.

   1. ****Remember Password**** – \[Selected\]  
![\[Microsoft Outlook 2013 configuration\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/SMTP_outlook_02.png)

1. Choose the **Advanced** tab, and then complete the following fields:

   1. ****Outgoing server \(SMTP\)**** – Type 587\.

   1. ****Use the following type of encrypted connection**** – Choose **TLS**\.  
![\[Microsoft Outlook 2013 configuration\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/SMTP_outlook_03.png)

1. Choose **OK**\.

1. On the **Add Account** page, choose **Test Account Settings**\. On the **Internet E\-mail \- none** window, choose **Cancel**\.

1. Choose the **Tasks** tab and confirm that the *Send test e\-mail message* test completed successfully\.
**Note**  
Because you are using Amazon SES as your outgoing email server only, the *Log onto incoming mail server* test is expected to fail\. The *Send test e\-mail message* test should pass\.

1. If the test completes successfully, clear the box next to **Automatically test account settings when Next is clicked**, and then choose **Next**\.

1. Choose **Next**, and then choose **Finish**\.

1. You set up Amazon SES for email sending only\. To ensure that the account is not set up to receive messages using Amazon SES, you must disable mail retrieval for the account by using the following steps\.

   1. In Microsoft Outlook, choose the **Send/Receive** tab\.

   1. On the **Send/Receive** tab, choose **Send/Receive Groups**, and then choose **Define Send/Receive Groups**\.

   1. In the **Send/Receive Groups** dialog box, choose **Edit**\.

   1. In the **Accounts** section on the left, choose the account you just created for sending mail through Amazon SES\.

   1. Under **Account Options**, clear **Receive mail items**\.

   1. Choose **OK**, and then choose **Close**\.

## Configuring macOS Mail to Send Email Using Amazon SES<a name="configure-email-client-macos-mail"></a>

The procedures in this section will help you configure the Mail application that is included with macOS to send email through Amazon SES\. These procedures were tested using Mail version 11\.1 on macOS High Sierra \(version 10\.13\)\. The procedures may differ slightly for other versions of the Mail app and macOS\.

**Note**  
The procedures in this section apply to the version of Mail that is included with macOS desktop and laptop computers\. The version of Mail that is included with iOS devices, such as iPhones and iPads, cannot be configured to send mail using Amazon SES\.

**To configure macOS Mail to send email using Amazon SES**

1. In Mail, on the **Mailbox** menu, choose **Take All Accounts Offline**\.

1. On the **Mail** menu, choose **Add Account**\.

1. On the **Add a Mail Account** window, complete the following sections:

   + For **Name**, type the name that you want to appear on messages sent from this account\.

   + For **Email Address**, type the email address you use to send email with Amazon SES\.

   + For **Password**, type any value\. This field is used to connect to an IMAP server\. Because Amazon SES does not include an IMAP server, the value you enter here is not important\.

   When you finish, choose **Sign In**\. You will see a message stating that Mail is unable to verify your account name or password\. Choose **Next**\.

1. On the **Select the apps you want to use with this account** window, choose **Mail**, and then choose **Done**\.

1. On the **Mail** menu, choose **Preferences**\.

1. In the pane on the left, choose the IMAP account you just created\.

1. On the **Server Settings** tab, under **Outgoing Mail Server \(SMTP\)**, complete the following sections:

   + For **User Name**, type your SMTP user name\.

   + For **Password**, type your SMTP password\.
**Note**  
Your SMTP user name and password are not the same as your AWS access key ID and secret access key\. For more information, see [Obtaining Your Amazon SES SMTP Credentials](smtp-credentials.md)\.

   + For **Host Name**, enter the SMTP endpoint for the AWS Region in which you use Amazon SES\. For a list of endpoints, see [Email Sending Endpoints](regions.md#region-endpoints-sending)\.

   When you finish, choose **Save**\.
**Note**  
Do not change any of the settings in the Incoming Mail Server \(IMAP\) section\. If you make changes in this section, you will not be able to save your settings\.

1. On the **Mailbox** menu, choose **Take All Accounts Online**\.

1. Send a test message to another email address you control to test the connection\.