# Amazon SES SMTP Issues<a name="smtp-issues"></a>

If you are having problems sending email through the Amazon SES Simple Mail Transfer Protocol \(SMTP\) interface, review the possible causes and solutions below\. For general information about sending email through the Amazon SES SMTP interface, see [Using the Amazon SES SMTP Interface to Send Email](send-email-smtp.md)\.
+ **You are unable to connect to the Amazon SES SMTP endpoint\.**

  Problems connecting to the Amazon SES SMTP endpoint are most commonly related to the following issues:
  + **Incorrect credentials** – The credentials that you use to connect to the SMTP endpoint are different from your AWS credentials\. To obtain your SMTP credentials, see [Obtaining Your Amazon SES SMTP Credentials](smtp-credentials.md)\. For more information about credentials, see [Using Credentials With Amazon SES](using-credentials.md)\.
  + **Network or firewall issues** – Your network might be blocking outbound connections over the port you're trying to send email from\. To determine if an issue on your local network is causing connection issues, type the following command at the command line, replacing `port` with the port you're trying to use \(typically 25, 465, 587, 2465, or 2587\): `telnet email-smtp.us-west-2.amazonaws.com port`

    If you are able to connect to the SMTP server using this command, and you are trying to connect to Amazon SES using TLS Wrapper or STARTTLS, complete the procedures shown in [Testing Email Sending Using the Command Line](send-email-smtp-client-command-line.md)\.

    If you cannot connect to the Amazon SES SMTP endpoint using telnet or openssl, it indicates that something in your network \(such as a firewall\) is blocking outbound connections over the port you're trying to use\. Work with your network administrator to diagnose and fix the problem\.
+ **You are sending to Amazon SES from an Amazon EC2 instance via port 25 and you cannot reach your Amazon SES sending limits or you are receiving time outs\.**

  Amazon EC2 imposes default sending limits on email sent via port 25 and throttles outbound connections if you attempt to exceed those limits\. To remove these limits, submit an [Amazon EC2 Request to Remove Email Sending Limitations](https://aws-portal.amazon.com/gp/aws/html-forms-controller/contactus/ec2-email-limit-rdns-request)\. You can also connect to Amazon SES using ports 465 or 587, neither of which is throttled\.
+ **Network errors are causing dropped emails\.**

  Ensure that your application uses retry logic when it connects to the Amazon SES SMTP endpoint, and that your application can detect and retry message delivery in case of a network error\. SMTP is a verbose protocol, and sending an email using this protocol requires several network round trips\. Because of the nature of SMTP, the potential for network errors increases\.
+ **You lose connection with the SMTP endpoint\.**

  Lost connections are most commonly caused by the following issues:
  + **MTU size** – If you receive a time\-out error message, the Maximum Transmission Unit \(MTU\) of the network interface for the computer you're using to connect to the Amazon SES SMTP interface may be too large\. To resolve this issue, set the MTU size on that computer to 1500 bytes\.

    For more information about setting the MTU size on Windows, Linux, and macOS operating systems, see [ Queries Appear to Hang in the Client and Do Not Reach the Cluster](https://docs.aws.amazon.com/redshift/latest/mgmt/connecting-drop-issues.html) in the *Amazon Redshift Cluster Management Guide*\.

    For more information about setting the MTU size for an Amazon EC2 instance, see [ Network Maximum Transmission Unit \(MTU\) for Your EC2 Instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/network_mtu.html) in the *Amazon EC2 User Guide for Linux Instances*\.
  + **Long\-lived connections** – The Amazon SES SMTP endpoint runs on a fleet of Amazon EC2 instances behind an Elastic Load Balancer \(ELB\)\. In order to ensure that the system is up\-to\-date and fault tolerant, active Amazon EC2 instances are periodically terminated and replaced with new instances\. Because your application connects to an Amazon EC2 instance through the ELB, the connection becomes invalid when the Amazon EC2 instance is terminated\. You should establish a new SMTP connection after you have delivered a fixed number of messages via a single SMTP connection, or if the SMTP connection has been active for some amount of time\. You will need to experiment to find appropriate thresholds depending on where your application is hosted and how it submits email to Amazon SES\.
+ **You want to know the IP addresses of the Amazon SES SMTP mail servers so that you can whitelist the IP addresses with your network\.**

  The IP addresses for the Amazon SES SMTP endpoints reside behind load balancers and the IP addresses change frequently\. For this reason, we are not able to provide a definitive list of IP addresses for the Amazon SES endpoints\. We recommend that you only whitelist based on DNS and not static IP addresses\.