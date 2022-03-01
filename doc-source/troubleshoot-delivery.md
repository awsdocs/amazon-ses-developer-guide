# Amazon SES Delivery problems<a name="troubleshoot-delivery"></a>

After you make a successful request to Amazon SES, your message is often sent immediately\. At other times, there might be a short delay\. In any case, you can be assured that your email will be sent\.

When Amazon SES sends your message, however, several factors can prevent it from being delivered successfully, and in some cases you will become aware that delivery failed only when the message you send does not arrive\. Use the following process to resolve this situation\.

If an email does not arrive, try the following:
+ Verify that you made a `SendEmail` or `SendRawEmail` request for the email in question and that you received a successful response\. If you are making these requests programmatically, check your software logs to ensure that the program made the request and received a successful response\.
+ Read the blog article [Three places where your email could get delayed when sending through SES](https://aws.amazon.com//blogs/messaging-and-targeting/three-places-where-your-email-could-get-delayed-when-sending-through-ses/) because the problem might actually be a delay rather than a nondelivery\.
+ Check the sender's email address \(the "From" address\) to verify that it is valid\. Also check the Return\-Path address, which is where bounce messages are sent\. If your mail bounced, there will be an explanatory error message there\.
+ Check the [AWS Service Health Dashboard](http://status.aws.amazon.com/) to confirm that there is not a known problem with Amazon SES\.
+ Contact the email recipient or the recipient's ISP\. Verify that the recipient is using the correct email address, and inquire whether there have been any known delivery problems with the recipient's ISP\. Also, determine whether the email did arrive but was filtered as spam\.
+ If you have signed up for a paid [AWS Support Plan](https://aws.amazon.com/premiumsupport/), you can open a new technical support case\. In your correspondence with us, please provide any relevant recipient addresses, along with any request IDs or message IDs returned from the `SendEmail` or `SendRawEmail` responses\.
+ Wait to see if the problem is actually a delay, not a permanent delivery failure\. To combat spammers, some ISPs temporarily reject incoming messages from unknown sending mail servers\. This process, called *greylisting*, can cause a delay in delivery\. Amazon SES will retry these messages\. If greylisting is the issue, the ISP might accept the email on one of these retry attempts\. 
