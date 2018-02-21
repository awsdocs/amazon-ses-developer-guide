# Appendix: Header Fields<a name="header-fields"></a>

Amazon SES accepts any email headers that follow the format described in [RFC 822](https://www.ietf.org/rfc/rfc0822.txt)\.

The following fields cannot appear more than once in a header:

+  Accept\-Language 

+  acceptLanguage \(**Note:** This field is nonstandard\. If possible, use Accept\-Language instead\.\) 

+  Archived\-At 

+  Auto\-Submitted 

+  Bounces\-to 

+  Comments 

+  Content\-Alternative 

+  Content\-Base 

+  Content\-Class 

+  Content\-Description 

+  Content\-Disposition 

+  Content\-Duration 

+  Content\-ID 

+  Content\-Language 

+  Content\-Length 

+  Content\-Location 

+  Content\-MD5 

+  Content\-Transfer\-Encoding 

+  Content\-Type 

+  Date \(**Note:** Amazon SES overrides any Date header you provide with the time that Amazon SES accepts the message\. The time zone of the Date header is UTC\.\)

+  Delivered\-To 

+  Disposition\-Notification\-Options 

+  Disposition\-Notification\-To 

+  DKIM\-Signature 

+  DomainKey\-Signature 

+  Errors\-To 

+  From 

+  Importance 

+  In\-Reply\-To 

+  Keywords 

+  List\-Archive 

+  List\-Help 

+  List\-Id 

+  List\-Owner 

+  List\-Post 

+  List\-Subscribe 

+  List\-Unsubscribe 

+  Message\-Context 

+  Message\-ID \(**Note:** Amazon SES overrides any Message\-ID header you provide\.\)

+  MIME\-Version 

+  Organization 

+  Original\-From 

+  Original\-Message\-ID 

+  Original\-Recipient 

+  Original\-Subject 

+  Precedence 

+  Priority 

+  References 

+  Reply\-To 

+  Return\-Path \(**Note:** After Amazon SES uses any Return\-Path header you provide, it removes that header before sending the email\.\)

+  Return\-Receipt\-To 

+  Sender 

+  Solicitation 

+  Sensitivity 

+  Subject 

+  Thread\-Index 

+  Thread\-Topic 

+  User\-Agent 

+  VBR\-Info 