# Using subscription management<a name="sending-email-subscription-management"></a>

Amazon SES provides a subscription management capability, in which Amazon SES automatically enables the unsubscribe links in every outgoing email when you specify the contactListName and topicName within ListManagementOptions in the [SendEmail](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_SendEmail.html) operation request\.

If a contact unsubscribes from a particular topic or list, Amazon SES does not allow email sending to the contact for that topic or list in the future\.

**Note**  
Subscription management is available for those using [Easy DKIM in Amazon SES](send-email-authentication-dkim-easy.md), but it’s not possible for Amazon SES to add the unsubscribe links to your email for senders who are signing emails themselves before calling Amazon SES\.

For information about list management, see [Using list management](sending-email-list-management.md)\.

## Subscription management overview<a name="subscription-management-overview"></a>

You should consider the following factors when you use subscription management:
+ Subscription management will be fully managed by Amazon SES\. This means that Amazon SES receives unsubscribe emails and requests from the unsubscribe webpage and then updates the contact’s preference in your list\. You can receive unsubscribe notifications using configuration set notifications\. For more information about configuration sets, see [Using Amazon SES configuration sets](using-configuration-sets.md)\.
+ You need to specify the contact list while sending the email\. Subscription management via the Link\-Unsubscribe header and footer links will be handled accordingly\. 
+ Amazon SES adds support for the List\-Unsubscribe header standards, which will enable email clients and inbox providers to display an unsubscribe link at the top of the email if they support it\.
+ List\-Unsubscribe headers follow the following behavior:

  If a contact clicks the List\-Unsubscribe header link in an email which has both the contact list and topic specified, then the contact will be unsubscribed only from that specific topic\.

  If the topic is not specified, then the contact will be unsubscribed from all the topics in the list\.
+ Contacts will be taken to an unsubscribe landing page when they click an unsubscribe link in the email footer\.
+ The unsubscribe landing page will give contacts an option to update their preferences, meaning `OPT_IN` or `OPT_OUT`, for all the topics in a particular list\. The landing page also gives an option to unsubscribe from all topics in the list\.
+ You must include a placeholder in your emails to indicate where Amazon SES needs to insert the unsubscribe URL\. You can include the placeholder two times maximum\. If used more than two times, only the first two occurrences are replaced\.
+ The List\-Unsubscribe header and footer link are added only if the email is being sent to a single recipient, and the placeholder must present in order to add the unsubscribe link\.
+ For transactional emails where you don't want contacts to be able to unsubscribe, you can omit the ListManagementOptions field with your [SendEmail](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_SendEmail.html) request\.

## Adding an unsubscribe footer link<a name="adding-unsubscribe-footer"></a>

You will need to use the `{{amazonSESUnsubscribeUrl}}` placeholder in templated and non\-templated emails to specify where Amazon SES needs to insert the unsubscribe URL\.

Placeholder replacement is supported only for HTML and TEXT content types\.

You can include the placeholder two times maximum\. If used more than two times, only the first two occurrences are replaced\.

## Unsubscribe header considerations<a name="unsubscribe-header-considerations"></a>

Subscription management through an unsubscribe header is enabled by the the following two headers, which Amazon SES adds\. If you’re adding these headers in your own email before sending them to Amazon SES, Amazon SES overrides them if you have enabled ListManagementOptions\.

`List-Unsubscribe`

`List-Unsubscribe-Post`

Contacts who unsubscribe by clicking the link provided by the unsubscribe header will have a different experience depending on their email client or inbox provider\. Users who unsubscribe via the link in the header will not have the option of choosing which topics they unsubscribe from, and they will be unsubscribed from the topic to which the email was sent\.

For more information about message headers, see [RFC 2369](https://tools.ietf.org/html/rfc2369) and [RFC 8058](https://tools.ietf.org/html/rfc8058)\.
