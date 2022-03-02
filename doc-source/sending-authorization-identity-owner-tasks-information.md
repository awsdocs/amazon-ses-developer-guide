# Getting information from the delegate sender for Amazon SES sending authorization<a name="sending-authorization-identity-owner-tasks-information"></a>

Your sending authorization policy must specify at least one *principal*, which is the entity of your delegate sender that you're granting access to so they can send on behalf of one of your verified identities\. For Amazon SES sending authorization policies, the principal can be either your delegate sender's AWS account or AWS Identity and Access Management \(IAM\) user ARN, or an AWS service\.

An easy way to think about this is that the *principal* \(delegate sender\) is the grantee, and you \(identity owner\) are the grantor in the authorization policy where you are granting them the *Allow* permission to send any combination of email, raw email, templated email, or bulk templated email from the *resource* \(verified identity\) that you own\.

If you want the finest grain control, ask the delegate sender to set up an IAM user so that only one delegate sender can send for you rather than any user in the delegate sender's AWS account\. The delegate sender can find information about setting up an IAM user in [Creating an IAM User in Your AWS Account](https://docs.aws.amazon.com/IAM/latest/UserGuide/Using_SettingUpUser.html) in the *IAM User Guide*\.

Ask your delegate sender for the AWS account ID or the IAM user's Amazon Resource Name \(ARN\) so that you can include it in your sending authorization policy\. You can refer your delegate sender to the instructions for finding this information in [Providing information to the identity owner](sending-authorization-delegate-sender-tasks-information.md)\. If the delegate sender is an AWS service, see the documentation for that service to determine the service name\.

The following example policy illustrates the basic elements of what is needed in a policy created by the identity owner to authorize the delegate sender to send from the identity owner's resource\. The identity owner would go into the Verified identities workflow, and under Authorization, use the Policy generator to create, in its simplest form, the following basic policy allowing the delegate sender to send on behalf of a resource owned by the identity owner:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/ses/latest/dg/images/simple_auth_policy_for_delegate_sender.png)

For the policy above, the following legend explains the key elements and who owns them:
+ **Principal** – this field is populated with the delegate sender's IAM user ARN\.
+ **Action** – this field is populated with two SES actions \(`SendEmail` & `SendRawEmail`\) that the identity owner is allowing the delegate sender to perform from the identity owner's resource\.
+ **Resource** – this field is populated with the identity owner's verified resource that they are authorizing the delegate sender to send from\.