# Amazon SES email receiving concepts and use cases<a name="receiving-email-concepts"></a>

When you use Amazon SES as your email receiver, you tell the service what to do with your mail\. The primary method, receipt rules, gives you fine\-grained control over your email receiving by utilizing *recipient\-based control* to specify a set of actions to take based on the recipient\. The other method, IP address filters, provides a broad level of *IP\-based control* to block or allow mail based on the originating IP address or range of addresses\.

Both of these methods are described in this section along with an overview of how Amazon SES processes received email, and use cases to help you consider how you want to receive, filter, and process your email when setting up rules and filters\.

**Topics**
+ [Recipient\-based control using receipt rules](#receiving-email-concepts-rules)
+ [IP\-based control using IP address filters](#receiving-email-concepts-ip-filters)
+ [Email\-receiving process](#receiving-email-process)
+ [Use cases and restrictions for Amazon SES email receiving](#receiving-email-consider-use-case)
+ [Email\-receiving authentication and malware scanning](#receiving-email-auth-and-scan)

## Recipient\-based control using receipt rules<a name="receiving-email-concepts-rules"></a>

The primary way to control your incoming mail is to specify how mail is handled through an ordered list of actions for any of your verified domain identities \(email addresses, domains, or sub\-domains\) that you own\. These actions are defined and ordered in *receipt rules* that you create within a *rule set*\.

As an option, you can also add recipient conditions as a way to specify that the actions only be taken if the recipient to whom the incoming mail is addressed matches a recipient identity specified in the condition\.  For example, if you own *example\.com*, you can specify that mail for *user@example\.com* should bounce, and that all other mail for *example\.com* and its subdomains should be delivered\.

Otherwise, if you do not add any recipient conditions, the actions will be applied to everything \- all email addresses, domains, and sub\-domains that belong to your verified domains\. The following actions are available to be applied to your receipt rules:
+ **Add header action—**Adds a header to the received email\. You typically use this action only in combination with other actions\.
+ **Return bounce response action—**Rejects the email by returning a bounce response to the sender and, optionally, notifies you through Amazon SNS\.
+ **Invoke AWS Lambda function action—**Calls your code through a Lambda function and, optionally, notifies you through Amazon SNS\.
+ **Deliver to S3 bucket action—**Delivers the mail to an Amazon S3 bucket and, optionally, notifies you through Amazon SNS\.
+ **Publish to Amazon SNS topic action—**Publishes the complete email to an Amazon SNS topic\. 
**Note**  
The SNS action includes a complete copy of the email content in the Amazon SNS notifications\. The other Amazon SNS notification options mentioned here simply notify you of email delivery; they contain information about the email, not the email content itself\.
+ **Stop rule set action—**Terminates the evaluation of the receipt rule set and, optionally, notifies you through Amazon SNS\.
+ **Integrate with Amazon WorkMail action—**Handles the mail with Amazon WorkMail\. You will typically not use this action directly because Amazon WorkMail takes care of the setup\.

Receipt rules are grouped together into *rule sets*\. If you don't have an existing rule set, you'll first have to create a rule set before you start creating receipt rules\. You can define multiple rule sets for your AWS account, but only one rule set is active at any time\. The following figure shows how receipt rules, rule sets, and actions relate to each other\.

![\[Inbound email overview\]](http://docs.aws.amazon.com/ses/latest/dg/images/inbound_overview_v2.png)

## IP\-based control using IP address filters<a name="receiving-email-concepts-ip-filters"></a>

You can control your mail flow by setting up **IP address filters**\. IP address filters are optional and enable you to specify whether to accept or reject mail originating from an IP address or range of IP addresses\. Your IP address filters can include *block lists* \(IP addresses from which you want to block incoming mail\) and *allow lists* \(IP addresses from which you want to always accept mail\)\.

IP address filters are useful for blocking spam\. Amazon SES maintains its own block list of IP addresses known to send spam, but you can choose to receive mail from those IP addresses by adding them to your allow list\.

**Note**  
If you want to allow mail that originates from an Amazon EC2 IP address, you must add it to your allow list\. All mail originating from Amazon EC2 is blocked by default\.
If you only want to receive mail from a finite list of known IP addresses, then set up a block list that contains `0.0.0.0/0`, and set up an allow list that contains the IP addresses that you trust\. This configuration blocks all IP addresses by default, and only allows mail from the IP addresses that you explicitly specify\.

## Email\-receiving process<a name="receiving-email-process"></a>

When Amazon SES receives an email for your domain, the following events occur:

1. Amazon SES first looks at the IP address of the sender\. Amazon SES allows the mail to pass this stage unless:
   + The IP address is in your block list\.
   + The IP address is in the Amazon SES block list, but not on your allow list\.

1. Amazon SES examines your active rule set to determine whether any of your receipt rules contain a recipient condition:
   + If there's a recipient condition and it matches any of the incoming email's recipients, Amazon SES accepts the email\. Otherwise, if there aren't any matches, Amazon SES rejects the email\.
   + If the receipt rule does not contain a recipient condition, Amazon SES accepts the mail \- all of the rule's actions will apply to all the verified identities you own\.

1. Amazon SES authenticates the email and scans its content for spam and malware:
   + The IP address of the remote host that delivered the email to Amazon SES is checked against the SPF policy specified under the MAIL FROM's domain used during the SMTP transaction\.
   + The DKIM signatures present in the email's header section are checked\.
   + If content scanning is enabled, the email content is scanned for spam and malware\.
   + The email authentication and content scanning results are made available to you during the receipt rules evaluation\.

   See [Email authentication and malware detection](#receiving-email-auth-and-scan) for more information\.

1. For the email that Amazon SES accepts, all of the receipt rules within your active rule set  are applied in the order you've defined; and within each receipt rule, the actions are executed in the order you've defined\. 

## Use cases and restrictions for Amazon SES email receiving<a name="receiving-email-consider-use-case"></a>

This section goes over some general considerations and use cases for Amazon SES email receiving\. Presented in question and answer format, are commonly asked questions and facts to help determine if it would be beneficial for using Amazon SES to receive and manage email on behalf of one or more of the verified domains that you own\.

### Regional availability<a name="receiving-email-consider-use-case-regions"></a>

**Does Amazon SES support email receiving in your Region?**

Amazon SES only supports email receiving in certain AWS Regions\. For a complete list of Regions where email receiving is supported, see [Amazon Simple Email Service endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/ses.html) in the AWS General Reference\.

### POP or IMAP based email clients<a name="receiving-email-consider-use-case-clients"></a>

**Can Microsoft Outlook be used to receive incoming email?**

Amazon SES doesn't include POP or IMAP servers for receiving incoming email\. This means that you can't use an email client such as Microsoft Outlook to receive incoming email\. If you need a solution that can both send and receive email by using an email client, consider using [Amazon WorkMail](https://aws.amazon.com/workmail)\.

### Using other AWS services<a name="receiving-email-consider-use-case-permissions"></a>

**Have you set up the appropriate permissions?**

If you want your mail to be delivered to an S3 bucket, published to an Amazon SNS topic you don't own, trigger a Lambda function, or use a customer managed key, you need to give Amazon SES permission to access those resources\. To give Amazon SES access, you create policies on resources from the consoles or APIs for those AWS services\. For more information [Giving permission](receiving-email-permissions.md)\.

### Email content<a name="receiving-email-consider-use-case-content"></a>

**How do you want Amazon SES to pass you the email content?**

Amazon SES can provide you the email content in two ways: it can store the emails in an S3 bucket that you specify, or it can send you an Amazon SNS notification that contains a copy of the email\. Amazon SES delivers you the raw, unmodified email in Multipurpose Internet Mail Extensions \(MIME\) format\. For more information about MIME format, see [RFC 2045](https://tools.ietf.org/html/rfc2045)\. 

**How large are the emails that you'll be receiving?**

If you store emails in an S3 bucket, the maximum email size \(including headers\) is 30 MB\. If you receive your emails through Amazon SNS notifications, the maximum email size \(including headers\) is 150 KB\.

**How do you want to trigger the processing of your mail?**

After your mail is delivered, you will want to process it with your own code\. For example, your application might convert the base 64\-encoded email into a displayable format and then make it available to an end user through an email client\. There are a couple of ways you can start the process:
+ If your emails are delivered to Amazon S3, your application can listen for Amazon SNS notifications generated by S3 actions, extract the message ID of the email from the notifications, and then use the message ID to retrieve the email from Amazon S3\.

  Alternatively, you can incorporate email processing into your receipt rules by writing a Lambda function\. In this case, your receipt rule should first write the email to Amazon S3, and then trigger the Lambda function\. Lambda actions can be executed synchronously or asynchronously from within your receipt rules, depending on whether the Lambda function needs to return a result that influences how other actions are executed\. We recommend that you use asynchronous execution unless synchronous is absolutely necessary for your use case\. For more information about AWS Lambda, see the [AWS Lambda Developer Guide](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html)\.
+ If your emails are delivered through an Amazon SNS notification by using the SNS action, your application can listen for Amazon SNS notifications, and then extract the email messages from the notifications\.

**Do you want the emails to be encrypted?**

Amazon SES integrates with AWS Key Management Service \(AWS KMS\) to optionally encrypt the mail it writes to your S3 bucket\. Amazon SES uses client\-side encryption to encrypt your mail before writing it to Amazon S3\. This means that you must decrypt the content on your side after retrieving the mail from Amazon S3\. The [AWS SDK for Java](https://aws.amazon.com/sdk-for-java/) and [AWS SDK for Ruby](https://aws.amazon.com/sdk-for-ruby/) provide a client that can handle the decryption for you\. Amazon SES can encrypt the emails for you only if you choose for your emails to be delivered to an S3 bucket\.

### Unwanted mail<a name="receiving-email-consider-use-case-unwanted"></a>

**At what point in the email\-receiving process do you want to reject unwanted mail?**

When a sender tries to send an email to a recipient, the sender's email server exchanges a sequence of commands with the recipient's server\. This sequence is called the *SMTP conversation*\.

You can reject incoming email at two points in the email receiving process: during the SMTP conversation, and after the SMTP conversation\. You use *IP address filters* to reject messages during the SMTP conversation, and *receipt rules* to reject emails after the SMTP conversation\.

You can use IP address filters to reject email that originates from specific IP addresses\. The benefit of using IP address filters to reject unwanted mail is that we don't charge you for messages that are rejected during the SMTP conversation\. The drawback to using IP address filters is that they reject email from the IP addresses you specify without performing any analysis on the actual content of the messages\. For more information about IP address filters, see [Create IP address filters console walkthrough](receiving-email-ip-filtering-console-walkthrough.md)\.

You can use receipt rules to send a bounce notification to the sender of an email based on the address \(or domain, or subdomain\) that the message was sent to\. The benefit of using receipt rules is that you can perform additional analysis on incoming messages before you send a bounce notification to the sender\. For example, you can use AWS Lambda to send bounce notifications only when messages fail DKIM authentication or are identified as spam\. The drawback to using receipt rules is that, because receipt rules are processed after the SMTP conversation, we bill you for each message that you receive\. You might also be charged if you use Lambda to analyze the content of incoming messages\. For more information about receipt rules, see [Creating receipt rules console walkthrough](receiving-email-receipt-rules-console-walkthrough.md)\. For more information about using Lambda to analyze incoming email, see [Lambda function examples](receiving-email-action-lambda-example-functions.md)\. 

### Mail streams<a name="receiving-email-consider-use-case-streams"></a>

**How do you want to divide your mail stream?**

Your domain most likely receives different classes of mail\. For example, some of your domain's mail, such as an email to *user@example\.com*, might be intended for a personal inbox\. Other mail, such as an email to *unsubscribe@example\.com*, might be better directed to automated systems instead\. You can use receipt rules to divide your incoming mail so that it can be processed differently\. For information about how to set up receipt rules, see [Creating receipt rules](receiving-email-receipt-rules-console-walkthrough.md)\.

## Email\-receiving authentication and malware scanning<a name="receiving-email-auth-and-scan"></a>

Amazon SES authenticates each received email and optionally scans the email’s content for spam and malware\. SES doesn’t take any actions on received email based on the results of the email authentication or content scanning; however, the results of these operations are provided to you as attributes that you can use in SES receipt rule actions such as [Amazon SNS notifications](receiving-email-notifications-examples.md#receiving-email-notifications-examples-sns-action) or as headers in a message [delivered to Amazon S3](receiving-email-action-s3.md)\.

**Email authentication**

Amazon SES authenticates each received email using SPF, DKIM and DMARC\. The results of each authentication mechanism is provided in the Amazon SNS notifications that SES dispatches as part of evaluating the rules in the active [receipt rule set](receiving-email-action-sns.md)\. In addition, if you chose to receive a copy of the email in Amazon S3, the result of the email authentication is captured in the `Authentication-Results` header that SES adds to the email’s header section:

```
Authentication-Results: example.com;
spf=pass (spfCheck: 10.0.0.1 is permitted by domain of example.com) client-ip=10.0.0.1; envelope-from=example@example.com; helo=10.0.0.1;
dkim=pass header.i=example.com;
dkim=permerror header.i=some-example.com;
dmarc=pass header.from=example@example.com;
```

The `Authentication-Results` header is described in [RFC 8601](https://datatracker.ietf.org/doc/html/rfc8601)

**Email content scanning for spam and malware detection**

Amazon SES scans received email content for malware depending of the value of the *ScanEnabled* \(API\) or *Spam and virus scanning* \(console\) attribute of the receipt rule that matched the email\. By default SES scans received email content for malware\. To disable content scanning for received emails that match a specific receipt rule, you would need to set the receipt rule’s *ScanEnabled* flag to false if [using the API](https://docs.aws.amazon.com/ses/latest/APIReference/API_ReceiptRule.html), or clear the *Spam and virus scanning* checkbox if [using the console](receiving-email-receipt-rules-console-walkthrough.md#receipt-rules-create-rule-settings)\. If the receipt rule that matched an email is scan enabled, the result of the content scanning is provided in the Amazon SNS notifications that SES dispatches as part of evaluating the rules in the active [receipt rule set](receiving-email-action-sns.md)\. In addition, if you chose to receive a copy of the email in Amazon S3, the result of the content scanning is captured in the `X-SES-Spam-Verdict` and the `X-SES-Virus-Verdict` headers that SES adds to the email’s header section\. 

```
X-SES-Spam-Verdict: PASS
X-SES-Virus-Verdict: FAIL
```

The possible values for the headers above are listed in:
+ [spam](receiving-email-notifications-contents.md#receiving-email-notifications-contents-spamverdict-object)
+ [virus](receiving-email-notifications-contents.md#receiving-email-notifications-contents-virusverdict-object)

Now that you have an understanding of the email receiving concepts, how it works, and it's use cases, you can get started by going to [Setting up email receiving](receiving-email-setting-up.md)\.