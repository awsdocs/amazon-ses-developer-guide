# Amazon SES Enforcement FAQs<a name="e-faq"></a>

We monitor the email that is sent through Amazon SES to ensure that users aren't sending unsolicited or malicious content\. If we determine that you're sending unsolicited or malicious content, we may limit or suspend your ability to send additional email\. This process is called *enforcement*\.

This section contains frequently asked questions about the following enforcement\-related topics:
+ [Probations](#e-faq-pr)
+ [Suspensions](#e-faq-sp)
+ [Bounces](#e-faq-bn)
+ [Complaints](#e-faq-cm)
+ [Spamtraps](#e-faq-st)
+ [Manual Investigations](#e-faq-mi)

## Amazon SES Probation FAQ<a name="e-faq-pr"></a>

### Q1\. I received a probation notice\. What does that mean?<a name="pr-q1"></a>

We've detected an issue related to the email sent from your account, and we're giving you time to fix it\. You can continue to send email as you normally would, but you must also correct the issue that caused your account to be placed on probation\. If you don't correct the issue before the probation period is over, your ability to send email using Amazon SES may be suspended\.

### Q2\. Will I always be notified if I am put on probation?<a name="pr-q2"></a>

Yes\. You'll receive a notification at the email address of the AWS account associated with the Amazon SES probation\.

### Q3\. Why didn't I receive a probation notice?<a name="pr-q3"></a>

When your account is placed on probation, we automatically send a notice to the email address associated with your AWS account\. This email address is the one you specified when you created your AWS account\. In some cases, this email address may be different from the one you use to send email using Amazon SES\.

We recommend that you monitor your sender reputation by regularly consulting the [Reputation Dashboard](monitor-sender-reputation.md)\. You can also [set up automated alarms in Amazon CloudWatch](reputationdashboard-cloudwatch-alarm.md)\. These alarms can send you a notification when your reputation metrics exceed certain thresholds\. You can also configure Amazon CloudWatch to contact you in other ways, such as by sending a text message to your mobile phone\.

### Q4\. Will the Amazon SES probation affect my use of other AWS services?<a name="pr-q4"></a>

You'll still be able to use other AWS services while your Amazon SES account is on probation\. However, if you request a service limit increase for another AWS service that sends outbound communications \(such as Amazon SNS\), that request may be denied until the probation on your Amazon SES account is lifted\.

### Q5\. What should I do if I'm on probation?<a name="pr-q5"></a>

You should do the following:
+ If your situation allows it, stop sending mail until you fix the problem\. Although probation doesn't affect your ability to send mail through Amazon SES, if you continue to send mail without first making changes, you're putting your continued sending at risk\.
+ Look at the email you received from us for a summary of the issue\.
+ Investigate your sending to determine what aspect of your sending specifically triggered the issue\.
+ Once you've made your fixes, send us an appeal telling us about the fixes you made \(see [Q6\. What's an appeal?](#pr-q6)\)\. Note that you should appeal your probation only after you've made your changes—don't submit an appeal outlining changes you plan to make\. If you do, we will ask you to contact us again once the fixes are actually in place\. If we find that you've fixed the problem, we'll take you off probation\.
+ Be sure to provide any information we specifically request\. We need this information to evaluate your case\.

### Q6\. What's an appeal?<a name="pr-q6"></a>

An appeal is when you reply to a probation or suspension notification \(or email *ses\-enforcement@amazon\.com* from the email address associated with your AWS account\) and provide the specific information we need to determine whether we can remove the probation or suspension\. For a list of information to provide, see [Q7\. How do I submit an appeal?](#pr-q7)\.

### Q7\. How do I submit an appeal?<a name="pr-q7"></a>

Reply to the probation notification\. If you can't find the probation notification, send your appeal to *ses\-enforcement@amazon\.com* from the email address associated with your AWS account\. In your appeal, you should explain in as much detail as possible the following three things:
+ An explanation of how and why you think the problem occurred\.
+ A list of changes you've already made to address the issue \(not the changes you plan to make\)\.
+ An explanation of why you believe these changes will prevent the problem from happening again\.

Please read the FAQ section specific to your issue for more information about the specific information that you should include in your appeal\.

### Q8\. What if my appeal isn't accepted?<a name="pr-q8"></a>

We will respond to you explaining why your appeal wasn't accepted, and you'll often have the option of appealing again after you address the issue\. For example, we might ask you for more information, and once you provide the information, your appeal might be accepted\. If you tell us how you'll fix the problem but you haven't actually fixed it yet, we'll ask you to contact us again once you've fixed the issue\.

### Q9\. Can you help me diagnose the problem?<a name="pr-q9"></a>

Typically we can give you only a high\-level overview of your issue \(for example, that you have a problem with bounces\)\. You'll need to investigate the root cause on your end\.

### Q10\. How will I know if I'm off probation?<a name="pr-q10"></a>

We will provide this information in our response to your appeal, or in some cases you'll receive an automated notification at the email address associated with your AWS account\. The notification will indicate either that you're off probation, or that your account has been suspended because you haven't fixed the problem\.

### Q11\. Will I always have a probation period if there's a problem?<a name="pr-q11"></a>

No\. There are two cases in which you might not be provided a probation period:
+ If the issue is very serious, your account may be suspended immediately\. If this occurs, we will send you a notification\.
+ If your account has been placed on probation multiple times in the past, your account may be suspended rather than being placed on probation again\. For this reason, it's important to address the underlying problem rather than just the specific incident that caused a specific probation\. For instance, if a particular campaign triggers a probation, you must do more than simply stop that campaign\. You need to determine which properties of the campaign were problematic and ensure that you have processes in place so that your future campaigns won't have the same issue\.

### Q12\. What if I make my fixes shortly before the probation is due to expire?<a name="pr-q12"></a>

Contact us through the appeal process to let us know that you fixed the problem\.

### Q13\. Can I get help from my AWS representative or Premium Support?<a name="pr-q13"></a>

If you're already working with an AWS account representative, we'll automatically contact him or her when your account is placed on probation\. Your account representative may be able to provide additional information to help you better understand the issue\. If you use Premium Support, you should also contact that team for additional help\.

## Amazon SES Suspension FAQ<a name="e-faq-sp"></a>

### Q1\. I received a suspension notice\. What does that mean?<a name="sp-q1"></a>

We shut down your account because of a critical issue with emails you sent\. Your account may be suspended for one of the following reasons:
+ Your account was previously placed on probation\. The issues that caused your account to be placed on probation weren't corrected before the end of the probation period, so your account was suspended\.
+ Your account has a history of being placed on probation for the same issue\.
+ Your account sent email that violated the [AWS Service Terms](https://aws.amazon.com/service-terms)\. If these violations are serious, your account may be suspended without being placed on suspension first\.

### Q2\. Will I always be notified if I am suspended?<a name="sp-q2"></a>

Yes\. You'll receive a notification at the email address of the AWS account associated with the Amazon SES suspension\.

### Q3\. Why didn't I receive a suspension notice?<a name="sp-q3"></a>

When your account is suspended, we automatically send a notice to the email address associated with your AWS account\. This email address is the one you specified when you created your AWS account\. In some cases, this email address may be different from the one you use to send email using Amazon SES\.

We recommend that you monitor your sender reputation by regularly consulting the [Reputation Dashboard](monitor-sender-reputation.md)\. You can also [set up automated alarms in Amazon CloudWatch](reputationdashboard-cloudwatch-alarm.md)\. These alarms can send you a notification when your reputation metrics exceed certain thresholds\. You can also configure Amazon CloudWatch to contact you in other ways, such as by sending a text message to your mobile phone\.

### Q4\. Will the Amazon SES suspension affect my use of other AWS services?<a name="sp-q4"></a>

You'll still be able to use other AWS services while your Amazon SES account is suspended\. However, if you request a service limit increase for another AWS service that sends outbound communications \(such as Amazon SNS\), that request may be denied until the suspension on your Amazon SES account is lifted\.

### Q5\. What should I do if my account has been suspended?<a name="sp-q5"></a>

You should do the following:
+ Look at the email you received from us for a summary of the issue\.
+ Investigate your sending to determine what aspect of your sending specifically triggered the issue\.
+ Once you've fixed the issue, send us an appeal telling us about the fixes you made \(see [Q6\. What's an appeal?](#sp-q6)\)\. Note that you should appeal your suspension only after you've made your changes— don't submit an appeal outlining changes you plan to make\. If you do, we will ask you to contact us again once the fixes are actually in place\.
+ Be sure to provide any information we specifically request\. We need this information to evaluate your case\.

### Q6\. What's an appeal?<a name="sp-q6"></a>

An appeal is when you reply to a probation or suspension notification \(or email *ses\-enforcement@amazon\.com* from the email address associated with your AWS account\) and provide the specific information we need to determine whether we can remove the probation or suspension\. For a list of information to provide, see [Q7\. How do I submit an appeal?](#sp-q7)\.

### Q7\. How do I submit an appeal?<a name="sp-q7"></a>

Just reply to the suspension notification\. If you can't find the suspension notification, send your appeal to *ses\-enforcement@amazon\.com* from the email address associated with your AWS account\. In your appeal, you should explain in as much detail as possible the following three things:
+ An explanation of how and why you think the problem occurred\.
+ A list of changes you've already made to address the issue \(not changes you plan to make\)\.
+ An explanation of why you believe these changes will prevent the problem from happening again\.

Read the FAQ specific to your issue \(for example, bounces\) to see if there is additional information you need to provide in your appeal\.

**Note**  
Failure to provide this information will delay the appeal process because we will request the remaining information before we can make a decision\. In addition, be sure to provide any additional information we specifically request during the appeal correspondence\.

### Q8\. What if my appeal isn't accepted?<a name="sp-q8"></a>

We will respond to you explaining why your appeal wasn't accepted, and you'll often have the option of appealing again after you address the issue\. For example, we might ask you for more information, and once you provide the information, your appeal might be accepted\. As another example, if you tell us how you'll fix the problem and haven't actually fixed it, we'll ask you to contact us again once you've actually fixed the issue\.

### Q9\. Can you help me diagnose the problem?<a name="sp-q9"></a>

Typically we can give you only a high\-level overview of your issue \(for example, that you have a problem with bounces\)\. It's your responsibility to correct the issue\.

### Q10\. How will I know if my account has been reinstated?<a name="sp-q10"></a>

We will provide this information in our response to your appeal, or in some cases you'll receive an automated notification at the email address associated with your AWS account\. You can also try sending an email to yourself through Amazon SES \(for example, using the Amazon SES console\)\. If the attempt is successful, then your account has been reinstated\. 

### Q11\. Can I get help from my AWS representative or Premium Support?<a name="sp-q11"></a>

If you're already working with an AWS account representative, we'll automatically contact him or her when your account is suspended\. Your account representative may be able to provide additional information to help you better understand the issue\. If you use Premium Support, you should also contact that team for additional help\.

## Amazon SES Bounce FAQ<a name="e-faq-bn"></a>

### Q1\. Why do you care about my bounces?<a name="bn-q1"></a>

High bounce rates are often used by entities such as ISPs, mailbox providers, and anti\-spam organizations as indicators that senders are engaging in low\-quality email\-sending practices and their email should be blocked or sent to the spam folder\.

### Q2\. What should I do if I receive a probation or suspension notice for my bounce rate?<a name="bn-q2"></a>

Fix the underlying problem and appeal to get your case reevaluated\. For information about the appeal process, see the FAQs on probation and suspension\. In your appeal, in addition to the information requested in the probation and suspension FAQs, tell us the following:
+ The method you use to track your bounces
+ How you ensure that the email addresses of new recipients are valid prior to sending to them\. For example, which of the recommendations are you following in [Q11\. What can I do to minimize bounces?](#bn-q11)

### Q3\. What types of bounces count toward my bounce rate?<a name="bn-q3"></a>

Your bounce rate includes only hard bounces to domains you haven't verified\. Hard bounces are permanent delivery failures such as "address does not exist\." Temporary and intermittent failures such as "mailbox full," or bounces due to blocked IP addresses, don't count toward your bounce rate\.

### Q4\. Do you disclose the Amazon SES bounce rate limits that trigger probation and suspension?<a name="bn-q4"></a>

No, we don't publish the bounce rates that can cause your account to be placed on probation or suspended\.

### Q5\. Over what period of time is my bounce rate calculated?<a name="bn-q5"></a>

We don't calculate your bounce rate based on a fixed period of time, because different senders send at different rates\. Instead, we look at a *representative volume*—an amount of email that represents your typical sending practices\. To be fair to both high\- and low\-volume senders, the representative volume is different for each user and changes as the user's sending patterns change\.

### Q6\. Can I calculate my own bounce rate by using the information from the Amazon SES console or the GetSendStatistics API?<a name="bn-q6"></a>

No\. The bounce rate is calculated using representative volume \(see [Q5\. Over what period of time is my bounce rate calculated?](#bn-q5)\)\. Depending on your sending rate, your bounce rate can stretch farther back in time than the Amazon SES console or `GetSendStatistics` can retrieve\. In addition, only emails to non\-verified domains are considered when calculating your bounce rate\. However, if you regularly monitor your bounce rates using those methods, you should still have a good indicator that you can use to catch problems before they get to levels that trigger a probation or suspension\.

### Q7\. How can I find out which email addresses bounced?<a name="bn-q7"></a>

Examine the bounce notifications that Amazon SES sends you\. The email address to which Amazon SES forwards the notifications depends on how you sent the original messages, as described at [Amazon SES Notifications Through Email](notifications-via-email.md)\. You can also set up bounce notifications through Amazon Simple Notification Service \(Amazon SNS\), as described at [Monitoring Using Amazon SES Notifications](monitor-sending-using-notifications.md)\. Note that simply removing bounced addresses from your list without any additional investigation might not solve the underlying problem\. For information about what you can do to reduce bounces, see [Q11\. What can I do to minimize bounces?](#bn-q11)\.

### Q8\. If I haven't been monitoring my bounces, can you give me a list of addresses that have bounced?<a name="bn-q8"></a>

No, we can't provide a complete list of addresses that have bounced\. You are responsible for monitoring and acting upon the bounces for your account\.

### Q9\. How should I handle bounces?<a name="bn-q9"></a>

You need to remove bounced addresses from your mailing list and stop sending mail to them immediately\. If you're a small sender, it might be sufficient to simply monitor bounces through email and manually remove bounced addresses from your mailing list\. If your volume is higher, you'll probably want to set up automation for this process, either by programmatically processing the mailbox where you receive bounces, or by setting up bounce notifications through Amazon SNS\. For more information, see [Monitoring Using Amazon SES Notifications](monitor-sending-using-notifications.md)\.

### Q10\. Could my emails be bouncing because I've reached my sending limits?<a name="bn-q10"></a>

No\. Bounces have nothing to do with sending limits\. If you try to exceed your sending limits, you'll receive an error from the Amazon SES API or SMTP interface when you try to send an email\.

### Q11\. What can I do to minimize bounces?<a name="bn-q11"></a>

First, be sure that you're aware of your bounces \(see [Q7\. How can I find out which email addresses bounced?](#bn-q7)\)\. Then follow these guidelines:
+ Don't buy, rent, or share email addresses\. Send email only to recipients who explicitly requested to receive email from you\.
+ Remove bounced email addresses from your list\.
+ On web forms, ask users to enter their email addresses two times, and check to make sure both addresses match before the form can be submitted\.
+ Use double opt\-in to sign up new users\. That is, when a new users sign up, send them a confirmation email that they need to click before receiving any additional mail\. This prevents people from signing up other people as well as accidental sign\-ups\.
+ If you must send to addresses that you haven't mailed lately \(and thus you can't be confident that the addresses are still valid\), do so only with a small portion of your overall sending\. For more information, see our blog post [ Never send to old addresses, but what if you have to?](https://aws.amazon.com//blogs/ses/never-send-to-old-addresses-but-what-if-you-have-to/)\. 
+ Ensure that you're not structuring sign\-ups to encourage people to use fictional addresses\. For example, don't provide any added value or benefits until recipients verify their addresses\.
+ If you have an "email a friend" feature, use CAPTCHA or a similar mechanism to discourage automated use of the feature, and don't allow users to insert arbitrary content\. For more information about CAPTCHA, see [http://www\.captcha\.net/](http://www.captcha.net/)\. 
+ If you're using Amazon SES for system notifications, ensure that you're sending the notifications to real addresses that can receive mail\. Also consider turning off notifications that you don't need\.
+ If you're testing a new system, be sure you're either sending to real addresses that can receive email, or you're using the Amazon SES mailbox simulator\. For more information, see [Testing Amazon SES Email Sending](mailbox-simulator.md)\. 

## Amazon SES Complaint FAQ<a name="e-faq-cm"></a>

### Q1\. What's a complaint?<a name="cm-q1"></a>

A complaint occurs when a recipient reports that they don't want to receive an email\. They might have clicked the "Report spam" button in their email client, complained to their email provider, notified Amazon SES directly, or through some other method\. This topic includes general information about complaints\. If your notification contains specific information about the source of the complaints, also read the relevant topic: [Amazon SES Complaints Through ISP Feedback Loops FAQ](#cm-feedback-loop), [Amazon SES Complaints Directly from Recipients FAQ](#cm-direct), or [Amazon SES Complaints Through Email Providers FAQ](#cm-email-provider)\.

### Q2\. Why do you care about my complaints?<a name="cm-q2"></a>

High complaint rates are often used by entities such as ISPs, email providers, and anti\-spam organizations as indicators that a sender is sending to recipients who didn't specifically sign up to receive emails, or that the sender is sending content that is different from the type that recipients signed up for\. 

### Q3\. What should I do if I receive a probation or suspension notice for my complaint rate?<a name="cm-q3"></a>

Review your list acquisition process and the content of your emails to try to understand why your recipients might not appreciate your email\. Once you've determined the cause, fix the underlying problem and appeal to get your case reevaluated\. For information about the appeal process, see the FAQs on probation and suspension\.

### Q4\. What can I do to minimize complaints?<a name="cm-q4"></a>

First, be sure that you monitor the complaints that Amazon SES can notify you about, which are complaints that Amazon SES receives through ISP feedback loops \(see the [Amazon SES Complaints Through ISP Feedback Loops FAQ](#cm-feedback-loop)\)\. Then follow these guidelines:
+ Do not buy, rent, or share email addresses\. Use only addresses that specifically requested your mail\.
+ Use double opt\-in to sign up new users\. That is, when users sign up, send them a confirmation email that they need to click before receiving any additional mail\. This prevents people from signing up other people as well as accidental sign\-ups\.
+ Monitor engagement with the mail you send and stop sending to recipients who don't open or click your messages\.
+ When new users sign up, be clear about the type of email they will receive from you, and ensure that you send only the type of mail that they signed up for\. For example, if users sign up for news updates, don't send them advertisements\.
+ Ensure that your mail is well\-formatted and looks professional\.
+ Ensure that your mail is clearly from you and can't be confused for something else\.
+ Provide users an obvious and easy way to unsubscribe from your mail\.

### Amazon SES Complaints Through ISP Feedback Loops FAQ<a name="cm-feedback-loop"></a>

This topic provides information about complaints that Amazon SES receives through feedback loops\. For general information that applies to all types of complaints, see the [Amazon SES Complaint FAQ](#e-faq-cm)\.

#### Q1\. How is this type of complaint reported?<a name="cm-feedback-loop-q1"></a>

Most email client programs provide a button labeled "Mark as Spam" or similar, which moves the message to a spam folder and forwards it to the ISP\. Additionally, most ISPs maintain an abuse address \(such as abuse@example\.com\), where users can forward unwanted emails and request that the ISP take action to prevent them\. If the Amazon SES has a feedback loop \(FBL\) set up with the ISP, then the ISP will send the complaint back to Amazon SES\. 

#### Q2\. Are these complaints included in the complaint rate statistic shown in the Amazon SES console and returned by the GetSendStatistics API?<a name="cm-feedback-loop-q2"></a>

Yes\. Note, however, that the complaint rate statistic doesn't include complaints from ISPs that don't provide feedback to Amazon SES\. Nevertheless, the complaint rate from domains that provide feedback is likely to be representative of the rest of your sending as well\.

#### Q3\. How can I be notified of these complaints?<a name="cm-feedback-loop-q3"></a>

You can be notified through email or through Amazon SNS notifications\. See the set\-up instructions in [Monitoring Using Amazon SES Notifications](monitor-sending-using-notifications.md)\.

#### Q4\. What should I do if I receive a complaint notification through email or through Amazon SNS?<a name="cm-feedback-loop-q4"></a>

First, you need to remove addresses that generated complaints from your mailing list and stop sending mail to them immediately\. Do not even send an email that says you've received the request to unsubscribe\. Consider setting up automation for this process, either by programmatically processing the mailbox where you receive complaints, or by setting up complaint notifications through Amazon SNS\. For more information, see [Monitoring Using Amazon SES Notifications](monitor-sending-using-notifications.md)\.

Then, take a close look at your sending to determine why your recipients don't appreciate the mail you're sending, and address that underlying problem\. For every person who complains, there are potentially dozens who didn't appreciate your mail who didn't \(or weren't able to\) complain\. If you only remove the recipients who actually complain, you're not addressing the underlying problem\.

#### Q5\. Do you disclose the Amazon SES complaint rate limits that trigger probation and suspension?<a name="cm-feedback-loop-q5"></a>

No, we don't publish the complaint rates that cause your account to be placed on probation or suspended\.

#### Q6\. Over what period of time is my complaint rate calculated?<a name="cm-feedback-loop-q6"></a>

We don't calculate your complaint rate based on a fixed period of time, because different senders send at different rates\. Instead, we look at a *representative volume*—an amount of mail that represents your typical sending practices\. To be fair to both high\- and low\-volume senders, the representative volume is different for each user and changes as the user's sending patterns change\. Additionally, the complaint rate isn't calculated based on every email\. Instead, it's calculated as the percentage of complaints on mail sent to domains that send complaint feedback to Amazon SES\.

#### Q7\. Can I calculate my own complaint rate by using metrics from the Amazon SES console or the GetSendStatistics API?<a name="cm-feedback-loop-q7"></a>

No\. There are two primary reasons for this:
+ The complaint rate is calculated using representative volume \(see [Q6\. Over what period of time is my complaint rate calculated?](#cm-feedback-loop-q6)\)\. Depending on your sending rate, your complaint rate can stretch farther back in time than the Amazon SES console or `GetSendStatistics` can retrieve\. However, if you regularly monitor your complaint rates using those methods, you should still have a good indicator that you can use to catch problems before they get to levels that trigger a probation or suspension\.
+ When calculating complaint rate, not every email counts\. Complaint rate is calculated as the percentage of complaints on mail sent to domains that send complaint feedback to Amazon SES\.

#### Q8\. How can I find out which email addresses complained?<a name="cm-feedback-loop-q8"></a>

Examine the complaint notifications that Amazon SES sends you through email or through Amazon SNS \(see [Monitoring Using Amazon SES Notifications](monitor-sending-using-notifications.md)\)\. However, different ISPs provide differing amounts of information, and some ISPs redact the complained recipient's email address before passing the complaint notification to Amazon SES\. To enable you to find the recipient's email address in the future, your best option is to store your own mapping between an identifier and the Amazon SES message ID that Amazon SES passes back to you when it accepts the email\. Note that Amazon SES doesn't retain any custom message IDs that you add\.

#### Q9\. If I haven't been monitoring my complaints, can you give me a list of addresses that have complained?<a name="cm-feedback-loop-q9"></a>

Unfortunately, we can't give you a comprehensive list\. However, you can monitor future complaints by email or through Amazon SNS\.

#### Q10\. Can I get a sample email?<a name="cm-feedback-loop-q10"></a>

We can't send you a sample email upon request, but you might find this information in the complaint notification\. For more information, see [Q8\. How can I find out which email addresses complained?](#cm-feedback-loop-q8)\.

### Amazon SES Complaints Directly from Recipients FAQ<a name="cm-direct"></a>

This topic provides information about complaints that Amazon SES receives directly from recipients\. For general information that applies to all types of complaints, see the [Amazon SES Complaint FAQ](#e-faq-cm)\.

#### Q1\. How is this type of complaint reported?<a name="cm-direct-q1"></a>

Multiple recipients directly contacted Amazon SES about your mail through email or some other means\. 

#### Q2\. Are these complaints included in the complaint rate statistic shown in the Amazon SES console and returned by the GetSendStatistics API?<a name="cm-direct-q2"></a>

No\. The complaint rate statistic you retrieve using the Amazon SES console or the `GetSendStatistics` API only includes complaints that Amazon SES receives through ISP feedback loops\. For more information about those types of complaints, see the [Amazon SES Complaints Through ISP Feedback Loops FAQ](#cm-feedback-loop)\.

#### Q3\. Why haven't I heard about these complaints through email feedback notifications or through Amazon SNS?<a name="cm-direct-q3"></a>

Email feedback forwarding and Amazon SNS notifications only include complaints that Amazon SES receives through ISP feedback loops\. You won't receive notifications for complaints that recipients filed directly with Amazon SES\.

#### Q4\. How can I find out which email addresses complained?<a name="cm-direct-q4"></a>

To protect the identities of the recipients who complained, we can't list the email addresses that complained about your email\.

Rather than focus on removing individual recipients from your lists, we recommend that you determine the problem that led to the complaints being issued\. We recommend that you begin by reviewing your customer acquisition process, and that you remove any customers from your lists that didn't explicitly ask to receive email from you\. You should also analyze the content of your emails to try to understand why your recipients are complaining\. 

#### Q5\. Can I get a sample email?<a name="cm-direct-q5"></a>

To protect the identities of the recipients who complained, we can't provide copies of the emails that caused your recipients to complain\.

#### Q6\. I have received a probation notice for direct recipient complaints\. What should I do?<a name="cm-direct-q6"></a>

Immediately change your sending processes so that you're only sending messages recipients who have specifically signed up to receive them\. Also, ensure that you're sending the type of content that your recipients signed up to receive\. Once you've completed these steps, reply to the probation notice\. In your reply, provide details about the changes that you've made in order to resolve the issue\. 

If you don't reply to the original probation notice within three weeks, and we're still receiving direct recipient complaints, we will suspend your account\.

### Amazon SES Complaints Through Email Providers FAQ<a name="cm-email-provider"></a>

This topic provides information about complaints that Amazon SES receives through email providers \(also called *mailbox providers*\)\. For general information that applies to all types of complaints, see the [Amazon SES Complaint FAQ](#e-faq-cm)\.

#### Q1\. How is this type of complaint reported?<a name="cm-email-provider-q1"></a>

An email provider reported to Amazon SES that a significant number of its customers marked your emails as spam\. The report was provided to Amazon SES through a means other than the feedback loops described in the [Amazon SES Complaints Through ISP Feedback Loops FAQ](#cm-feedback-loop)\.

#### Q2\. Are these complaints included in the complaint rate statistic shown in the Amazon SES console and returned by the GetSendStatistics API?<a name="cm-email-provider-q2"></a>

No\. The complaint rate statistic you retrieve using the Amazon SES console or the `GetSendStatistics` API only includes complaints that Amazon SES receives through ISP feedback loops\.

#### Q3\. Why haven't I heard about these complaints through email feedback notifications or through Amazon SNS?<a name="cm-email-provider-q3"></a>

Email feedback forwarding and Amazon SNS notifications only include complaints that Amazon SES receives through ISP feedback loops\.

#### Q4\. How can I find out which email addresses complained?<a name="cm-email-provider-q4"></a>

Email providers typically don't disclose this information\. However, rather than focusing on removing individual recipients from your list, you need to focus on finding and fixing the underlying problem\. Start by reviewing your list acquisition process and the content of your emails to try to understand why your recipients might not appreciate your email\. 

#### Q5\. Can I get a sample email?<a name="cm-email-provider-q5"></a>

No\. Email providers typically don't provide an example email\.

#### Q6\. I have received a probation or shutdown notice for this type of complaint\. What should I do?<a name="cm-email-provider-q6"></a>

Fix your system so that your mailing list only includes recipients who have specifically signed up to receive your mail, and ensure that the email content itself is something your recipients actually want\. Then, please email us with the details of your changes so that we can start the process of re\-evaluating your case\. 

If you don't reply to the original probation notice within three weeks, and we're still receiving complaints from providers, we will suspend your account\.

## Amazon SES Spamtrap FAQ<a name="e-faq-st"></a>

### Q1\. What are spamtraps?<a name="st-q1"></a>

A spamtrap is a special email address maintained by an email provider, ISP, or anti\-spam organization\. Because that address will never legitimately be signed up to receive email, the organizations that maintain these spamtraps know that anyone who sends mail to any of these addresses is likely to be engaging in questionable email practices\.

### Q2\. How are spamtraps set up?<a name="st-q2"></a>

Spamtrap addresses can be set up in multiple ways\. They can be converted from addresses that were once valid, but have been unused \(and bouncing\) for an extended period of time\. They can also be addresses that were set up just to be spamtraps\. They can be unusual addresses that are hard to guess, and sometimes they are addresses that are close to real addresses \(for example, introducing a typo into a common domain name\)\. Often, but not always, spamtraps are "seeded" into the world by putting them on the internet in a variety of ways\.

### Q3\. How does Amazon SES know if I am sending to spamtraps?<a name="st-q3"></a>

Certain organizations that operate spamtraps send Amazon SES notifications when their spamtraps are hit by Amazon SES senders\.

### Q4\. How does Amazon SES use the spamtrap reports?<a name="st-q4"></a>

We review the reports, and if we find enough evidence that you have a problem with sending to spamtraps, we will put you on probation and ask you to fix the underlying problem\. If you don't fix the problem before the probation period is over, your account will be suspended\. Also, if your spamtrap problem is very severe, you might be immediately suspended without a probation period\. As with any suspension, we will send you a notification at that time\.

### Q5\. What should I do if I receive a probation or suspension notice for sending to spamtraps?<a name="st-q5"></a>

Fix the underlying problem and appeal to get your case reevaluated\. For information about the appeal process, see the FAQs on probation and suspension\. Due to the way spamtrap sending is reported, it will take a minimum of three weeks before we can confirm that a fix you've put in place has succeeded\. 

### Q6\. How many spamtrap hits can I have before I am put on probation or suspended?<a name="st-q6"></a>

Spamtrap hits are a very negative sign, so it takes only a small number of them to indicate that you're engaging in questionable sending practices\.

### Q7\. Do you disclose the spamtrap addresses?<a name="st-q7"></a>

No\. In order for spamtraps to be effective, it's essential that they remain confidential\. Spamtrap organizations disclose only the occurrence of spamtrap hits, not the actual spamtrap addresses\.

### Q8\. What can I do to avoid sending to spamtraps?<a name="st-q8"></a>

To reduce the risk of sending to spamtraps, follow these guidelines:
+ Do not buy, rent, or share email addresses\. Use only addresses that specifically requested your mail\.
+ On web forms, ask users to enter their email addresses two times, and check to make sure both addresses match before the form can be submitted\.
+ Use double opt\-in to sign up new users\. That is, when users sign up, send them a confirmation email that they need to click before receiving any additional mail\.
+ Ensure that you remove addresses that hard bounce from your list, so that they are removed long before they are converted to spamtraps\.
+ Ensure that you're monitoring engagement by your recipients, and stop sending to recipients who haven't engaged with your emails or website recently\. Time frames for what an "engaged user" is depend on your use case, but generally speaking if users haven't opened or clicked your emails in several months, you should consider removing them unless you have evidence that they do want your mail\.
+ Be very careful with re\-engagement campaigns where you intentionally contact people who haven't interacted with you recently\. These efforts tend to be highly risky, and can often cause problems not only with spamtrap sending, but also with bounces and complaints\.
+ Send an opt\-in message to your entire mailing list and keep only the recipients who click on the verification link\. In addition to removing inactive recipients from your list, this procedure will remove spamtrap addresses as well\. However, we don't recommend using this technique if you think that your mailing list might contain a lot of bad addresses and/or you already have a problem with bounces, because it might cause your bounce rate to reach the point at which your sending is put on probation or shut down\.

## Amazon SES Manual Investigation FAQ<a name="e-faq-mi"></a>

### Q1\. I received a probation or shutdown notice for a manual investigation\. What does that mean?<a name="mi-q1"></a>

An Amazon SES investigator has identified a significant problem with your sending\. Typical problems include, but aren't limited to, the following:
+ Your sending violates the [AWS Acceptable Use Policy](https://aws.amazon.com/aup/) \(AUP\)\.
+ Your emails appear to be unsolicited\.
+ Your content is associated with a use case that Amazon SES doesn't support\.

If the problem is correctable, your account is put on probation and you're given a certain amount of time \(rather than a certain volume of mail, as with bounces and complaints\) to correct the problem\. If the problem is uncorrectable, your account is suspended without a probation period\.

### Q2\. Why would you do a manual investigation?<a name="mi-q2"></a>

There are a variety of reasons\. These include, but aren't limited to, the following:
+ Recipients contact Amazon SES to complain about your emails\.
+ We detect a significant change in your sending patterns\.
+ The spam filters of Amazon SES flag a significant portion of your emails\.

The probation or suspension notification indicates the issue at a high level\. For some problems, we are able to provide more specific details\.

### Q3\. What are "unsolicited" emails?<a name="mi-q3"></a>

Unsolicited emails are emails that the recipient didn't explicitly ask to receive\. This includes cases in which a recipient signs up for a certain type of mail \(for example, notifications\), and instead is sent a different type of mail \(for example, advertisements\)\. If the probation or suspension notice indicates that unsolicited sending is your problem, you should provide the following information in your appeal:
+ Are all the messages that you send specifically requested by the recipient, and do they comply with the [AWS Acceptable Use Policy](https://aws.amazon.com/aup/)?
+ Have you acquired email addresses in any way other than a customer specifically interacting with you or your website and requesting emails from it? You should explain how you acquired your mailing list\.
+ How do your subscribe and unsubscribe processes work? You should include your opt\-in and opt\-out links\.

### Q4\. What should I do if I receive a probation or suspension notice for a manual investigation?<a name="mi-q4"></a>

As with any probation or suspension, fix the underlying problem that is causing the issue specified in the probation or suspension notice, and then appeal to get your case reevaluated\. For information about the appeal process, see the FAQs on probation and suspension\. 

### Q5\. What types of problems do you view as "correctable?"<a name="mi-q5"></a>

Generally, we believe the situation is correctable if you have a history of good sending practices, and if there are steps you can take to eliminate the problematic sending while continuing the bulk of your sending\. For example, if you're sending three different types of email and only one type is problematic, you might be able to simply stop the problematic sending and continue with the rest of your sending\.

### Q6\. What if I can't find the source of the problem?<a name="mi-q6"></a>

You can respond to the notification \(or email *ses\-enforcement@amazon\.com* from the email address associated with your AWS account\) and request a sample of the mail that caused the issue\.