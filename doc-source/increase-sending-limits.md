# Increasing Your Amazon SES Sending Limits<a name="increase-sending-limits"></a>

When your account is out of the sandbox, your sending limits will increase if you are sending high\-quality content and we detect that your utilization is approaching your current limits\. Often, the system automatically increases your quota before you actually need it, and no further action is needed\.

If your existing quota is not adequate for your needs and the system has not automatically increased your quota, you can open an [SES Sending Limits Increase case](https://aws.amazon.com/ses/extendedaccessrequest/) in Support Center\. 

For a list of the information that you need when you open the case, see [Opening an SES Sending Limits Increase Case](submit-extended-access-request.md)\.

**Important**  
Plan ahead\. Be aware of your sending limits and try to stay within them\. If you anticipate needing a higher quota than the system has allocated automatically, open an SES Sending Limits Increase case well before the date that you need the higher quota\.

**Important**  
If you anticipate needing to send more than one million emails per day, you must open an SES Sending Limits Increase case\.

For Amazon SES to increase your quota, use the following guidelines:
+ **Send high\-quality content**—Send content that recipients want and expect\. 
+ **Send real production content**—Send your actual production email\. This enables Amazon SES to accurately evaluate your sending patterns, and verify that you are sending high\-quality content\.
+ **Send near your current quota**—If your volume stays close to your quota without exceeding it, Amazon SES can detect this usage pattern and automatically increase your quota\.
+ **Have low bounce and complaint rates**—Try to minimize the numbers of bounces and complaints\. Having high numbers of bounces and complaints can adversely affect your sending limits\.

**Important**  
Test emails that you send to your own email addresses may adversely affect your bounce and complaint metrics, or appear as low\-quality content to our filters\. Whenever possible, use the Amazon SES mailbox simulator to test your system\. Emails that are sent to the mailbox simulator do not count toward your sending metrics or your bounce and complaint rates\. For more information, see [Testing Amazon SES Email Sending](mailbox-simulator.md)\. 

For information about opening an SES Sending Limits Increase case, see [Opening an SES Sending Limits Increase Case](submit-extended-access-request.md)\.