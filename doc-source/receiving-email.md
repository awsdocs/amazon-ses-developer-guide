# Email receiving with Amazon SES<a name="receiving-email"></a>

Besides using Amazon Simple Email Service to manage your email sending, you can also configure Amazon SES to receive email on behalf of one or more of your domains\. As the email receiver, Amazon SES handles underlying mail\-receiving operations, such as communicating with other mail servers, scanning for spam and viruses, rejecting mail from untrusted sources, and accepting mail for recipients in your domain\.

The extent of processing on your received email is determined by the custom instructions you specify\. These instructions come in two forms:
+ **Receipt rules** *\(recipient\-based control\)* provide the finest granularity of control over incoming email\. Receipt rules can do advanced processing such as deliver incoming mail to an Amazon S3 bucket, publish it to an Amazon SNS topic, send it to Amazon WorkMail, or automatically send bounce messages when messages are to specific email addresses, and more\.
+ **IP address filters** *\(IP\-based control\)* provide a broad level of control and are simple to setup\. These filters allow you to explicitly block or allow all messages from specific IP addresses or IP address ranges\.

To get started with learning about email receiving, setting it up, and implementation using either *receipt rules* or *IP address filters*, first read through [Email receiving concepts & use cases](receiving-email-concepts.md) to get an overview of how it works and the different ways you can use it\. Next, [Setting up email receiving](receiving-email-setting-up.md) will guide you through the email receiving set up prerequisites\. Then, the [Email receiving console walkthroughs](receiving-email-walkthroughs.md) will guide you through the wizards used for configuring *receipt rules* and *IP address filters*\. 

**Topics**
+ [Amazon SES email receiving concepts and use cases](receiving-email-concepts.md)
+ [Setting up Amazon SES email receiving](receiving-email-setting-up.md)
+ [Amazon SES email receiving console walkthroughs](receiving-email-walkthroughs.md)
+ [Increasing your Amazon SES receiving quotas](manage-receiving-quotas-request-increase.md)