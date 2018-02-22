# Tips and Best Practices<a name="tips-and-best-practices"></a>

Even when you have your customers' best interests in mind, you may still encounter situations that impact the deliverability of your messages\. The following sections contain recommendations to help ensure that your email communications reach your intended audience\.

## General Recommendations<a name="tips-and-best-practices-general-recommendations"></a>

+ Put yourself in your customer's shoes\. Ask yourself if the message you are sending is something you would want to receive in your own inbox\. If the answer is anything less than an enthusiastic "yes\!" then you probably shouldn't send it\.

+ Some industries have a reputation for poor quality or even malicious email practices\. If you are involved in the following industries, you must monitor your reputation very closely and resolve issues immediately:

  + Home mortgage

  + Credit

  + Pharmaceuticals and supplements

  + Alcohol and tobacco

  + Adult entertainment

  + Casinos and gambling

  + Work\-from\-home programs

## Domain and "From" Address Considerations<a name="domain-and-from-address-considerations"></a>

+ Think carefully about the addresses you send email from\. The "From" address is one of the first pieces of information your recipients see, and therefore can leave a lasting first impression\. Additionally, some ISPs associate your reputation with your "From" address\.

+ Consider using subdomains for different types of communications\. For example, assume you are sending email from the domain *example\.com*, and you plan to send both marketing and transactional messages\. Rather than sending all of your messages from *example\.com*, send your marketing messages from a subdomain such as *marketing\.example\.com*, and your transactional messages from a subdomain such as *orders\.example\.com*\. Unique subdomains develop their own reputations\. Using subdomains reduces the risk of damage to your reputation if, for example, your marketing communications land in a spam trap or trigger a content filter\.

+ If you plan to send a large number of messages, don't send those messages from an ISP\-based address such as *sender@hotmail\.com*\. If an ISP notices a large volume of messages coming from *sender@hotmail\.com*, that email is treated differently than an email that comes from an outbound email sending domain that you own\.

+ Work with your domain registrar to ensure that the WHOIS information for your domain is accurate\. Maintaining an honest and up\-to\-date WHOIS record demonstrates that you value transparency, and allows users to quickly identify whether or not your domain is legitimate\.

+ Avoid using a *no\-reply* address, such as *no\-reply@example\.com*, as your "From" or "Reply\-to" address\. Using a *no\-reply@* email address sends your recipients a clear message: that you aren't offering them a way to contact you, and that you're not interested in their feedback\.

## Authentication<a name="authentication-considerations"></a>

+ Authenticate your domain with [SPF](spf.md) and SenderID\. These authentication methods confirm to email recipients that each email you send is actually from the domain it claims to be from\.

+ Sign your outbound mail with [DKIM](dkim.md)\. This step confirms to recipients that the content has not been changed in transit between sender and receiver\.

+ You can test your authentication settings for both SPF and DKIM by sending an email to an ISP\-based email address that you own, such as a personal Gmail or Hotmail account, and then viewing the message's headers\. The headers indicate whether your attempts to authenticate and sign the message were successful\.

## Building and Maintaining Your Lists<a name="building-and-maintaining-lists"></a>

+ Implement a double opt\-in strategy\. When users sign up to receive email from you, send them a message with a confirmation link, and do not start sending them email until they confirm their address by clicking that link\. A double opt\-in strategy helps reduce the number of hard bounces resulting from typographical errors\.

+ When collecting email addresses with a web\-based form, perform minimal validation on those addresses upon submission\. For example, ensure that the addresses you collect are well\-formed \(that is, they are in the format *recipient@example\.com*\), and that they refer to domains with valid MX records\.

+ Use caution when allowing user\-defined input to be passed to Amazon SES unchecked\. Forums registrations and form submissions present unique risks because the content is completely user\-generated, and spammers can fill out forms with their own content\. It's your responsibility to ensure that you only send email with high\-quality content\.

+ It is highly unlikely that a standard alias \(such as *postmaster@*, *abuse@*, or *noc@*\) will ever sign up for your email intentionally\. Ensure that you are only sending messages to real people who actually want to receive them\. This rule is especially true for standard aliases, which are customarily reserved for email watchdogs\. These aliases can be maliciously added to your list as a form of sabotage, in order to damage your reputation\.

## Compliance<a name="compliance-considerations"></a>

+ Be aware of the email marketing and anti\-spam laws and regulations in the countries and regions you send email to\. You're responsible for ensuring that the email you send complies with these laws\. This guide doesn't cover these laws, so it's important that you research them\. For a list of laws, see [Email Spam Legislation by Country](https://en.wikipedia.org/wiki/Email_spam_legislation_by_country) on Wikipedia\.

+ Always consult an attorney to obtain legal advice\.