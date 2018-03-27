# Authenticating Your Email in Amazon SES<a name="authentication"></a>

Amazon Simple Email Service \(Amazon SES\) uses the Simple Mail Transfer Protocol \(SMTP\) to send email\. Because SMTP does not provide any authentication by itself, spammers can send email messages that claim to originate from someone else, while hiding their true origin\. By falsifying email headers and spoofing source IP addresses, spammers can mislead recipients into believing that the email messages that they are receiving are authentic\.

Most ISPs that forward email traffic take measures to evaluate whether email is legitimate\. One such measure that ISPs take is to determine whether an email is *authenticated*\. Authentication requires senders to verify that they are the owner of the account that they are sending from\. In some cases, ISPs refuse to forward email that is not authenticated\. To ensure optimal deliverability, we recommend that you authenticate your emails\.

The following sections describe two authentication mechanisms ISPs use—Sender Policy Framework \(SPF\) and DomainKeys Identified Mail \(DKIM\)—and provide instructions for how to use these standards with Amazon SES\. 
+ To learn about SPF, which provides a way to trace an email message back to the system from which it was sent, see [Authenticating Email with SPF in Amazon SES](spf.md)\.
+ To learn about DKIM, a standard that allows you to sign your email messages to show ISPs that your messages are legitimate and have not been modified in transit, see [Authenticating Email with DKIM in Amazon SES](dkim.md)\.
+ To learn how to comply with Domain\-based Message Authentication, Reporting and Conformance \(DMARC\), which relies on SPF and DKIM, see [Complying with DMARC Using Amazon SES](dmarc.md)\.


****  

|  | 
| --- |
| For technical discussions about various Amazon SES topics, visit the [Amazon SES Blog](https://aws.amazon.com//blogs/ses/)\. To browse and post questions, go to the [Amazon SES Forum](https://forums.aws.amazon.com/forum.jspa?forumID=90)\. | 