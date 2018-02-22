# Amazon SES Email Sending Metrics FAQs<a name="sending-metric-faqs"></a>

Amazon SES collects several metrics about the emails you send\. These metrics enable you to analyze the effectiveness of your email program and monitor important statistics, such as your bounce and complaint rates\.

This section contains FAQs on the following topics related to email sending metrics:

+ [General Questions](#sending-metric-faqs-general)

+ [Open Tracking](#sending-metric-faqs-opens)

+ [Click Tracking](#sending-metric-faqs-clicks)

## General Questions<a name="sending-metric-faqs-general"></a>

### Q1\. After an email is delivered, how long will Amazon SES continue to collect open and click metrics?<a name="sending-metric-faqs-general-q1"></a>

Amazon SES collects open and click metrics for 60 days after each email is sent\.

### Q2\. If a user opens an email multiple times, or clicks a link in an email multiple times, is each of those events tracked separately?<a name="sending-metric-faqs-general-q2"></a>

If a recipient opens an email multiple times, each instance is counted as a unique open event\. Similarly, if a recipient clicks the same link multiple times, each click is counted as a unique click event\.

### Q3\. Are open and click metrics aggregated, or can they be measured down to the recipient level?<a name="sending-metric-faqs-general-q3"></a>

Opens and clicks are tracked at the recipient level\. With open and click tracking, you can determine which recipients opened an email or clicked a link in an email\.

### Q4\. Can I retrieve open and click metrics using the Amazon SES API?<a name="sending-metric-faqs-general-q4"></a>

The Amazon SES API does not provide a method for retrieving open and click metrics\. However, you can retrieve open and click metrics for Amazon SES using the CloudWatch API\. For example, you can use the AWS CLI to retrieve click metrics using the CloudWatch API by issuing the following command:

```
1. aws cloudwatch get-metric-statistics --namespace AWS/SES --metric-name Click \
2.   --statistics Sum --period 86400 --start-time 2017-01-01T00:00:00Z \
3.   --end-time 2017-12-31T23:59:59Z
```

The command shown above retrieves the total number of click events for each day in 2017\. To retrieve open metrics change the value of the `metric-name` parameter to `Open`\. You can also modify the `start-time` and `end-time` parameters to change the analysis period, or change the `period` parameter for more fine\-grained analysis\.

## Open Tracking<a name="sending-metric-faqs-opens"></a>

### Q1\. How does open tracking work?<a name="sending-metric-faqs-opens-q1"></a>

At the bottom of each email sent through Amazon SES, we insert a 1 pixel by 1 pixel transparent GIF image\. Each email includes a unique reference to this image file; when the image is opened, Amazon SES can tell exactly which message was opened and by whom\.

The addition of this tracking pixel does not change the appearance of your email\.

### Q2\. Is open tracking enabled by default?<a name="sending-metric-faqs-opens-q2"></a>

Open tracking is available to all Amazon SES users by default\. To use open tracking, you must do the following:

1. Create a configuration set\.

1. In the configuration set, create an event destination\.

1. Configure the event destination to publish open event notifications to a destination\.

1. In every email for which you want to track opens, specify the configuration set that you created in step 1\.

For a more detailed explanation of this process, see [Monitoring Using Amazon SES Event Publishing](monitor-using-event-publishing.md)\.

### Q3\. Can I omit the open tracking pixel from certain emails?<a name="sending-metric-faqs-opens-q3"></a>

There are two ways to omit the open tracking pixel from your emails\. The first method is to send the email without specifying a configuration set\. Alternatively, you can specify a configuration set that is not configured to publish data about open events\.

### Q4\. Do you track opens for plaintext emails?<a name="sending-metric-faqs-opens-q4"></a>

Open tracking only works with HTML emails\. Because open tracking relies on the inclusion of an image, it is not possible to collect open metrics for users who open emails using a text\-only \(non\-HTML\) email client\.

## Click Tracking<a name="sending-metric-faqs-clicks"></a>

### Q1\. How does click tracking work?<a name="sending-metric-faqs-clicks-q1"></a>

To track clicks, Amazon SES modifies each link in the body of the email\. When recipients click a link, they are sent to an Amazon SES server, and are immediately forwarded to the destination address\. As with open tracking, each redirect link is unique\. This enables Amazon SES to determine which recipient clicked the link, when they clicked it, and the email from which they arrived at the link\.

**Important**  
If you send a single message to multiple recipients, each recipient will save the same click tracking link\. To track individual recipients' click activity, send email to one recipient per send operation\.

### Q2\. Can I disable click tracking?<a name="sending-metric-faqs-clicks-q2"></a>

You can disable click tracking by adding an attribute, `ses:no-track`, to the anchor tags in the HTML body of your email\. For example, if you link to the AWS home page, a normal anchor link resembles the following:

```
<a href="https://aws.amazon.com">Amazon Web Services</a>
```

To disable click tracking for that same link, modify the link to resemble the following:

```
<a ses:no-track href="aws.amazon.com">Amazon Web Services</a>
```

Because `ses:no-track` is not a standard HTML attribute, we automatically remove it from the version of the email that arrives in your recipients' inboxes\.

### Q3\. Is there a limit to the number of links that can be tracked in each email?<a name="sending-metric-faqs-clicks-q3"></a>

There is a limit to the number of links that can be tracked in a single email, although you are highly unlikely to encounter this limit in practice\. Currently, the click tracking system will track a maximum of 250 links\.

### Q4\. Are click metrics collected for links in plaintext emails?<a name="sending-metric-faqs-clicks-q4"></a>

In order to use click tracking, you must send HTML emails\. Links in plaintext emails are not tracked by Amazon SES\.

### Q5\. Can I tag links with unique identifiers?<a name="sending-metric-faqs-clicks-q5"></a>

You can add an unlimited number of tags, as key\-value pairs, to links in your email by using the `ses:tags` attribute\. When you use this attribute, specify the keys and values using the same format that you would use to pass inline CSS properties: type the key, followed by a colon \(:\), followed by the value\. If you need to pass several key\-value pairs, separate each pair with a semicolon \(;\)\.

For example, assume you want to add the tags `product:book, genre:fiction, subgenre:scifi, type:newrelease` to a link\. The resulting link resembles the following:

```
<a ses:tags="product:book;genre:fiction;subgenre:scifi;type:newrelease;" 
    href="http://www.amazon.com/…/">New Releases in Science Fiction</a>
```

These tags are passed through to your event publishing destination so that you can perform additional analysis on the specific links that your users clicked\.

### Q6\. Do tracked links use the HTTP or HTTPS protocol?<a name="sending-metric-faqs-clicks-q6"></a>

Tracking links use the same protocol as the original links in your email\.

For example, if your email includes a link to `https://www.amazon.com`, the link is replaced with a tracking link that uses the HTTPS protocol\. If your email includes a link to `http://www.example.com`, the link is replaced with a tracking link that uses HTTP\. If your email includes both of the previously mentioned links, the HTTPS link is replaced with a tracking link that uses the HTTPS protocol, and the HTTP link is replaced with a tracking link that uses the HTTP protocol\.