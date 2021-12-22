# EC2 Console<a name="launch-from-console"></a>

**Note**  
To launch an instance with Elastic Fabric Adapter \(EFA\), refer to [these steps\.](https://docs.aws.amazon.com/dlami/latest/devguide/tutorial-efa-launching.html) 

1. Open the [EC2 Console](https://console.aws.amazon.com/ec2)\.

1. Note your current region in the top\-most navigation\. If this isn't your desired AWS Region, change this option before proceeding\. For more information, see [EC2 Regions](https://docs.aws.amazon.com/general/latest/gr/rande.html#ec2_region)\.

1. Choose **Launch Instance**\.

1. Search for the desired instance by name:

   1. Select the DLAMI that is right for you\. Find the DLAMI name as listed in the [release notes](https://docs.aws.amazon.com/dlami/latest/devguide/appendix-ami-release-notes) or find the [DLAMI ID](https://docs.aws.amazon.com/dlami/latest/devguide/find-dlami-id) using the AWS CLI\. 

   1. Choose **Community AMIs**\.

      1. To view a selection of the latest DLAMIs, choose **Quick Start\.**

      1. Choose **AWS Marketplace** to browse additional DLAMIs\. Only a subset of available DLAMIs will be listed here\.

   1. Enter the DLAMI name or search the DLAMI ID\. Browse the options and then click **Select** on your choice\.

1. Review the details, and then choose **Continue**\.

1. Choose an instance type\. For recommendations on DLAMI instance types, see [Instance Selection](https://docs.aws.amazon.com/dlami/latest/devguide/instance-select.html)\.
**Note**  
If you want to use [Elastic Inference](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/elastic-inference.html) \(EI\), click **Configure Instance Details**, select **Add an Amazon EI accelerator**, then select the size of the Amazon EI accelerator\.

1. Choose **Review and Launch**\.

1. Review the details and pricing\. Choose **Launch**\.

**Tip**  
Check out [Get Started with Deep Learning Using the AWS Deep Learning AMI](https://aws.amazon.com/blogs/ai/get-started-with-deep-learning-using-the-aws-deep-learning-ami/) for a walk\-through with screenshots\!

**Next Step**  
[Step 2: Connect to the DLAMI](launch-config-connect.md)