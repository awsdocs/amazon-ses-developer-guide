# Specifying a configuration set when you send email<a name="using-configuration-sets-in-email"></a>

To use a configuration set when sending an email, you must pass the name of the configuration set in the headers of the email\. All of the Amazon SES email sending methods—including the [AWS CLI](https://aws.amazon.com/cli), the [AWS SDKs](https://aws.amazon.com/tools/#sdk), and the [Amazon SES SMTP interface](send-email-smtp.md)—allow you to pass a configuration set in the headers of the email you send\.

If you are using the [SMTP interface](send-email-smtp.md) or the [`SendRawEmail` API operation](/ses/latest/APIReference/API_SendRawEmail.html), you can specify a configuration set by including the following header in your email \(replacing `ConfigSet` with the name of the configuration set you want to use\):

```
X-SES-CONFIGURATION-SET: ConfigSet
```

This guide includes code examples for sending email using Postfix, the AWS SDKs, and the Amazon SES SMTP interface\. Each of these examples includes a method of specifying a configuration set\. To see step\-by\-step procedures for sending emails that include references to configuration sets, see the following: 
+ [Integrating Amazon SES with Postfix](postfix.md)
+ [Sending email through Amazon SES using an AWS SDK](send-an-email-using-sdk-programmatically.md)
+ [Using the Amazon SES SMTP interface to send email](send-email-smtp.md)