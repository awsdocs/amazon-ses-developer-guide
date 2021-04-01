# Managing Easy DKIM<a name="send-email-authentication-dkim-easy-managing"></a>

There are two ways to manage the Easy DKIM settings for your identities: by using the web\-based Amazon SES console, or by using the Amazon SES API\. You can use either of these methods to obtain the DKIM records for an identity, or to enable or disable Easy DKIM for an identity\. 

## Obtaining Easy DKIM Records for An Identity<a name="send-email-authentication-dkim-easy-managing-obtain-records"></a>

You can obtain the Easy DKIM records for your domain or email address at any time by using the Amazon SES console\.

**To obtain the Easy DKIM records for an identity by using the console**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane, under **Identity Management**, choose the type of identity that you want to obtain Easy DKIM records for\.

1. In the list of identities, choose the identity that you want to obtain Easy DKIM records for\.

1. In the **DKIM** section, copy the three CNAME records\. The following image shows an example of the **DKIM** section\.  
![\[\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/dkim_existing_dns.png)

You can also obtain the CNAME records for an identity by using the Amazon SES API\. A common method of interacting with the API is to use the AWS CLI\.

**To obtain the Easy DKIM records for an identity by using the AWS CLI**

1. At the command line, type the following command:

   ```
   aws ses get-identity-dkim-attributes --identities "example.com"
   ```

   In the preceding example, replace *example\.com* with the identity that you want to obtain Easy DKIM records for\. You can specify either an email address or a domain\.

1. The output of this command contains a `DkimTokens` section, as shown in the following example:

   ```
   {
       "DkimAttributes": {
           "example.com": {
               "DkimEnabled": true,
               "DkimVerificationStatus": "Success",
               "DkimTokens": [
                   "hirjd4exampled5477y22yd23ettobi",
                   "v3rnz522czcl46quexamplek3efo5o6x",
                   "y4examplexbhyhnsjcmtvzotfvqjmdqoj"
               ]
           }
       }
   }
   ```

   You can use the tokens to create the CNAME records that you add to the DNS settings for your domain\. To create the CNAME records, use the following template:

   ```
   token1._domainkey.example.com CNAME token1.dkim.amazonses.com
   token2._domainkey.example.com CNAME token2.dkim.amazonses.com
   token3._domainkey.example.com CNAME token3.dkim.amazonses.com
   ```

   Replace each instance of *token1* with the first token in the list you received when you ran the aws ses `get-identity-dkim-attributes` command, replace all instances of *token2* with the second token in the list, and replace all instances of *token3* with the third token in the list\. 

   For example, applying this template to the tokens shown in the preceding example produces the following records:

   ```
   hirjd4exampled5477y22yd23ettobi._domainkey.example.com CNAME hirjd4exampled5477y22yd23ettobi.dkim.amazonses.com
   v3rnz522czcl46quexamplek3efo5o6x._domainkey.example.com CNAME v3rnz522czcl46quexamplek3efo5o6x.dkim.amazonses.com
   y4examplexbhyhnsjcmtvzotfvqjmdqoj._domainkey.example.com CNAME y4examplexbhyhnsjcmtvzotfvqjmdqoj.dkim.amazonses.com
   ```

## Disabling Easy DKIM for an Identity<a name="send-email-authentication-dkim-easy-managing-disabling"></a>

You can quickly disable DKIM authentication for an identity by using the Amazon SES console\.

**To disable Easy DKIM for an identity**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane, under **Identity Management**, choose the type of identity that you want to disable Easy DKIM for\.

1. In the list of identities, choose the identity that you want to disable Easy DKIM for\.

1. In the **DKIM** section, next to **DKIM: enabled**, choose **disable**, as shown in the following image\.  
![\[\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/dkim_disable.png)

You can also disable Easy DKIM for an identity by using the Amazon SES API\. A common method of interacting with the API is to use the AWS CLI\.

**To disable Easy DKIM for an identity by using the AWS CLI**
+ At the command line, type the following command:

  ```
  aws ses set-identity-dkim-enabled --identity example.com --no-dkim-enabled
  ```

  In the preceding example, replace *example\.com* with the identity that you want to disable Easy DKIM for\. You can specify either an email address or a domain\.

## Enabling Easy DKIM for an Identity<a name="send-email-authentication-dkim-easy-managing-enabling"></a>

If you previously disabled Easy DKIM for an identity, you can enable it again by using the Amazon SES console\.

**To enable Easy DKIM for an identity**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation pane, under **Identity Management**, choose the type of identity that you want to enable Easy DKIM for\.

1. In the list of identities, choose the identity that you want to enable Easy DKIM for\.

1. In the **DKIM** section, next to **DKIM: disabled**, choose **enable**, as shown in the following image\.  
![\[\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/dkim_reenable.png)

You can also enable Easy DKIM for an identity by using the Amazon SES API\. A common method of interacting with the API is to use the AWS CLI\.

**To enable Easy DKIM for an identity by using the AWS CLI**
+ At the command line, type the following command:

  ```
  aws ses set-identity-dkim-enabled --identity example.com --dkim-enabled
  ```

  In the preceding example, replace *example\.com* with the identity that you want to enable Easy DKIM for\. You can specify either an email address or a domain\.
