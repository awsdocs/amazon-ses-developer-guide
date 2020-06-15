# Integrating Amazon SES with Microsoft Exchange<a name="send-email-exchange"></a>

You can configure Microsoft Exchange to send email through Amazon SES\. The following procedures show you how to integrate Microsoft Exchange with Amazon SES using the Microsoft Exchange GUI or Windows PowerShell\.

**Note**  
Microsoft Exchange is a third\-party application, and isn't developed or supported by Amazon Web Services\. The procedures in this section are provided for informational purposes only, and are subject to change without notice\.

These instructions were written using Microsoft Exchange 2013\.

**Important**  
Follow only one of the following procedures \(Microsoft Exchange GUI or Windows PowerShell\)\. If you follow both procedures, you will get an error stating that you have two send connectors with the same name\.

**To integrate Microsoft Exchange with Amazon SES using the Microsoft Exchange GUI**

1. Go to the Microsoft Exchange admin center \(typically *https://<CASServerName>/ecp*\) and sign in as a user who is part of the Exchange administrators group\.

1. From the left menu, choose **mail flow**\.  
![\[Choose mail flow\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/exchange_integration_mail_flow.png)

1. Choose **send connectors**\.  
![\[Choose send connectors\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/exchange_integration_send_connectors.png)

1. Choose the plus sign\.

1. Enter a name for the send connector \(for example, SES\)\.

1. Under **Type**, select **Internet**\.  
![\[New send connector\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/exchange_integration_new_send_connector.png)

1. Choose **Next**\.

1. Select **Route mail through smart hosts**\.  
![\[Route mail through smart hosts\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/exchange_integration_smart_host.png)

1. Choose the plus sign and then enter the Amazon SES endpoint that you will use \(for example, *email\-smtp\.us\-west\-2\.amazonaws\.com*\)\. For a list of endpoint URLs for the AWS Regions where Amazon SES is available, see [Amazon Simple Email Service \(Amazon SES\)](https://docs.aws.amazon.com/general/latest/gr/rande.html#ses_region) in the *AWS General Reference*\.

1. Choose **Save**\. The endpoint you entered will appear in the **SMART HOST** box\.

1. Choose **Next**\.

1. Select **Basic authentication**, then select **Offer basic authentication only after starting TLS**, and then enter your Amazon SES SMTP user name and password\.
**Important**  
Your SMTP user name and password are not the same as your AWS access key ID and secret access key\. Do not attempt to use your AWS credentials to authenticate yourself against the SMTP endpoint\. For more information about credentials, see [Using credentials with Amazon SES](using-credentials.md)\.  
![\[Enter SMTP credentials\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/exchange_integration_smtp_credentials.png)

1. Choose **Next**\.

1. Choose the plus sign\.

1. Verify that **Type** is `SMTP`, **FQDN** is `*`, and **Cost** is `1`\.   
![\[Add address space\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/exchange_integration_address_space.png)

1. Choose **Save** and then choose **Next**\.

1. Choose the plus sign\.

1. Select all transport servers you would like to apply this rule to and choose **Add**\. When you have added all the servers you want to send email through Amazon SES, choose **ok**\.  
![\[Add transport servers\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/exchange_integration_transport_servers.png)

1. Verify that the servers are added and then choose **finish**\.  
![\[Verify servers\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/exchange_integration_confirm_servers.png)

   You should now see a send connector for Amazon SES with an enabled status\. All outbound mail will now flow through Amazon SES\.  
![\[Send connector enabled\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/exchange_integration_connector_enabled_GUI.png)

**To integrate Microsoft Exchange with Amazon SES using Windows PowerShell**

1. Open the Exchange Management Shell and type `$creds = Get-Credential`\. A Windows PowerShell Credential Request dialog box will appear\.

1. In the dialog box, enter your Amazon SES SMTP user name and password and then choose **OK**\.
**Important**  
Your SMTP user name and password are not the same as your AWS access key ID and secret access key\. Do not attempt to use your AWS credentials to authenticate yourself against the SMTP endpoint\. For more information about credentials, see [Using credentials with Amazon SES](using-credentials.md)\.

1. At the command prompt, type the following line, replacing ENDPOINT with an Amazon SES SMTP endpoint \(for example, *email\-smtp\.us\-west\-2\.amazonaws\.com*\)\. For a list of endpoint URLs for the AWS Regions where Amazon SES is available, see [Amazon Simple Email Service \(Amazon SES\)](https://docs.aws.amazon.com/general/latest/gr/rande.html#ses_region) in the *AWS General Reference*\.

    `New-SendConnector -Name "SES" -AddressSpaces "*;1" -SmartHosts "ENDPOINT" -SmartHostAuthMechanism BasicAuthRequireTLS -Usage Internet -AuthenticationCredential $creds` 

   The command line should now display a send connector for Amazon SES with an enabled status\. All outbound mail will now flow through Amazon SES\.  
![\[Send connector enabled\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/exchange_integration_connector_enabled_powershell.png)