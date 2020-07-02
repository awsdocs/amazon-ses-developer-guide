# Amazon SES SMTP issues<a name="troubleshoot-smtp"></a>

This section contains solutions for several common issues related to sending email through the Amazon SES Simple Mail Transfer Protocol \(SMTP\) interface\. It also contains a list of SMTP response codes that Amazon SES returns\.

To learn more about sending email through the Amazon SES SMTP interface, see [Using the Amazon SES SMTP interface to send email](send-email-smtp.md)\.
+ **You can't connect to the Amazon SES SMTP endpoint\.**

  Problems connecting to the Amazon SES SMTP endpoint are most commonly related to the following issues:
  + **Incorrect credentials** – The credentials that you use to connect to the SMTP endpoint are different from your AWS credentials\. To obtain your SMTP credentials, see [Obtaining your Amazon SES SMTP credentials](smtp-credentials.md)\. For more information about credentials, see [Using credentials with Amazon SES](using-credentials.md)\.
  + **Network or firewall issues** – Your network might be blocking outbound connections over the port you're trying to send email from\. To determine if an issue on your local network is causing connection issues, type the following command at the command line, replacing `port` with the port you're trying to use \(typically 465, 587, 2465, or 2587\): `telnet email-smtp.us-west-2.amazonaws.com port`

    If you are able to connect to the SMTP server using this command, and you are trying to connect to Amazon SES using TLS Wrapper or STARTTLS, complete the procedures shown in [Test your connection to the Amazon SES SMTP interface using the command line](send-email-smtp-client-command-line.md)\.

    If you can't connect to the Amazon SES SMTP endpoint using `telnet` or `openssl`, it indicates that something in your network \(such as a firewall\) is blocking outbound connections over the port you're trying to use\. Work with your network administrator to diagnose and fix the problem\.
+ **You're sending to Amazon SES from an Amazon EC2 instance using port 25, and you're receiving timeout errors\.**

  Amazon EC2 restricts port 25 by default\. To remove these restrictions, submit an [Amazon EC2 Request to Remove Email Sending Limitations](https://aws-portal.amazon.com/gp/aws/html-forms-controller/contactus/ec2-email-limit-rdns-request)\. You can also connect to Amazon SES using ports 465 or 587, neither of which is restricted\.
+ **Network errors are causing dropped emails\.**

  Ensure that your application uses retry logic when it connects to the Amazon SES SMTP endpoint, and that your application can detect and retry message delivery in case of a network error\. SMTP is a verbose protocol, and sending an email using this protocol requires several network round trips\. Because of the nature of SMTP, the potential for network errors increases\.
+ **You lose connection with the SMTP endpoint\.**

  Lost connections are most commonly caused by the following issues:
  + **MTU size** – If you receive a time\-out error message, the Maximum Transmission Unit \(MTU\) of the network interface for the computer you're using to connect to the Amazon SES SMTP interface may be too large\. To resolve this issue, set the MTU size on that computer to 1500 bytes\.

    For more information about setting the MTU size on Windows, Linux, and macOS operating systems, see [ Queries Appear to Hang in the Client and Do Not Reach the Cluster](https://docs.aws.amazon.com/redshift/latest/mgmt/connecting-drop-issues.html) in the *Amazon Redshift Cluster Management Guide*\.

    For more information about setting the MTU size for an Amazon EC2 instance, see [ Network Maximum Transmission Unit \(MTU\) for Your EC2 Instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/network_mtu.html) in the *Amazon EC2 User Guide for Linux Instances*\.
  + **Long\-lived connections** – The Amazon SES SMTP endpoint runs on a fleet of Amazon EC2 instances behind an Elastic Load Balancer \(ELB\)\. In order to ensure that the system is up\-to\-date and fault tolerant, active Amazon EC2 instances are periodically terminated and replaced with new instances\. Because your application connects to an Amazon EC2 instance through the ELB, the connection becomes invalid when the Amazon EC2 instance is terminated\. You should establish a new SMTP connection after you have delivered a fixed number of messages via a single SMTP connection, or if the SMTP connection has been active for some amount of time\. You will need to experiment to find appropriate thresholds depending on where your application is hosted and how it submits email to Amazon SES\.
+ **You want to know the IP addresses of the Amazon SES SMTP mail servers so that you can whitelist the IP addresses with your network\.**

  The IP addresses for the Amazon SES SMTP endpoints reside behind load balancers\. As a result, these IP addresses change frequently\. It's not possible to provide a definitive list of all of the IP addresses for the Amazon SES endpoints\. We recommend that you whitelist the `amazonses.com` domain, rather than whitelisting individual IP addresses\.

## SMTP response codes returned by Amazon SES<a name="troubleshoot-smtp-response-codes"></a>

This section contains a list of response codes that the Amazon SES SMTP interface returns\.

You should retry SMTP requests that receive 400 errors\. We recommend that you implement a system that retries requests with progressively longer wait times \(for example, wait 5 seconds before retrying, then wait 10 seconds, and then wait 30 seconds\)\. If the third retry doesn't succeed, wait 20 minutes, and then repeat the process\. To see an example of an implementation that uses an exponential retry policy, see [How to handle a "Throttling \- Maximum sending rate exceeded" error ](https://aws.amazon.com//blogs/messaging-and-targeting/how-to-handle-a-throttling-maximum-sending-rate-exceeded-error/) on the AWS Messaging and Targeting Blog\.

**Note**  
AWS SDKs implement retry logic [automatically](https://docs.aws.amazon.com/general/latest/gr/api-retries.html), but they use the HTTPS interface instead of SMTP\.

If you receive a 500 error, you have to revise your request to correct an issue before you submit the request again\. For example, if your AWS authentication credentials are invalid, you have to update your application to use the correct credentials before you submit your request again\.


****  

| Description | Response code | More information | 
| --- | --- | --- | 
| Authentication successful | `235 Authentication successful` | Your SMTP client successfully connected and signed in to the SMTP server\. | 
| Successful delivery | `250 Ok MessageID` | *MessageID* is a unique string of characters that Amazon SES uses to identify a message\. | 
| Service unavailable | `421 Too many concurrent SMTP connections` | Amazon SES can't process the request because there are currently too many connections to the SMTP server\. | 
| Local processing error | `451 Temporary service failure` | Amazon SES couldn't process the request\. There might be issues with the request that prevent it from being processed\. | 
| Timeout | `451 Timeout waiting for data from client` | Too much time elapsed between requests, so the SMTP server closed the connection\. | 
| Daily sending quota exceeded | `454 Throttling failure: Daily message quota exceeded` | You've exceeded the maximum number of emails that Amazon SES permits you to send in a 24\-hour period\. For more information, see [Managing Your Amazon SES sending quotas](manage-sending-quotas.md)\. | 
| Maximum send rate exceeded | `454 Throttling failure: Maximum sending rate exceeded` | You've exceeded the maximum number of emails that Amazon SES permits you to send per second\. For more information, see [Managing Your Amazon SES sending quotas](manage-sending-quotas.md)\. | 
| Amazon SES issue when validating SMTP credentials |  `454 Temporary authentication failure`  | Issues that could cause this issue include \(but aren't limited to\):  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/troubleshoot-smtp.html)  | 
| Problem receiving the request |  `454 Temporary service failure`  | Amazon SES didn't successfully receive the request\. As a result, the message wasn't sent\. | 
| Incorrect credentials | `530 Authentication required` | The application that you use to send email didn't attempt to authenticate when it connected to the Amazon SES SMTP interface\. | 
| Authentication Credentials Invalid | `535 Authentication Credentials Invalid` | The application that you use to send email didn't provide the correct SMTP credentials to Amazon SES\. Note that your SMTP credentials aren't the same as your AWS credentials\. For more information, see [Obtaining your Amazon SES SMTP credentials](smtp-credentials.md)\. | 
| Account not subscribed to Amazon SES | `535 Account not subscribed to SES` | The AWS account that owns the SMTP credentials is not signed up for Amazon SES\. | 
| Message is too long | `552 Message is too long.` | The message that you're trying to send is larger than 10 MB in size\. | 
| Account not subscribed to Amazon SES | `535 Account not subscribed to SES` | The AWS account that owns the SMTP credentials is not signed up for Amazon SES\. | 
| User not authorized to call the Amazon SES SMTP endpoint | `554 Access denied: User UserARN is not authorized to perform ses:SendRawEmail on resource IdentityARN` | The AWS Identity and Access Management \(IAM\) policy or the Amazon SES sending authorization policy of the user who owns the SMTP credentials isn't allowed to call the Amazon SES SMTP endpoint\. | 
| Unverified email address | `554 Message rejected: Email address is not verified. The following identities failed the check in region region: identity0, identity1, identity2` | You're trying to send email from an email address or domain that isn't [verified to send email from your Amazon SES account](verify-addresses-and-domains.md)\. This error could apply to the "From", "Source", "Sender", or "Return\-Path" addresses\. If your account is still in the sandbox, you also have to verify every recipient email address \(except for the recipients provided by the [Amazon SES mailbox simulator](send-email-simulator.md)\)\. If Amazon SES isn't able to show all of the identities that failed the verification check, the error message ends with three periods \(\.\.\.\)\.  Amazon SES has endpoints in [several AWS Regions](regions.md), and email address verification status is separate for each AWS Region\. You have to complete the verification process for each sender in the AWS Regions that you want to use\.  | 