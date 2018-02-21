# Step 5: View the Received Email<a name="receiving-email-getting-started-view"></a>

1. In the upper left of the AWS Management Console, choose the AWS icon, and then, under **Storage & Content Delivery**, choose **S3**\.  
![\[\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/white_space_horizontal.png)  
![\[Find the email you received with Amazon SES\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/getting_started_receiving_s3_1.png)

1. In the Amazon S3 console, choose the bucket that you created when you set up Amazon SES email receiving earlier in this tutorial\.  
![\[\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/white_space_horizontal.png)  
![\[Find the email you received with Amazon SES\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/getting_started_receiving_s3_2.png)

1. In the bucket, find the received email\. The name of the email will be an alphanumeric string\. The bucket will also contain an item with the name AMAZON\_SES\_SETUP\_NOTIFICATION, which you can ignore\.  
![\[\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/white_space_horizontal.png)  
![\[Find the email you received with Amazon SES\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/getting_started_receiving_s3_3.png)

1. To download the email to your computer, choose the box to the left of the email, and then choose **Download** from the menu\.  
![\[\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/white_space_horizontal.png)  
![\[Find the email you received with Amazon SES\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/images/getting_started_receiving_s3_4.png)

1. Open the email in a text editor\. The email will be in a raw format, which is typically [Multipurpose Internet Mail Extensions \(MIME\)](http://tools.ietf.org/html/rfc2045)\. To decode MIME, you must use your own application\.

Next step: [Step 6: Clean Up](receiving-email-getting-started-clean.md)