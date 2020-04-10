# Using the Account\-Level Suppression List<a name="sending-email-suppression-list"></a>

Amazon SES includes an *account\-level suppression list* that applies to your AWS account in the current AWS Region\. This suppression list prevents you from sending email to addresses that previously produced a bounce or complaint event\. When you configure the account\-level suppression list, you specify whether addresses should be added to the list when they result in hard bounces, when they result in complaints, or both\. You can manually add or remove addresses from the account\-level suppression list by using the Amazon SES API v2\.

Amazon SES also includes a global suppression list\. For more information, see [Using the Amazon SES Global Suppression List](sending-email-global-suppression-list.md)\.

## Account\-Level Suppression List Considerations<a name="sending-email-suppression-list-considerations"></a>

You should consider the following factors when you use the account\-level suppression list:
+ If you started using Amazon SES after November 25, 2019, your account uses the account\-level suppression list by default for both bounces and complaints\. If you started using Amazon SES before this date, then you have to enable this feature by using the `PutAccountSuppressionAttributes` operation in the Amazon SES API\.
+ If you attempt to send a message to an address that's on the account\-level suppression list, Amazon SES accepts the message, but doesn't send it\.
+ Amazon SES doesn't count the messages that you send to addresses on the account\-level suppression list toward the bounce rate for your account\.
+ Amazon SES counts the messages that you send to addresses on the account\-level suppression list toward your daily sending quota\.
+ Email addresses on the account\-level suppression list remain there until you remove them by using the [DeleteSuppressedDestination](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_DeleteSuppressedDestination.html) operation in the Amazon SES API v2\.
+ If your account's ability to send email is paused, Amazon SES automatically deletes the addresses in the account\-level suppression list after 90 days\. If your account's ability to send email is restored before this 90\-day period ends, then the addresses in the account\-level suppression list aren't deleted\.
+ Gmail doesn't provide complaint data to Amazon SES\. If a recipient uses the **Spam** button in the Gmail web client to report a message that they receive from you as spam, they aren't added to the account\-level suppression list\.
+ You can enable the account\-level suppression list if your account is in the Amazon SES sandbox\. However, you can't use the [PutSuppressedDestination](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_PutSuppressedDestination) API operation until your account is removed from the sandbox\. To learn more about the sandbox, see [Moving Out of the Amazon SES Sandbox](request-production-access.md)\.
+ When you use the account\-level suppression list, Amazon SES also adds addresses that result in hard bounces to the global suppression list\.

## Enabling the Account\-Level Suppression List<a name="sending-email-suppression-list-enabling"></a>

You can use the [PutAccountSuppressionAttributes](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_PutAccountSuppressionAttributes.html) operation in the Amazon SES API v2 to enable and set up the account\-level suppression list\. You can quickly and easily configure this setting by using the AWS CLI\. For more information about installing and configuring the AWS CLI, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\.

**To configure the account\-level suppression list by using the AWS CLI**
+ At the command line, enter the following command:

------
#### [ Linux, macOS, or Unix ]

  ```
  aws sesv2 put-account-suppression-attributes \
  --suppressed-reasons BOUNCE COMPLAINT
  ```

------
#### [ Windows ]

  ```
  aws sesv2 put-account-suppression-attributes `
  --suppressed-reasons BOUNCE COMPLAINT
  ```

------

  To enable the account\-level suppression list, you have to specify at least one reason for the `suppressed-reasons` parameter\. You can specify either `BOUNCE` or `COMPLAINT`, or you can specify both, as shown in the preceding example\.

## Enabling the Account\-Level Suppression List for a Configuration Set<a name="sending-email-suppression-list-enabling-configuration-set"></a>

You can also configure the account\-level suppression so that it only applies to specific [configuration sets](using-configuration-sets.md)\. When you do, addresses are only added to the suppression list if you specified the configuration set when you sent the email that caused the bounce or complaint event\.

**Note**  
The following procedure assumes that you've already installed the AWS CLI\. For more information about installing and configuring the AWS CLI, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\.

**To configure the account\-level suppression list for a configuration set by using the AWS CLI**
+ At the command line, enter the following command:

------
#### [ Linux, macOS, or Unix ]

  ```
  aws sesv2 put-configuration-set-suppression-options \
  --configuration-set-name configSet \
  --suppressed-reasons BOUNCE COMPLAINT
  ```

------
#### [ Windows ]

  ```
  aws sesv2 put-configuration-set-suppression-options `
  --configuration-set-name configSet `
  --suppressed-reasons BOUNCE COMPLAINT
  ```

------

  In the preceding example, replace *configSet* with the name of the configuration set that should use the account\-level suppression list\.

## Manually Adding an Email Address to the Suppression List for Your Account<a name="sending-email-suppression-list-manual-add"></a>

You can manually add addresses to the account\-level suppression list by using the [PutSuppressedDestination](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_PutSuppressedDestination.html) operation in the Amazon SES API v2\. There's no limit to the number of addresses that you can add to the account\-level suppression list\.

**Note**  
The following procedure assumes that you've already installed the AWS CLI\. For more information about installing and configuring the AWS CLI, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\.

**To manually add an address to the account\-level suppression list by using the AWS CLI**
+ At the command line, enter the following command:

------
#### [ Linux, macOS, or Unix ]

  ```
  aws sesv2 put-suppressed-destination \
  --email-address recipient@example.com \
  --reason BOUNCE
  ```

------
#### [ Windows ]

  ```
  aws sesv2 put-suppressed-destination `
  --email-address recipient@example.com `
  --reason BOUNCE
  ```

------

  In the preceding example, replace *recipient@example\.com* with the email address that you want to add to the account\-level suppression list, and *BOUNCE* with the reason that you're adding the address to the suppression list \(acceptable values are `BOUNCE` and `COMPLAINT`\)\.

## Removing an Email Address from the Suppression List for Your Account<a name="sending-email-suppression-list-manual-delete"></a>

If an address is on the suppression list for your account, but you know that the address shouldn't be on the list, you can manually remove it by using [DeleteSuppressedDestination](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_DeleteSuppressedDestination.html) operation in the Amazon SES API v2\.

**Note**  
The following procedure assumes that you've already installed the AWS CLI\. For more information about installing and configuring the AWS CLI, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\.

**To remove an address from the account\-level suppression list by using the AWS CLI**
+ At the command line, enter the following command:

------
#### [ Linux, macOS, or Unix ]

  ```
  aws sesv2 delete-suppressed-destination \
  --email-address recipient@example.com
  ```

------
#### [ Windows ]

  ```
  aws sesv2 delete-suppressed-destination `
  --email-address recipient@example.com
  ```

------

  In the preceding example, replace *recipient@example\.com* with the email address that you want to remove from the account\-level suppression list\.

## Disabling the Account\-Level Suppression List<a name="sending-email-suppression-list-enabling"></a>

You can use the [PutAccountSuppressionAttributes](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_PutAccountSuppressionAttributes.html) operation in the Amazon SES API v2 to effectively disable the account\-level suppression list by removing the values from the `suppressed-reasons` attribute\.

**Note**  
The following procedure assumes that you've already installed the AWS CLI\. For more information about installing and configuring the AWS CLI, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\.

**To disable the account\-level suppression list by using the AWS CLI**
+ At the command line, enter the following command:

  ```
  aws sesv2 put-account-suppression-attributes --suppressed-reasons
  ```